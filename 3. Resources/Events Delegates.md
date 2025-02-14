---
date: 2025-02-04
tags: []
---
Is a way for objects to communicate between each other, it's like notifying that something happened. 

1. **Publisher**: The object that generates the event and sends notifications.
2. **Subscriber**: The object(s) that listen for the event and respond to it when it occurs.

### Declaring event

```csharp
// Set the type it accepts with generics
public event EventHandler<string> TransactionApprovedEvent; 
...
public bool AddDeposit(string depositName, decimal amount)
{
    _transactions.Add($"Deposited { string.Format("{0:C2}", amount) } for { depositName }");
    Balance += amount;
    TransactionApprovedEvent?.Invoke(this, depositName);
    return true;
}
```

### Using declared event

```csharp
 customer.CheckingAccount.TransactionApprovedEvent += CheckingAccount_TransactionApprovedEvent;
...
 private void CheckingAccount_TransactionApprovedEvent(object sender, string e)
 {
     checkingTransactions.DataSource = null;
     checkingTransactions.DataSource = customer.CheckingAccount.Transactions;
     checkingBalanceValue.Text = string.Format("{0:C2}", customer.CheckingAccount.Balance);
 }
 ...
```

### Unsubscribing from events

```csharp
public class Button
{
    public event EventHandler Clicked; // Declare event

    public void OnClick()
    {
        Clicked?.Invoke(this, EventArgs.Empty); // Raise the event
    }
}

public class Subscriber
{
    private Button _button;

    public Subscriber(Button button)
    {
        _button = button;
        _button.Clicked += HandleButtonClicked;
    }

    private void HandleButtonClicked(object sender, EventArgs e)
    {
        Console.WriteLine("Button was clicked!");
    }

    public void Unsubscribe()
    {
        _button.Clicked -= HandleButtonClicked;
    }
}

```

### Using `Dispose` to Unsubscribe

```csharp
public class DisposableSubscriber : IDisposable
{
    private Button _button;

    public DisposableSubscriber(Button button)
    {
        _button = button;
        _button.Clicked += HandleButtonClicked;
    }

    private void HandleButtonClicked(object sender, EventArgs e)
    {
        Console.WriteLine("Button was clicked!");
    }

    public void Dispose()
    {
        // Unsubscribe from the event
        _button.Clicked -= HandleButtonClicked;
        Console.WriteLine("Unsubscribed from Clicked event.");
    }
}

```

### Custom objects using EventArgs

```csharp
public class OverdraftEventArgs : EventArgs
{
    public decimal AmountOverdrafted { get; private set; }
    public string MoreInfo { get; private set; }
    public bool CancelTransaction { get; set; } = false;

    public OverdraftEventArgs(decimal amountOverdrafted, string moreInfo)
    {
        AmountOverdrafted = amountOverdrafted;
        MoreInfo = moreInfo;
    }
}
```

```csharp
public event EventHandler<OverdraftEventArgs> OverdraftEvent;
...
// Checks to see if we have enough money in the backup account
if ((backupAccount.Balance + Balance) >= amount)
{

    // We have enough backup funds so transfar the amount to this account
    // and then complete the transaction.
    decimal amountNeeded = amount - Balance;
    OverdraftEventArgs args = new OverdraftEventArgs(amountNeeded, "Extra Info");

    OverdraftEvent?.Invoke(this, args);

    if (args.CancelTransaction == true)
    {
        return false;
    }
    ...
```

```csharp
customer.CheckingAccount.OverdraftEvent += CheckingAccount_OverdraftEvent;
...
private void CheckingAccount_OverdraftEvent(object sender, OverdraftEventArgs e)
{
    errorMessage.Text = $"You had an overdraft protection transfer of { string.Format("{0:C2}", e.AmountOverdrafted) }";
    e.CancelTransaction = denyOverdraft.Checked;
    errorMessage.Visible = true;
}
```

## Delegates
They are types that represent references  to methods with specific parameters list and return type. 
A delegate is like a signature, if a method matches it, it can be called upon.

### Simple delegate declaration

```csharp
public delegate void MentionDiscount(decimal subTotal);
```

### Simple delegate usage

```csharp
public decimal GenerateTotal(MentionDiscount mentionSubtotal)
{
	decimal subTotal = Items.Sum(x => x.Price);
	
	mentionSubtotal(subTotal);
...
}
```

### Simple delegate usage

This part is what makes them flexible 

On console app:

```csharp
...
Console.WriteLine($"The total for the cart is {cart.GenerateTotal(SubTotalAlert");
...
private static void SubTotalAlert(decimal subTotal)
{
    Console.WriteLine($"The subtotal is {subTotal:C2}");
}
```

On Forms app:

```csharp
private void messageBoxDemoButton_Click(object sender, EventArgs e)
{
    decimal total = cart.GenerateTotal(SubTotalAlert);

    MessageBox.Show($"The total is {total:C2}");
}

private void SubTotalAlert(decimal subTotal)
{
    MessageBox.Show($"The subtotal is {subTotal:C2}");
}
```

### Delegates with Return Values

Delegates can represent methods with return values. 

```csharp
public delegate int MathOperation(int x, int y);

public class Calculator
{
    public int Add(int x, int y)
    {
        return x + y;
    }

    public int Multiply(int x, int y)
    {
        return x * y;
    }
}
```

```csharp
public class Program
{
    static void Main(string[] args)
    {
        Calculator calculator = new Calculator();
        MathOperation addOperation = calculator.Add;
        MathOperation multiplyOperation = calculator.Multiply;

        Console.WriteLine(addOperation(5, 3)); // Output: 8
        Console.WriteLine(multiplyOperation(5, 3)); // Output: 15
    }
}
```

### Anonymous Methods

```csharp
MathOperation subtractOperation = delegate (int x, int y)
{
    return x - y;
};
Console.WriteLine(subtractOperation(10, 5)); // Output: 5
```

### Lambda expressions

```csharp
MathOperation divideOperation = (x, y) => x / y;

Console.WriteLine(divideOperation(10, 2)); // Output: 5
```

Lambda expressions are commonly used with delegates for concise, readable code.

### Multicast Delegates

It can reference multiple methods. When a multicast delegate is invoked, it calls each method in its invocation list in the order they were added.

**Combining Delegates**: Use the `+=` operator to add methods to the delegate and `-=` to remove them.

```csharp
public delegate void Notify();

public class Notification
{
    public void EmailNotification()
    {
        Console.WriteLine("Email Notification Sent.");
    }

    public void SMSNotification()
    {
        Console.WriteLine("SMS Notification Sent.");
    }
}
```

**Using a Multicast Delegate**: Add `EmailNotification` and `SMSNotification` to `Notify` and invoke it.

```csharp
public class Program
{
    static void Main(string[] args)
    {
        Notification notification = new Notification();
        Notify notifyDelegate = notification.EmailNotification;
        notifyDelegate += notification.SMSNotification;

        notifyDelegate(); 
        // Output:
        // Email Notification Sent.
        // SMS Notification Sent.
    }
}
```

### *Built-in delegates*

**Using built-in delegates like `Action`, `Func`, and `Predicate`:

- **Simplifies code** by eliminating custom delegate definitions for common use cases.
- **Enhances readability and maintainability** by following conventions.
- **Saves time** and reduces potential for errors due to fewer declarations.
### Action
Represents a delegate that takes parameters, but returns no value. 

```csharp
public decimal GenerateTotal(Action<string> tellUserWeAreDiscounting)
{
	tellUserWeAreDiscounting("We are applying your discount.");
	...
}
```

```csharp
Console.WriteLine($"The total for the cart is {cart.generateTotla(AlertUser)}");
...
private static void AlertUser(string message)
{
    Console.WriteLine(message);
}
```

### Func

Represents a delegate that takes parameters and returns a value.

```csharp
public List<ProductModel> Items { get; set; } = new List<ProductModel>();

public decimal GenerateTotal(Func<List<ProductModel>,decimal,decimal> calculateDiscountedTotal)
{
    decimal subTotal = Items.Sum(x => x.Price);

	decimal total = calculateDiscountedTotal(Items, subTotal);

    return total;
}
```

```csharp
Console.WriteLine($"The total for the cart is {cart.GenerateTotal(CalculateLeveledDiscount):C2}");

private static decimal CalculateLeveledDiscount(List<ProductModel> items, decimal subTotal)
{
    if (subTotal > 100)
    {
        return subTotal * 0.80M;
    }
    else if (subTotal > 50)
    {
        return subTotal * 0.85M;
    }
    else if (subTotal > 10)
    {
        return subTotal * 0.95M;
    }
    else
    {
        return subTotal;
    }
}
```

### Predicate

A delegate that always returns a bool, commonly used for filtering or conditional checks.

```csharp
Predicate<int> isEven = x => x % 2 == 0;
Console.WriteLine(isEven(4)); // Output: True
Console.WriteLine(isEven(3)); // Output: False
```
