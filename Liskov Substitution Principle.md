---
date: 2025-02-10
tags:
  - note/programming
---
> [!note]
> "Objects of a superclass should be replaceable with objects of a subclass without affecting the functionality of the program."

In simpler terms, if a class `B` is a subclass of class `A`, then objects of class `A` should be replaceable with objects of class `B` without altering the correct behavior of the program.
# Identifying LSP Violations

LSP violations typically occur when a subclass changes the behavior of a base class in ways that are unexpected, contradictory, or problematic. Common signs of LSP violations include:

- **Modifying methods** in ways that restrict or alter the expected behavior of the base class.
- **Throwing exceptions** in subclasses where the base class would not.
- **Adding preconditions** (more requirements to use a method) or **weakening postconditions** (failing to meet base class guarantees).

#  <font color=FF0000>Bad implementation of LSP</font>

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

# Correcting the Violation

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