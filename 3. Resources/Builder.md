---
date: 2025-02-04
---
Build complex objects step by step, separating the construction from a particular representation of a object.

# Problem
---
Imagine a complex object that requires laborious, step-by-step initialization of many fields and nested objects. Such initialization code is usually buried inside a monstrous constructor with a lot of parameters. Or even worse, scattered all over the client code.

# Solution
---
The builder pattern suggests that you extract the object construction code out of its own class and move it to separate objects called *builders*.

The pattern organizes object construction into a set of steps. To create an object, A series of steps are executed. Not all the steps are called, only the ones necessary for producing a particular configuration of an object.

**Director**
A series of calls can be extracted to the builder steps to construct a product into a *director*. It defines the order in which to execute the building steps.
It isn't necessary, however, the director class might be a good place to put various construction routines that can be reused across the program.

# Structure
---
![Obisidian|400](Builder%20Diagram.png)

1. The **Builder** interface declares product construction steps that are common to all types of builders.
2. **Concrete Builders** provide different implementations of the construction steps. Concrete builders may produce products that don’t follow the common interface.
3. **Products** are resulting objects. Products constructed by different builders don’t have to belong to the same class hierarchy or interface.
4. The **Director** class defines the order in which to call construction steps, so you can create and reuse specific configurations of products.
5. The **Client** must associate one of the builder objects with the director. Usually, it’s done just once, via parameters of the director’s constructor. Then the director uses that builder object for all further construction. However, there’s an alternative approach for when the client passes the builder object to the production method of the director. In this case, you can use a different builder each time you produce something with the director.

# Applicability
---
- [i] To get rid of a "telescoping constructor".
- [i] When is needed that the code represents different representations of some product.
- [i] To construct [[Composite]] trees or other complex objects.

# Pros and Cons
---
- [p] Objects can be constructed step-by-step, defer construction steps or run steps recursively.
- [p] The same construction code can be reused when building various representations of products
- [p] [[Single Responsibility Principle]]. Complex construction code can be isolated from the business logic of the product.
- [c] The overall complexity of the code increases since the pattern requires creating multiple new classes.

# Relation to other patterns
---
- Many designs start by using [[Factory Method]] (less complicated and more customizable via subclasses) and evolve toward [[Abstract Factory]], [[Prototype]], or Builder (more flexible, but more complicated).
- Builder focuses on constructing complex objects step by step. [[Abstract Factory]] specializes in creating families of related objects. _Abstract Factory_ returns the product immediately, whereas _Builder_ lets you run some additional construction steps before fetching the product.
- You can use Builder when creating complex [[Composite]] trees because you can program its construction steps to work recursively.
- You can combine Builder with [[Bridge]]: the director class plays the role of the abstraction, while different builders act as implementations.
- [[Abstract Factory]], Builder and [[Prototype]] can all be implemented as [[Singleton]].

# Implementation
---
```csharp
using System;
using System.Collections.Generic;

// The builder interface specifies methods for creating the different parts of the Product objects
public interface IBuilder
{
	void BuildPartA();
	void BuildPartB();
	void BuildPartC();
}
//The concrete Builder classes follow the builder interface and provide //specific implementations of the building steps. 
//The program may have several variations of builders, implemented differently
public class ConcreteBuilder
{
	private Product _product = new Product();

	//A fresh builder instance should contain a blank product object,
	//which is used in further aseembly.
	public ConcreteBuilder()
	{
		this.Reset();
	}

	public void Reset()
	{
		this._product = new Product();
	}

	// All production steps work with the same product instance.
	public void BuildPartA()
	{
		this._product.Add("PartA1");
	}

	public void BuildPartB()
	{
		this._product.Add("PartB1");
	}
	
	public void BuildPartC()
	{
		this._product.Add("PartC1");
	}

	/* Concrete Builders are supposed to provide their own methods for
	   retrieving results. That's because various types of builders may
	   create entirely different products that don't follow the same
	   interface. Therefore, such methods cannot be declared in the base
	   Builder interface (at least in a statically typed programming
	   language). 
	*/
	
	/* Usually, after returning the end result to the client, a builder
	   instance is expected to be ready to start producing another product.
	   That's why it's a usual practice to call the reset method at the end
	   of the `GetProduct` method body. However, this behavior is not
	   mandatory, and you can make your builders wait for an explicit reset
	   call from the client code before disposing of the previous result. 
	*/
	
	public Product GetProduct()
	{
		Product result = this._product;

		this.Reset();

		return result;
	}
}

/* It makes sense to use the Builder pattern only when your products are
   quite complex and require extensive configuration.
*/
/* Unlike in other creational patterns, different concrete builders can
   produce unrelated products. In other words, results of various builders
   may not always follow the same interface.
*/
public class Product
{
	private List<object> _parts = new List<object>();

	public void Add(string part)
	{
		this._parts.Add(part);
	}

	public string ListParts()
	{
		string str = string.Empty;

		for (int i = 0; i < this._parts.Count; i++)
		{
			str += this._parts[i] + ", ";
		}

		str = str.Remove(str.Length - 2); // removing last ",c"
		return "Product parts: " + str + "\n";
	}
}

// The Director is only responsible for executing the building steps in a
    // particular sequence. It is helpful when producing products according to a
    // specific order or configuration. Strictly speaking, the Director class is
    // optional, since the client can control builders directly.
public class Director
{
	private IBuilder _builder;

	public IBuilder Builder
	{
		set { _builder = value; }
	}

	// The Director can construct several product variations using the same building steps
	public void BuildMinimalViableProduct()
	{
		this._builder.BuildPartA();
	}

	public void BuildFullFeaturedProduct()
	{
		this._builder.BuildPartA();
		this._builder.BuildPartB();
		this._builder.BuildPartC();
	}
}

class Program
{
	static void Main(string[] args)
	{
		// The client code creates a builder object, passes it to the director and then
		//initiates the construction process. The end result is retrived from the builder
		//object.
		var director = new Director();
		var builder = new ConcreteBuilder();
		director.Builder = builder;
		Console.WriteLine("Standard basic product:");
		director.BuildMinimalViableProduct();
		Console.WriteLine(builder.GetProduct().ListParts());

		Console.WriteLine("Standard full featured product:");
		director.BuildFullFeaturedProduct();
		Console.WriteLine(builder.GetProduct().ListParts());

		// Remember, the Builder pattern can be used without a Director class.
		Console.WriteLine("Custom product:");
		builder.BuildPartA();
		builder.BuildPartC();
		Console.Write(builder.GetProduct().ListParts());
	}
}

```