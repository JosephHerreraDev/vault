---
date: 2025-02-04
tags:
  - design-patterns
  - csharp
  - study
---
Ensures a class has only one instance and provides a global point of access to that instance.

## When to use?
- When you need a single resource to the application

## Implementation

### Example 1: Eager Initialization

```csharp
using System;

public sealed class EagerSingleton
{
    // Private static readonly instance that is created eagerly.
    private static readonly EagerSingleton _instance = new EagerSingleton();

    // Private constructor to prevent instantiation from outside.
    private EagerSingleton() 
    {
        Console.WriteLine("EagerSingleton instance created.");
    }

    // Global access point for the singleton instance.
    public static EagerSingleton Instance
    {
        get { return _instance; }
    }

    public void DoSomething()
    {
        Console.WriteLine("EagerSingleton is doing something.");
    }
}

class Program
{
    static void Main(string[] args)
    {
        // Access the singleton
        EagerSingleton singleton = EagerSingleton.Instance;
        singleton.DoSomething();
    }
}

```

### Example 2: Lazy Initialization with Thread Safety

```csharp
using System;

public sealed class LazySingleton
{
    // The Lazy<T> ensures that the instance is created in a thread-safe manner
    // and it is created only when it is first used.
    private static readonly Lazy<LazySingleton> _lazyInstance =
        new Lazy<LazySingleton>(() => new LazySingleton());

    // Private constructor to prevent instantiation from outside.
    private LazySingleton() 
    {
        Console.WriteLine("LazySingleton instance created.");
    }

    // Global access point for the singleton instance.
    public static LazySingleton Instance
    {
        get { return _lazyInstance.Value; }
    }

    public void DoSomething()
    {
        Console.WriteLine("LazySingleton is doing something.");
    }
}

class Program
{
    static void Main(string[] args)
    {
        // The singleton instance is created only when accessed for the first time.
        LazySingleton singleton = LazySingleton.Instance;
        singleton.DoSomething();
    }
}
```