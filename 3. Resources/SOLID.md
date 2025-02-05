---
date: 2025-02-04
tags:
  - csharp
  - study
source: "[[Videos#Advanced Topics in C Sharp]]"
---
### Single Responsibility Principle

>[!note]
"A class should have only one reason to change"

Each class should focus on only one thing and doing it well. A "responsibility" generally refers to a reason to change. 

#### Why?
1. Easier to understand
2. Improved Maintainability
3. Reduced Coupling and Dependency
4. Better Reusability

#### Example of multiple responsibilities

```csharp
public class Report
{
    public string Title { get; set; }
    public string Content { get; set; }

    public void GenerateReport()
    {
        // Generate the report content
        Console.WriteLine("Generating report...");
    }

    public void SaveToFile()
    {
        // Save the report to a file
        Console.WriteLine("Saving report to file...");
    }

    public void SendReportByEmail(string email)
    {
        // Send the report via email
        Console.WriteLine($"Sending report to {email}...");
    }
}
```

Report has multiple responsibilities, each of these actions could independently change over time. 

#### Applying SRP

We can refactor the `Report` class by breaking it into smaller classes, each with a single responsibility. Separating the different actions:

1. **Report Generation**: This class will handle only the report creation process.
2. **File Saving**: This class will be responsible for saving reports to files.
3. **Email Sending**: This class will handle sending reports via email.

**Refactored Code**

```csharp
public class Report
{
    public string Title { get; set; }
    public string Content { get; set; }

    public void GenerateReport()
    {
        Console.WriteLine("Generating report...");
        // Logic for generating the report
    }
}

public class FileSaver
{
    public void SaveToFile(Report report)
    {
        Console.WriteLine("Saving report to file...");
        // Logic for saving the report to a file
    }
}

public class EmailSender
{
    public void SendReportByEmail(Report report, string email)
    {
        Console.WriteLine($"Sending report to {email}...");
        // Logic for sending the report via email
    }
}
```

#### Common Misconceptions About SRP

- **SRP is not about small classes**: It’s about cohesive classes with single, well-defined responsibilities. Large classes can adhere to SRP if their methods contribute to one responsibility.
- **SRP doesn’t mean minimal classes**: SRP can lead to a reasonable number of classes, but it doesn’t mean every method should be in a separate class. Aim for logical grouping of responsibilities.

### Open/Closed Principle

> [!note]
> "Software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification."

You should be able to add new functionality to a system by extending existing code without altering or modifying it. 

#### Why?

4. Stability of Existing Code
5. Flexibility for Future Requirements
6. Improved Maintainability

#### Implementing OCP in Object-Oriented Design
Is often implemented using **polymorphism** and **abstraction**. Key techniques include:

- **Inheritance**: By inheriting and extending base classes, you can introduce new behavior without modifying existing code.
- **Interfaces and Abstract Classes**: Interfaces or abstract classes define a contract, allowing new classes to implement or extend this contract with unique behavior.

#### <font color=FF0000>Violating OCP</font>

```csharp
public class Circle
{
    public double Radius { get; set; }
}

public class Rectangle
{
    public double Width { get; set; }
    public double Height { get; set; }
}

public class AreaCalculator
{
    public double CalculateArea(object shape)
    {
        if (shape is Circle)
        {
            Circle circle = (Circle)shape;
            return Math.PI * circle.Radius * circle.Radius;
        }
        else if (shape is Rectangle)
        {
            Rectangle rectangle = (Rectangle)shape;
            return rectangle.Width * rectangle.Height;
        }

        throw new ArgumentException("Unknown shape");
    }
}

```


#### Applying OCP

```csharp
public interface IShape
{
    double CalculateArea();
}

public class Circle : IShape
{
    public double Radius { get; set; }

    public double CalculateArea()
    {
        return Math.PI * Radius * Radius;
    }
}

public class Rectangle : IShape
{
    public double Width { get; set; }
    public double Height { get; set; }

    public double CalculateArea()
    {
        return Width * Height;
    }
}

public class AreaCalculator
{
    public double CalculateArea(IShape shape)
    {
        return shape.CalculateArea();
    }
}

```

**Adding a new shape**

```csharp
public class Triangle : IShape
{
    public double Base { get; set; }
    public double Height { get; set; }

    public double CalculateArea()
    {
        return 0.5 * Base * Height;
    }
}
```

#### Base class with no modifications

```csharp
public class Program
{
    public static void Main()
    {
        AreaCalculator calculator = new AreaCalculator();

        IShape circle = new Circle { Radius = 5 };
        IShape rectangle = new Rectangle { Width = 4, Height = 6 };
        IShape triangle = new Triangle { Base = 3, Height = 4 };

        Console.WriteLine(calculator.CalculateArea(circle));
        Console.WriteLine(calculator.CalculateArea(rectangle));
        Console.WriteLine(calculator.CalculateArea(triangle));
    }
}
```

#### Common Misconceptions about OCP

7. OCP doesn't mean no changes ever
8. OCP is not just for large systems
9. Overengineering

### Liskov Substitution Principle

> [!note]
> "Objects of a superclass should be replaceable with objects of a subclass without affecting the functionality of the program."

In simpler terms, if a class `B` is a subclass of class `A`, then objects of class `A` should be replaceable with objects of class `B` without altering the correct behavior of the program.
#### Identifying LSP Violations

LSP violations typically occur when a subclass changes the behavior of a base class in ways that are unexpected, contradictory, or problematic. Common signs of LSP violations include:

- **Modifying methods** in ways that restrict or alter the expected behavior of the base class.
- **Throwing exceptions** in subclasses where the base class would not.
- **Adding preconditions** (more requirements to use a method) or **weakening postconditions** (failing to meet base class guarantees).

####  <font color=FF0000>Bad implementation of LSP</font>

```csharp
public class Employee
{
	public string name {get; set;}
	public int age {get; set;}

	public void Fire()
	{
		Console.WriteLine("Sorry bro")
	}
}
```

```csharp
public class Ceo : Employee
{
	public void Fire()
	{
		throw new NotImplementedException("Can't fire me");
	}
	
	public void PermormanceReview()
	{
		throw new NotImplementedException("Generating performance review...");
	}
}
```

```csharp
Employee employee = new Ceo();

employee.Fire(); // Can't fire a Ceo
```

#### Correcting the Violation

```csharp
public class IEmployee
{
	string name;
	int age;
}
```

```csharp
public abstract class BaseEmployee
{
	public string name {get; set;}
	public int age {get; set;}
}
```

```csharp
public class Employee : BaseEmployee
{
	public void Fire()
	{
		Console.WriteLine("Sorry bro")
	}
}
```

```csharp
public class Ceo : BaseEmployee
{
	public void PermormanceReview()
	{
		throw new NotImplementedException("Generating performance review...");
	}
}
```

```csharp
Employee employee = new Ceo();

employee.Fire(); // Can't fire a Ceo
```


### Interface Segregation Principle

> [!note]
> "A client should not be forced to depend on methods it does not use."

Interfaces should be specific to the needs of the client. Following ISP keeps interfaces concise and prevents “fat” interfaces that force classes to implement unnecessary methods.

#### Why?

10. Decoupling
11. Flexibility and Extensibility
12. Improved Maintainability

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

13. **ISP is Not Just About Splitting Interfaces**
14. **Over-Segregation Can Lead to Complexity**
15. **ISP is Beneficial in All Sizes of Projects**

### Dependency Inversion Principle

> [!note]
> **"High-level modules should not depend on low-level modules. Both should depend on abstractions."**
> **"Abstractions should not depend on details. Details should depend on abstractions."**

#### Goals of Dependency Inversion Principle

16. Reduce Coupling
17. Increase Flexibility
18. Enhance Testability

#### DIP Violation

```csharp
public class PayPalGateway
{
    public void ProcessPayPalPayment(decimal amount)
    {
        Console.WriteLine($"Processing payment of {amount} through PayPal.");
    }
}

public class PaymentProcessor
{
    private PayPalGateway _payPalGateway = new PayPalGateway();

    public void ProcessPayment(decimal amount)
    {
        _payPalGateway.ProcessPayPalPayment(amount);
    }
}

```

#### Problems

- **Tight Coupling**: If the store wants to switch from PayPal to another gateway, the `PaymentProcessor` class has to be modified, as it’s directly coupled to `PayPalGateway`.
- **Limited Flexibility**: Adding another payment method (e.g., `Stripe`) would require additional logic and dependencies in `PaymentProcessor`, leading to potentially unwieldy and difficult-to-maintain code.
- **Difficult Testing**: Testing `PaymentProcessor` becomes challenging because it requires a real `PayPalGateway` object, making tests dependent on external resources.

#### Applying Dependency Inversion Principle

**Define abstraction**

```csharp
public interface IPaymentGateway
{
    void ProcessPayment(decimal amount);
}
```

**Implement interface for Each Payment Gateway**

```csharp
public class PayPalGateway : IPaymentGateway
{
    public void ProcessPayment(decimal amount)
    {
        Console.WriteLine($"Processing payment of {amount} through PayPal.");
    }
}

public class StripeGateway : IPaymentGateway
{
    public void ProcessPayment(decimal amount)
    {
        Console.WriteLine($"Processing payment of {amount} through Stripe.");
    }
}
```

**Modify `PaymentProcessor` to Depend on the Abstraction**

```csharp
public class PaymentProcessor
{
    private readonly IPaymentGateway _paymentGateway;

    // Dependency Injection through constructor
    public PaymentProcessor(IPaymentGateway paymentGateway)
    {
        _paymentGateway = paymentGateway;
    }

    public void ProcessPayment(decimal amount)
    {
        _paymentGateway.ProcessPayment(amount);
    }
}
```

Create the object passing in the object

```csharp
var payPalProcessor = new PaymentProcessor(new PayPalGateway());
payPalProcessor.ProcessPayment(100m);

var stripeProcessor = new PaymentProcessor(new StripeGateway());
stripeProcessor.ProcessPayment(200m);
```

#### Dependency Inversion Principle with Dependency Injection

DIP is often paired with **Dependency Injection (DI)**, a technique that provides an instance of an abstraction (e.g., an interface) rather than letting classes instantiate dependencies themselves.

```csharp
// Example using dependency injection with a DI container (like ASP.NET Core's DI)
public void ConfigureServices(IServiceCollection services)
{
    services.AddTransient<IPaymentGateway, PayPalGateway>(); // or StripeGateway, ApplePayGateway, etc.
    services.AddTransient<PaymentProcessor>();
}
```

This setup lets you switch the payment gateway implementation easily, without changing `PaymentProcessor` code.
