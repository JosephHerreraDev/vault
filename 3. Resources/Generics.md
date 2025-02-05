---
date: 2025-02-04
tags:
  - csharp
  - study
source: "[[Videos#Advanced Topics in C Sharp]]"
---
Is a feature to create classes, methods and data structures that work with any type.
### Generics in classes

```csharp
public class Box<T> // Generic class with placeholder type T
{
    private T _value; // T represents any data type

    public void Add(T value) // Set value
    {
        _value = value;
    }

    public T Get() // Retrive it
    {
        return _value;
    }
}
```

Usage

```csharp
var intBox = new Box<int>();
intBox.Add(123);
Console.WriteLine(intBox.Get()); // Output: 123

var stringBox = new Box<string>();
stringBox.Add("Hello, Generics!");
Console.WriteLine(stringBox.Get()); // Output: Hello, Generics!
```

### Generics in methods

```csharp
public class Printer
{
    public void Print<T>(T item) // Can receive any data type
    {
        Console.WriteLine(item.ToString());
    }
}

```

```csharp
var printer = new Printer();
printer.Print(42);          // Output: 42
printer.Print("Generics");   // Output: Generics
printer.Print(3.14);         // Output: 3.14
```

### Generic Constrains

Sometimes you need to restrict the types used with generics. 

```csharp
// Constrained to types that implement the IEntity interface, meaning T must have an Id property
public class Repository<T> where T : IEntity 
{
    private List<T> _items = new List<T>();

    public void Add(T item)
    {
        _items.Add(item);
    }

    public T Get(int id)
    {
        return _items.FirstOrDefault(i => i.Id == id); // So this works
    }
}

```
