---
date: 2025-02-11
tags: []
---
Provides a simplified interface to a library, a framework, or any other complex set of classes.

# Problem

Imagine that you must make your code work with a broad set of objects that belong to a sophisticated library or framework. Ordinarily, you’d need to initialize all of those objects, keep track of dependencies, execute methods in the correct order, and so on.

As a result, the business logic of your classes would become tightly coupled to the implementation details of 3rd-party classes, making it hard to comprehend and maintain.

# Solution

A facade is a class that provides a simple interface to a complex subsystem which contains lots of moving parts. A facade might provide limited functionality in comparison to working with the subsystem directly. However, it includes only those features that clients really care about.

Having a facade is handy when you need to integrate your app with a sophisticated library that has dozens of features, but you just need a tiny bit of its functionality.

# Structure

![[Facade Diagram.png]]
1. The **Facade** provides convenient access to a particular part of the subsystem’s functionality. It knows where to direct the client’s request and how to operate all the moving parts.
2. An **Additional Facade** class can be created to prevent polluting a single facade with unrelated features that might make it yet another complex structure. Additional facades can be used by both clients and other facades.
3. The **Complex Subsystem** consists of dozens of various objects. To make them all do something meaningful, you have to dive deep into the subsystem’s implementation details, such as initializing objects in the correct order and supplying them with data in the proper format.
    Subsystem classes aren’t aware of the facade’s existence. They operate  within the system and work with each other directly.
4. The **Client** uses the facade instead of calling the subsystem objects directly.

# Applicability

- [i] When is needed to have a limited, but straightforward interface to a complex subsystem.
- [i] When a subsystem has to be structured into layers.

# Pros and Cons

- [p] Code can be isolated from the complexity of a subsystem.
- [c] A facade can become a god object coupled to all classes of an app.

# Relations with Other Patterns

- **Facade** defines a new interface for existing objects, whereas [[Adapter]] tries to make the existing interface usable. _Adapter_ usually wraps just one object, while _Facade_ works with an entire subsystem of objects.
- [[Abstract Factory]] can serve as an alternative to Facade when you only want to hide the way the subsystem objects are created from the client code.
- [[Flyweight]] shows how to make lots of little objects, whereas **Facade** shows how to make a single object that represents an entire subsystem.
- Facade and [[Mediator]] have similar jobs: they try to organize collaboration between lots of tightly coupled classes.
    - _Facade_ defines a simplified interface to a subsystem of objects, but it doesn’t introduce any new functionality. The subsystem itself is unaware of the facade. Objects within the subsystem can communicate directly.
    - _Mediator_ centralizes communication between components of the system. The components only know about the mediator object and don’t communicate directly.
- A **Facade** class can often be transformed into a [[Singleton]] since a single facade object is sufficient in most cases.
- **Facade** is similar to [[Proxy]] in that both buffer a complex entity and initialize it on its own. Unlike _Facade_, _Proxy_ has the same interface as its service object, which makes them interchangeable.

# Implementation

```csharp
using System;


{
    // The Facade class provides a simple interface to the complex logic of one
    // or several subsystems. The Facade delegates the client requests to the
    // appropriate objects within the subsystem. The Facade is also responsible
    // for managing their lifecycle. All of this shields the client from the
    // undesired complexity of the subsystem.
    public class Facade
    {
        protected Subsystem1 _subsystem1;
        
        protected Subsystem2 _subsystem2;

        public Facade(Subsystem1 subsystem1, Subsystem2 subsystem2)
        {
            this._subsystem1 = subsystem1;
            this._subsystem2 = subsystem2;
        }
        
        // The Facade's methods are convenient shortcuts to the sophisticated
        // functionality of the subsystems. However, clients get only to a
        // fraction of a subsystem's capabilities.
        public string Operation()
        {
            string result = "Facade initializes subsystems:\n";
            result += this._subsystem1.operation1();
            result += this._subsystem2.operation1();
            result += "Facade orders subsystems to perform the action:\n";
            result += this._subsystem1.operationN();
            result += this._subsystem2.operationZ();
            return result;
        }
    }
    
    // The Subsystem can accept requests either from the facade or client
    // directly. In any case, to the Subsystem, the Facade is yet another
    // client, and it's not a part of the Subsystem.
    public class Subsystem1
    {
        public string operation1()
        {
            return "Subsystem1: Ready!\n";
        }

        public string operationN()
        {
            return "Subsystem1: Go!\n";
        }
    }
    
    // Some facades can work with multiple subsystems at the same time.
    public class Subsystem2
    {
        public string operation1()
        {
            return "Subsystem2: Get ready!\n";
        }

        public string operationZ()
        {
            return "Subsystem2: Fire!\n";
        }
    }


    class Client
    {
        // The client code works with complex subsystems through a simple
        // interface provided by the Facade. When a facade manages the lifecycle
        // of the subsystem, the client might not even know about the existence
        // of the subsystem. This approach lets you keep the complexity under
        // control.
        public static void ClientCode(Facade facade)
        {
            Console.Write(facade.Operation());
        }
    }
    
    class Program
    {
        static void Main(string[] args)
        {
            // The client code may have some of the subsystem's objects already
            // created. In this case, it might be worthwhile to initialize the
            // Facade with these objects instead of letting the Facade create
            // new instances.
            Subsystem1 subsystem1 = new Subsystem1();
            Subsystem2 subsystem2 = new Subsystem2();
            Facade facade = new Facade(subsystem1, subsystem2);
            Client.ClientCode(facade);
        }
    }
}
```