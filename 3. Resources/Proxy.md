---
date: 2025-02-11T12:18
tags: []
---
Provides a substitute or placeholder for another object. A proxy controls access to the original object, allowing to perform something either before or after the request gets through to the original object.

# Problem

Why would you want to control access to an object? Here is an example: you have a massive object that consumes a vast amount of system resources. You need it from time to time, but not always.

You could implement lazy initialization: create this object only when it’s actually needed. All of the object’s clients would need to execute some deferred initialization code. Unfortunately, this would probably cause a lot of code duplication.

In an ideal world, we’d want to put this code directly into our object’s class, but that isn’t always possible. For instance, the class may be part of a closed 3rd-party library.
# Solution

The Proxy pattern suggests that you create a new proxy class with the same interface as an original service object. Then you update your app so that it passes the proxy object to all of the original object’s clients. Upon receiving a request from a client, the proxy creates a real service object and delegates all the work to it.

But what’s the benefit? If you need to execute something either before or after the primary logic of the class, the proxy lets you do this without changing that class. Since the proxy implements the same interface as the original class, it can be passed to any client that expects a real service object.

# Structure

![[Proxy Diagram.png]]

1. The **Service Interface** declares the interface of the Service. The proxy must follow this interface to be able to disguise itself as a service object.
2. The **Service** is a class that provides some useful business logic.
3. The **Proxy** class has a reference field that points to a service object. After the proxy finishes its processing (e.g., lazy initialization, logging, access control, caching, etc.), it passes the request to the service object.
    Usually, proxies manage the full lifecycle of their service objects.
4. The **Client** should work with both services and proxies via the same interface. This way you can pass a proxy into any code that expects a service object.

# Applicability

- [i] Lazy initialization. When there is a heavyweight service object that wastes system resources by being always up, even though it's only needed once.
- [i] Access control. When the service object has to be used only by specific clients.
- [i] Local execution of a remote service. When the service object is located on a remote server.
- [i] Logging requests (logging proxy). When a history of requests to the service object is want to be kept.
- [i] Caching request results (caching proxy). When the cache results of client requests is needed and manage the life cycle of this cache, specially if results are quite large.
- [i] Smart reference. When is needed to dismiss a heavyweight object once there are no clients that use it.

# Pros and Cons

- [p] The service object can be controlled without the clients knowing about it.
- [p] The lifecycle of the service object can be managed when clients don't care about it.
- [p] The proxy works even if the service object isn't ready or is not available.
- [p] [[Open Closed Principle]]. New proxies can be introduced without changing the service or clients.
- [c] The code may become more complicated since a lot of new classes need to be introduced.
- [c] The response from the service might get delayed.

# Relations with Other Patterns

- With [[Adapter]] you access an existing object via different interface. With Proxy, the interface stays the same. With [[Decorator]] you access the object via an enhanced interface.
- [[Facade]] is similar to Proxy in that both buffer a complex entity and initialize it on its own. Unlike _Facade_, _Proxy_ has the same interface as its service object, which makes them interchangeable.
- [[Decorator]] and Proxy have similar structures, but very different intents. Both patterns are built on the composition principle, where one object is supposed to delegate some of the work to another. The difference is that a _Proxy_ usually manages the life cycle of its service object on its own, whereas the composition of _Decorators_ is always controlled by the client.

# Implementation

```csharp
using System;

namespace RefactoringGuru.DesignPatterns.Proxy.Conceptual
{
    // The Subject interface declares common operations for both RealSubject and
    // the Proxy. As long as the client works with RealSubject using this
    // interface, you'll be able to pass it a proxy instead of a real subject.
    public interface ISubject
    {
        void Request();
    }
    
    // The RealSubject contains some core business logic. Usually, RealSubjects
    // are capable of doing some useful work which may also be very slow or
    // sensitive - e.g. correcting input data. A Proxy can solve these issues
    // without any changes to the RealSubject's code.
    class RealSubject : ISubject
    {
        public void Request()
        {
            Console.WriteLine("RealSubject: Handling Request.");
        }
    }
    
    // The Proxy has an interface identical to the RealSubject.
    class Proxy : ISubject
    {
        private RealSubject _realSubject;
        
        public Proxy(RealSubject realSubject)
        {
            this._realSubject = realSubject;
        }
        
        // The most common applications of the Proxy pattern are lazy loading,
        // caching, controlling the access, logging, etc. A Proxy can perform
        // one of these things and then, depending on the result, pass the
        // execution to the same method in a linked RealSubject object.
        public void Request()
        {
            if (this.CheckAccess())
            {
                this._realSubject.Request();

                this.LogAccess();
            }
        }
        
        public bool CheckAccess()
        {
            // Some real checks should go here.
            Console.WriteLine("Proxy: Checking access prior to firing a real request.");

            return true;
        }
        
        public void LogAccess()
        {
            Console.WriteLine("Proxy: Logging the time of request.");
        }
    }
    
    public class Client
    {
        // The client code is supposed to work with all objects (both subjects
        // and proxies) via the Subject interface in order to support both real
        // subjects and proxies. In real life, however, clients mostly work with
        // their real subjects directly. In this case, to implement the pattern
        // more easily, you can extend your proxy from the real subject's class.
        public void ClientCode(ISubject subject)
        {
            // ...
            
            subject.Request();
            
            // ...
        }
    }
    
    class Program
    {
        static void Main(string[] args)
        {
            Client client = new Client();
            
            Console.WriteLine("Client: Executing the client code with a real subject:");
            RealSubject realSubject = new RealSubject();
            client.ClientCode(realSubject);

            Console.WriteLine();

            Console.WriteLine("Client: Executing the same client code with a proxy:");
            Proxy proxy = new Proxy(realSubject);
            client.ClientCode(proxy);
        }
    }
}
```