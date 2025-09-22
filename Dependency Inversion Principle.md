---
date: 2025-02-10
tags:
  - note/programming
---

> [!note]
> **"High-level modules should not depend on low-level modules. Both should depend on abstractions."**
> **"Abstractions should not depend on details. Details should depend on abstractions."**

#### Goals of Dependency Inversion Principle

1. Reduce Coupling
2. Increase Flexibility
3. Enhance Testability

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