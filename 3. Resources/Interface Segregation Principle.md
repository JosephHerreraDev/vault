---
date: 2025-02-10
tags: []
---
> [!note]
> "A client should not be forced to depend on methods it does not use."

Interfaces should be specific to the needs of the client. Following ISP keeps interfaces concise and prevents “fat” interfaces that force classes to implement unnecessary methods.

#### Why?

1. Decoupling
2. Flexibility and Extensibility
3. Improved Maintainability

#### "Fat" Interface

```csharp
public interface IMultiFunctionDevice
{
    void Print(Document doc);
    void Scan(Document doc);
    void Fax(Document doc);
}
```

**Basic class that implements interface, but does not need all functions**

```csharp
public class BasicPrinter : IMultiFunctionDevice
{
    public void Print(Document doc)
    {
        Console.WriteLine("Printing document...");
    }

    public void Scan(Document doc)
    {
        throw new NotImplementedException("Scan not supported by BasicPrinter.");
    }

    public void Fax(Document doc)
    {
        throw new NotImplementedException("Fax not supported by BasicPrinter.");
    }
}
```

#### Applying ISP to Improve the Design

```csharp
public interface IPrinter
{
    void Print(Document doc);
}

public interface IScanner
{
    void Scan(Document doc);
}

public interface IFax
{
    void Fax(Document doc);
}
```

Now they can implement more specific interfaces

```csharp
public class BasicPrinter : IPrinter
{
    public void Print(Document doc)
    {
        Console.WriteLine("Printing document...");
    }
}
```

**It is recommended to do interfaces in between that combine one or more**

```csharp
public interface IPrinterScanner : IPrinter, IScanner
{
}

public interface IFullPrinter : IPrinter, IScanner, IFax
{
}
```

and implement

```csharp
public class AdvancedPrinter : IPrinterScanner
{
    public void Print(Document doc)
    {
        Console.WriteLine("Printing document...");
    }

    public void Scan(Document doc)
    {
        Console.WriteLine("Scanning document...");
    }
}

public class FullFeaturePrinter : IFullPrinter
{
    public void Print(Document doc)
    {
        Console.WriteLine("Printing document...");
    }

    public void Scan(Document doc)
    {
        Console.WriteLine("Scanning document...");
    }

    public void Fax(Document doc)
    {
        Console.WriteLine("Faxing document...");
    }
}
```

#### Common Misconceptions about ISP

1. **ISP is Not Just About Splitting Interfaces**
2. **Over-Segregation Can Lead to Complexity**
3. **ISP is Beneficial in All Sizes of Projects**