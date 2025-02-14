---
date: 2025-02-11T12:18
tags: []
---
Turns a request into a stand-alone object that contains all information about the request. This transformation lets pass requests as a method arguments, delay or queue a request's execution, and support undoable operations.

# Problem

If there is a button class from which there is a button subclass that inherits from it and changes the functionality respectively, it brings problems because all the subclasses are dependent.
# Solution

Good software design is often based on the [[Principle of Separation of Concerns]], which usually results in breaking an app into layers. The most common example: a layer for the graphical user interface and another layer for the business logic. The GUI layer is responsible for rendering a beautiful picture on the screen, capturing any input and showing results of what the user and the app are doing. However, when it comes to doing something important, like calculating the trajectory of the moon or composing an annual report, the GUI layer delegates the work to the underlying layer of business logic.

In the code it might look like this: a GUI object calls a method of a business logic object, passing it some arguments. This process is usually described as one object sending another a _request_.

The Command pattern suggests that GUI objects shouldn’t send these requests directly. Instead, you should extract all of the request details, such as the object being called, the name of the method and the list of arguments into a separate _command_ class with a single method that triggers this request.

Command objects serve as links between various GUI and business logic objects. From now on, the GUI object doesn’t need to know what business logic object will receive the request and how it’ll be processed. The GUI object just triggers the command, which handles all the details.

The next step is to make your commands implement the same interface. Usually it has just a single execution method that takes no parameters. This interface lets you use various commands with the same request sender, without coupling it to concrete classes of commands. As a bonus, now you can switch command objects linked to the sender, effectively changing the sender’s behavior at runtime.

You might have noticed one missing piece of the puzzle, which is the request parameters. A GUI object might have supplied the business-layer object with some parameters. Since the command execution method doesn’t have any parameters, how would we pass the request details to the receiver? It turns out the command should be either pre-configured with this data, or capable of getting it on its own.

# Structure

![[Command Diagram.png]]
1. The **Sender** class (aka _invoker_) is responsible for initiating requests. This class must have a field for storing a reference to a command object. The sender triggers that command instead of sending the request directly to the receiver. Note that the sender isn’t responsible for creating the command object. Usually, it gets a pre-created command from the client via the constructor.
2. The **Command** interface usually declares just a single method for executing the command.
3. **Concrete Commands** implement various kinds of requests. A concrete command isn’t supposed to perform the work on its own, but rather to pass the call to one of the business logic objects. However, for the sake of simplifying the code, these classes can be merged.
    Parameters required to execute a method on a receiving object can be declared as  fields in the concrete command. You can make command objects immutable by only allowing the initialization of these fields via the constructor.
4. The **Receiver** class contains some business logic. Almost any object may act as a receiver. Most commands only handle the details of how a request is passed to the receiver, while the receiver itself does the actual work.
5. The **Client** creates and configures concrete command objects. The client must pass all of the request parameters, including a receiver instance, into the command’s constructor. After that, the resulting command may be associated with one or multiple senders.

# Applicability

- [i] Parametrize objects with operations
- [i] Queue operations, schedule their execution, or execute them remotely
- [i] Implement reversible operations

# Pros and Cons

- [p] [[Single Responsibility Principle]]. Decouple classes that invoke operations from classes that perform these operations.
- [p] [[Open Closed Principle]]. Introduce new commands into the app without breaking existing client code.
- [p] Implement undo/redo
- [p] Implement deferred execution of operations
- [p] Assemble a set of simple commands into a complex one.
- [c] The code may become more complicated since it's introducing a whole new layer between senders and receivers.

# Relations with Other Patterns

- [[Chain of Responsibility]] Command, [[Mediator]] and [[Observer]] address various ways of connecting senders and receivers of requests:
    
    - _Chain of Responsibility_ passes a request sequentially along a dynamic chain of potential receivers until one of them handles it.
    - _Command_ establishes unidirectional connections between senders and receivers.
    - _Mediator_ eliminates direct connections between senders and receivers, forcing them to communicate indirectly via a mediator object.
    - _Observer_ lets receivers dynamically subscribe to and unsubscribe from receiving requests.
- Handlers in [[Chain of Responsibility]] can be implemented as Commands. In this case, you can execute a lot of different operations over the same context object, represented by a request.
    
    However, there’s another approach, where the request itself is a _Command_ object. In this case, you can execute the same operation in a series of different contexts linked into a chain.
    
- You can use Command and [[Memento]] together when implementing “undo”. In this case, commands are responsible for performing various operations over a target object, while mementos save the state of that object just before a command gets executed.
- Command and [[Strategy]] may look similar because you can use both to parameterize an object with some action. However, they have very different intents.
    
    - You can use _Command_ to convert any operation into an object. The operation’s parameters become fields of that object. The conversion lets you defer execution of the operation, queue it, store the history of commands, send commands to remote services, etc.
        
    - On the other hand, _Strategy_ usually describes different ways of doing the same thing, letting you swap these algorithms within a single context class.
        
- [[Prototype]] can help when you need to save copies of Commands into history.
    
- You can treat [[Visitor]] as a powerful version of the Command pattern. Its objects can execute operations over various objects of different classes.

# Implementation

```csharp
using System;

{
    // The Command interface declares a method for executing a command.
    public interface ICommand
    {
        void Execute();
    }

    // Some commands can implement simple operations on their own.
    class SimpleCommand : ICommand
    {
        private string _payload = string.Empty;

        public SimpleCommand(string payload)
        {
            this._payload = payload;
        }

        public void Execute()
        {
            Console.WriteLine($"SimpleCommand: See, I can do simple things like printing ({this._payload})");
        }
    }

    // However, some commands can delegate more complex operations to other
    // objects, called "receivers."
    class ComplexCommand : ICommand
    {
        private Receiver _receiver;

        // Context data, required for launching the receiver's methods.
        private string _a;

        private string _b;

        // Complex commands can accept one or several receiver objects along
        // with any context data via the constructor.
        public ComplexCommand(Receiver receiver, string a, string b)
        {
            this._receiver = receiver;
            this._a = a;
            this._b = b;
        }

        // Commands can delegate to any methods of a receiver.
        public void Execute()
        {
            Console.WriteLine("ComplexCommand: Complex stuff should be done by a receiver object.");
            this._receiver.DoSomething(this._a);
            this._receiver.DoSomethingElse(this._b);
        }
    }

    // The Receiver classes contain some important business logic. They know how
    // to perform all kinds of operations, associated with carrying out a
    // request. In fact, any class may serve as a Receiver.
    class Receiver
    {
        public void DoSomething(string a)
        {
            Console.WriteLine($"Receiver: Working on ({a}.)");
        }

        public void DoSomethingElse(string b)
        {
            Console.WriteLine($"Receiver: Also working on ({b}.)");
        }
    }

    // The Invoker is associated with one or several commands. It sends a
    // request to the command.
    class Invoker
    {
        private ICommand _onStart;

        private ICommand _onFinish;

        // Initialize commands.
        public void SetOnStart(ICommand command)
        {
            this._onStart = command;
        }

        public void SetOnFinish(ICommand command)
        {
            this._onFinish = command;
        }

        // The Invoker does not depend on concrete command or receiver classes.
        // The Invoker passes a request to a receiver indirectly, by executing a
        // command.
        public void DoSomethingImportant()
        {
            Console.WriteLine("Invoker: Does anybody want something done before I begin?");
            if (this._onStart is ICommand)
            {
                this._onStart.Execute();
            }
            
            Console.WriteLine("Invoker: ...doing something really important...");
            
            Console.WriteLine("Invoker: Does anybody want something done after I finish?");
            if (this._onFinish is ICommand)
            {
                this._onFinish.Execute();
            }
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            // The client code can parameterize an invoker with any commands.
            Invoker invoker = new Invoker();
            invoker.SetOnStart(new SimpleCommand("Say Hi!"));
            Receiver receiver = new Receiver();
            invoker.SetOnFinish(new ComplexCommand(receiver, "Send email", "Save report"));

            invoker.DoSomethingImportant();
        }
    }
}
```