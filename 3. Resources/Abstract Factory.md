---
date: 2025-02-04
---
Provides an interface for creating group of related or dependent objects without specifying their concrete classes. 

# Problem
---
There are different classes, which are available in different variants which are like families. Individual classes should be created that match other objects of the same family. No code change should be made when adding new classes or families.

# Solution
---
First, explicitly declare interfaces for each distinct class of the family. Then, all the classes should follow this interface.
Next, declare an Abstract Factory-an interface with the list of creation methods for all the classes that are part of the family. This methods must return abstract product types represented by the interfaces extracted previously.
For each family variant, a separate factory class based on the `AbstractFactory` is created.

# Structure
---
![[Abstract Factory Diagram.png]]

1. **Abstract Products** declare interfaces for a set of distinct but related products which make up a product family.
2. **Concrete Products** are various implementations of abstract products, grouped by variants. Each abstract product  must be implemented in all given variants.
3. The **Abstract Factory** interface declares a set of methods for creating each of the abstract products.
4. **Concrete Factories** implement creation methods of the abstract factory. Each concrete factory corresponds to a specific variant of products and creates only those product variants.
5. Although concrete factories instantiate concrete products, signatures of their creation methods must return corresponding _abstract_ products. This way the client code that uses a factory doesn’t get coupled to the specific variant of the product it gets from a factory. The **Client** can work with any concrete factory/product variant, as long as it communicates with their objects via abstract interfaces.

# Applicability
---
- [i] Families of related object
- [i] When having a class with a set of [[Factory Method|Factory Methods]] that blur its primary responsibility.

# Pros and Cons
---
- [p] The products coming from the factory are compatible with each other. 
- [p] Tight coupling between concrete products and client code is avoided.
- [p] [[Single Responsibility Principle]]. The product creation can be extracted into one place, making the code easier to support.
- [p] [[Open Closed Principle]]. Introducing new products doesn't break the code.
- [c] The code may become more complicated than it should be, since a lot of new interfaces and classes are introduced with the pattern.

# Relations with Other Patterns
---
- Many designs start by using [[Factory Method]] (less complicated and more customizable via subclasses) and evolve toward Abstract Factory, [[Prototype]], or [[Builder]] (more flexible, but more complicated).
- [[Builder]] focuses on constructing complex objects step by step. Abstract Factory specializes in creating families of related objects. _Abstract Factory_ returns the product immediately, whereas _Builder_ runs some additional construction steps before fetching the product.
- Abstract Factory classes are often based on a set of [[Factory Method|Factory Methods]], but [[Prototype]] to compose the methods on these classes can also be used.
- Abstract Factory can serve as an alternative to [[Facade ]]when the subsystem objects are created from the client code is wanted to be hidden.
- Abstract Factory can be used along with [[Bridge]]. This pairing is useful when some abstractions defined by _Bridge_ can only work with specific implementations. In this case, _Abstract Factory_ can encapsulate these relations and hide the complexity from the client code.
- Abstract Factory, [[Builder]] and [[Prototype]] can all be implemented as [[Singleton]].

# Implementation
---
## Define the Abstract Factory Interface

```csharp
public interface IGUIFactory
{
    IButton CreateButton();
    ICheckbox CreateCheckbox();
}
```

## Create Concrete Factories

```csharp
public class WindowsFactory : IGUIFactory
{
    public IButton CreateButton()
    {
        return new WindowsButton();
    }

    public ICheckbox CreateCheckbox()
    {
        return new WindowsCheckbox();
    }
}

public class MacOSFactory : IGUIFactory
{
    public IButton CreateButton()
    {
        return new MacOSButton();
    }

    public ICheckbox CreateCheckbox()
    {
        return new MacOSCheckbox();
    }
}
```

### Define the abstract product interfaces

```csharp
public interface IButton
{
    void Render();
}

public interface ICheckbox
{
    void Render();
}
```

### Create Concrete Products for Windows

```csharp
public class WindowsButton : IButton
{
    public void Render()
    {
        Console.WriteLine("Rendering a Windows styled button.");
    }
}

public class WindowsCheckbox : ICheckbox
{
    public void Render()
    {
        Console.WriteLine("Rendering a Windows styled checkbox.");
    }
}

```

### Create Concrete Products for macOS

```csharp
public class MacOSButton : IButton
{
    public void Render()
    {
        Console.WriteLine("Rendering a macOS styled button.");
    }
}

public class MacOSCheckbox : ICheckbox
{
    public void Render()
    {
        Console.WriteLine("Rendering a macOS styled checkbox.");
    }
}
```

## Client Code that Uses the Factories

```csharp
public class Application
{
    private readonly IButton _button;
    private readonly ICheckbox _checkbox;

    public Application(IGUIFactory factory)
    {
        _button = factory.CreateButton();
        _checkbox = factory.CreateCheckbox();
    }

    public void Render()
    {
        _button.Render();
        _checkbox.Render();
    }
}

class Program
{
    static void Main(string[] args)
    {
        // Determine which factory to use based on configuration or runtime
        IGUIFactory factory;

        // For demonstration, let's assume the platform is Windows.
        // In a real scenario, you might have a configuration setting or runtime check.
        string os = "Windows";

        if (os == "Windows")
        {
            factory = new WindowsFactory();
        }
        else
        {
            factory = new MacOSFactory();
        }

        Application app = new Application(factory);
        app.Render();

        // Alternatively, if you want to test macOS appearance, uncomment the following:
        // factory = new MacOSFactory();
        // app = new Application(factory);
        // app.Render();
    }
}
```

