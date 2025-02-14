---
date: 2025-02-10
tags: []
---
Allows objects with incompatible interfaces to collaborate.

# Problem
A library or exterior part of the code is incompatible with another one. For example, a library might use JSON and XML is used across the code base. It might be difficult or imposible to change one or the other.

# Solution
An adapter is a special object that converts the interface of one object so that another object can understand it.
Adapters can not only convert data into various formats but can also help objects with different interfaces collaborate.
1. The adapter gets an interface, compatible with one of the existing objects.
2. Using this interface, the existing object can safely call the adapter's methods.
3. Upon receiving a call, the adapter passes the request to the second object, but in a format and order that the second object expects.
# Structure
![[Apadater Diagram.png]]
1. The **Client** is a class that contains the existing business logic of the program.
2. The **Client Interface** describes a protocol that other classes must follow to be able to collaborate with the client code.
3. The **Service** is some useful class (usually 3rd-party or legacy). The client cant's use this class directly because it has an incompatible interface.
4.  The **Adapter** is a class that's able to work with both the client and the service: it implements the client interface, while wrapping the service object. The adapter receives calls from the client via the client interface and translates them into calls to the wrapped service object in a format it can understand.
5. The client code doesn’t get coupled to the concrete adapter class as long as it works with the adapter via the client interface. Thanks to this, you can introduce new types of adapters into the program without breaking the existing client code. This can be useful when the interface of the service class gets changed or replaced: you can just create a new adapter class without changing the client code.

# When to use?
- Interface from existing code of some existing code is not compatible with the rest of the code
- Reuse several existing subclasses that lack some common functionality that can't be added to the superclass.

# Pros and Cons
- [p] [[Single Responsibility Principle]]. Interface or data conversion code can be separated from the primary business logic of the program
- [p] [[Open Closed Principle]]. New types of adapters can be introduced without breaking the existing client code, as long as they work with the adapters through the client interface.
- [c] The overall complexity of the code increases because is needed to introduce a set of new interfaces and classes. Sometimes it's simples just to change the service class so that it matches the rest of the code.
# Relations with Other Patterns
- [[Bridge]] is usually designed up-front. On the other hand Adapter is commonly used with an existing app to make some otherwise-incompatible classes work together nicely.
- Adapter provides a completely different interface for accessing an existing object. On the other hand, with the [[Decorator]] pattern the interface either stays the same or gets extended.
- With Adapter an existing object can be accessed via different interface. With [[Proxy]], the interface stays the same. With [[Decorator]] the object is accessed via an enhanced interface.
- [[Facade]] defines a new interface for existing objects, whereas Adapter tries to make the existing interface usable.
- [[Bridge]], [[State]], [[Strategy]] (and to some degree Adapter)  have very similar structures. Indeed, all of these patterns are based on composition, which is delegating work to other objects. However, they all solve different problems. A pattern isn’t just a recipe for structuring the code in a specific way. It can also communicate to other developers the problem the pattern solves.
# Implementation

```csharp
using System;

{
    // The Target defines the domain-specific interface used by the client code.
    public interface ITarget
    {
        string GetRequest();
    }

    // The Adaptee contains some useful behavior, but its interface is
    // incompatible with the existing client code. The Adaptee needs some
    // adaptation before the client code can use it.
    class Adaptee
    {
        public string GetSpecificRequest()
        {
            return "Specific request.";
        }
    }

    // The Adapter makes the Adaptee's interface compatible with the Target's interface.
    class Adapter : ITarget
    {
        private readonly Adaptee _adaptee;

        public Adapter(Adaptee adaptee)
        {
            this._adaptee = adaptee;
        }

        public string GetRequest()
        {
            return $"This is '{this._adaptee.GetSpecificRequest()}'";
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            Adaptee adaptee = new Adaptee();
            ITarget target = new Adapter(adaptee);

            Console.WriteLine("Adaptee interface is incompatible with the client.");
            Console.WriteLine("But with adapter client can call it's method.");

            Console.WriteLine(target.GetRequest());
        }
    }
}
```