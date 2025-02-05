---
date: 2025-02-04
tags:
  - csharp
  - study
source: "[[Videos#Advanced Topics in C Sharp]]"
---
Is a design pattern used to achieve Inversion of Control (IoC) between classes and dependencies. Instead of creating dependencies directly within a class, dependencies are provided from an external source, allowing for **loose coupling** between classes and **greater flexibility** in how dependencies are managed.

In essence, Dependency Injection allows a class to delegate the responsibility of acquiring its dependencies to an external system or mechanism, improving modularity, reusability, and testability.

### Why?
1. Loose Coupling
2. Testability
3. Flexibility
4. Modularity

### Dependency Injection in Detail

```csharp
public class OrderProcessor
{
    private EmailService _emailService = new EmailService(); // Concrete dependency

    public void ProcessOrder()
    {
        // Process the order
        _emailService.SendEmail("Order processed successfully!");
    }
}
```

- `OrderProcessor` is tightly coupled to `EmailService`.
- If we need to switch to a different notification method (e.g., SMS), we must modify `OrderProcessor`.

### Using Dependency Injection to Reduce Coupling

- **Define an Abstraction**: Create an interface that defines the contract for sending notifications.

```csharp
public interface INotificationService
{
    void Send(string message);
}
```

- **Implement the Interface**: Implement the interface in `EmailService` and other classes as needed.

```csharp
public class EmailService : INotificationService
{
    public void Send(string message)
    {
        Console.WriteLine($"Sending email: {message}");
    }
}

public class SmsService : INotificationService
{
    public void Send(string message)
    {
        Console.WriteLine($"Sending SMS: {message}");
    }
}
```

- **Inject the Dependency**: Pass an implementation of the interface to `OrderProcessor` via a constructor.

```csharp
public class OrderProcessor
{
    private readonly INotificationService _notificationService;

    public OrderProcessor(INotificationService notificationService) // Dependency Injection
    {
        _notificationService = notificationService;
    }

    public void ProcessOrder()
    {
        // Process the order
        _notificationService.Send("Order processed successfully!");
    }
}

```

### Types of Dependency Injection

1. **Constructor Injection**: The dependency is provided through the constructor. This is the most common and preferred method.

```csharp
public OrderProcessor(INotificationService notificationService) { ... }
```

2. **Property Injection**: The dependency is assigned to a property.

```csharp
public INotificationService NotificationService { get; set; }
```

1. **Method Injection**: The dependency is passed to a method where it’s used.

```csharp
public void ProcessOrder(INotificationService notificationService) { ... }
```

### Using a Dependency Injection Container

In larger applications, managing dependencies manually can be cumbersome. **DI containers** (such as the one in ASP.NET Core) automate dependency management by providing instances of services to classes that need them.

In ASP.NET Core, we configure services and dependencies in the `ConfigureServices` method using `IServiceCollection`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddTransient<INotificationService, EmailService>(); // Register EmailService as implementation for INotificationService
    services.AddTransient<OrderProcessor>(); // Register OrderProcessor
}
```

Now it can be used:

```csharp
public class OrderController : Controller
{
    private readonly OrderProcessor _orderProcessor;

    public OrderController(OrderProcessor orderProcessor)
    {
        _orderProcessor = orderProcessor;
    }
}
```

The container ensures `OrderProcessor` has all the dependencies it needs, and we can change `INotificationService`’s implementation without altering `OrderProcessor`.
