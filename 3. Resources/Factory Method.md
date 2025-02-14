---
date: 2025-02-06
---
Provides an interface to create objects in a superclass while allowing subclasses to alter the type of objects created.
# Problem
---
And app is designed for one specific type of object, which is also tightly coupled. If another one is needed to be added, it will present changes in all the app.

# Solution
---
Replace direct object construction calls (using `new` operator) with calls to a *special* factory method.

# Structure
---

![[Factory Diagram.png]]
1. The **Product** declares the interface, which is common to all objects that can be produced by the creator and its subclasses.
2. **Concrete Products** are different implementations of the product interface.
3. The **Creator** class declares the factory method that returns new product objects. It’s important that the return type of this method matches the product interface.
    The factory method can be declared as `abstract` to force all subclasses to implement their own versions of the method. As an alternative, the base factory method can return some default product type.
    Note, despite its name, product creation is **not** the primary responsibility of the  creator. Usually, the creator class already has some core business logic related to products. The factory method helps to decouple this logic from the concrete product classes. Here is an analogy: a large software development company can have a training department for programmers. However, the primary function of the company as a whole is still writing code, not producing programmers.
4. **Concrete Creators** override the base factory method so it returns a different type of product.
    Note that the factory method doesn’t have to **create** new instances all the time. It can also return existing objects from a cache, an object pool, or another source.

# Applicability
---
- [i] When is not known beforehand the exact types and dependencies of the objects the code should work with.
- [i] Provide users of library or framework with a way to extend its internal components.
- [i] Save system resources by reusing existing objects instead of rebuilding them each time.

# Pros and Cons
---
- [p] Tight coupling is avoided between the creator an the concrete products
- [p] [[Single Responsibility Principle]]. The product creation code can be moved into one place in the program, making the code easier to support. 
- [p]  [[Open Closed Principle]]. New types can be introduced into the program without breaking existing client code.
- [c]  The code may become more complicated since a lot of new subclasses need to be added to implement the pattern. The best case scenario is when the pattern is introduced into an existing hierarchy of creator classes.

# Relation with Other Patterns
---
- Many designs start by using Factory Method and evolve toward [[Abstract Factory]], [[Prototype]], or [[Builder]].
- [[Abstract Factory]] classes are often based on a set of Factory Methods, but [[Prototype]] can also be used to compose the methods on these classes.
- The Factory Method can be used along side the [[Iterator]] to let collection subclasses return different types of iterators that are compatible with the collections.
- [[Prototype]] isn't based on inheritance, so it doesn't have its drawbacks. On the other hand, prototype requieres a complicated initialization of the cloned object. Factory Method is based on inheritance, but doesn't require an initialization step.
- Factory Method is a specialization of [[Template Method]]. At the same time, a Factory Method may serve as a step in a large Template Method.

# Implementation
---
### Define the Product interface
```csharp
public interface IDocument
{
	void Open();
	void Save();
}
```

### Create Concrete Products
```csharp
public class WordDocument : IDocument
{
    public void Open()
    {
        Console.WriteLine("Opening a Word document.");
    }

    public void Save()
    {
        Console.WriteLine("Saving a Word document.");
    }
}

public class PDFDocument : IDocument
{
    public void Open()
    {
        Console.WriteLine("Opening a PDF document.");
    }

    public void Save()
    {
        Console.WriteLine("Saving a PDF document.");
    }
}
```

### Define the Creator Class

```csharp
public abstract class DocumentCreator
{
    // The factory method
    public abstract IDocument CreateDocument();

    // Other methods can use the factory method
    public void OpenDocument()
    {
        IDocument document = CreateDocument();
        document.Open();
    }

    public void SaveDocument()
    {
        IDocument document = CreateDocument();
        document.Save();
    }
}
```

### Create Concrete Creators
```csharp
public class WordDocumentCreator : DocumentCreator
{
    public override IDocument CreateDocument()
    {
        return new WordDocument();
    }
}

public class PDFDocumentCreator : DocumentCreator
{
    public override IDocument CreateDocument()
    {
        return new PDFDocument();
    }
}
```

### Client Code
```csharp
class Program
{
    static void Main(string[] args)
    {
        DocumentCreator creator;

        // Create a Word document
        creator = new WordDocumentCreator();
        creator.OpenDocument();
        creator.SaveDocument();

        // Create a PDF document
        creator = new PDFDocumentCreator();
        creator.OpenDocument();
        creator.SaveDocument();
    }
}
```