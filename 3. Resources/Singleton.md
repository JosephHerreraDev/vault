---
date: 2025-02-04
---
Ensures a class has only one instance and provides a global point of access to that instance.

# Problem
---
Solves two problems at once that put in danger the [[Single Responsibility Principle]]:
1. Ensures a class has a single instance.
2. Provides a global access point to said instance.

# Solution
---
All implementations of Singleton follow this two common steps:
- Make the default constructor private to prevent other objects from using the `new` operator with the Singleton class. 
- Create a static creation method that acts as the constructor. Behind the scenes, this object calls the private constructor and saves the static field. Following calls to this method return the object stored in cache.
# Structure
---
![Obsidian|500](Singleton%20Diagram.png)
# Applicability
---
- [i] When a class in the program should have just a single instance available to all clients
- [i] Stricter control over global variables is needed.
# Pros and Cons
---
- [p] A class will always have a single instance
- [p] There is a global access point to said instance
- [p] The Singleton object is only initialized when it is required one time  
- [c]  It endangers the [[Single Responsibility Principle]]. The pattern solves two problems at once
- [c] It can result in a bad design, when for example, the components of a program know a lot about each other
- [c] The pattern requires a special treatment in multiple thread environments, so that multiple threads don't create multiple Singleton objects. 
- [c] Can result difficult to make unitary tests, because many frameworks rely on inheritance to build simulated objects (mocks). The Singleton has to be simulated in another way or not use Singleton.
# Relation to other patterns
---
- A [[Facade]] class can often transform into a Singleton, since a single Facade object is enough for most cases.
- [[Flyweight]] can be similar to a Singleton if in some way all the shared states could be reduced to a single flyweight.
	1. There should only be a single instance of Singleton, and flyweight can have many with different states.
	2. The Singleton can be mutable. flyweight are unchangeable.
- The patterns [[Abstract Factory]], [[Builder]] and [[Prototype]] can all be implemented as Singletons.
# Implementation
---
## Example 1: Eager Initialization
---
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

## Example 2: Lazy Initialization with Thread Safety
---
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