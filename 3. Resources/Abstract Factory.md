---
date: 2025-02-04
tags:
  - csharp
  - study
  - design-patterns
source:
---
Provides an interface for creating group of related or dependent objects without specifying their concrete classes. 

## When to use?
- Families of related objects
- Decoupling client code
- Product variants
## Implementation

### Define the abstract product interfaces

```csharp
public interface IButton
{
    void Render();
}

public interface ICheckbox
{
    void Render();
}
```

### Create Concrete Products for Windows

```csharp
public class WindowsButton : IButton
{
    public void Render()
    {
        Console.WriteLine("Rendering a Windows styled button.");
    }
}

public class WindowsCheckbox : ICheckbox
{
    public void Render()
    {
        Console.WriteLine("Rendering a Windows styled checkbox.");
    }
}

```

### Create Concrete Products for macOS

```csharp
public class MacOSButton : IButton
{
    public void Render()
    {
        Console.WriteLine("Rendering a macOS styled button.");
    }
}

public class MacOSCheckbox : ICheckbox
{
    public void Render()
    {
        Console.WriteLine("Rendering a macOS styled checkbox.");
    }
}
```

### Define the Abstract Factory Interface

```csharp
public interface IGUIFactory
{
    IButton CreateButton();
    ICheckbox CreateCheckbox();
}
```

### Create Concrete Factories

```csharp
public class WindowsFactory : IGUIFactory
{
    public IButton CreateButton()
    {
        return new WindowsButton();
    }

    public ICheckbox CreateCheckbox()
    {
        return new WindowsCheckbox();
    }
}

public class MacOSFactory : IGUIFactory
{
    public IButton CreateButton()
    {
        return new MacOSButton();
    }

    public ICheckbox CreateCheckbox()
    {
        return new MacOSCheckbox();
    }
}
```

### Client Code that Uses the Factories

```csharp
public class Application
{
    private readonly IButton _button;
    private readonly ICheckbox _checkbox;

    public Application(IGUIFactory factory)
    {
        _button = factory.CreateButton();
        _checkbox = factory.CreateCheckbox();
    }

    public void Render()
    {
        _button.Render();
        _checkbox.Render();
    }
}

class Program
{
    static void Main(string[] args)
    {
        // Determine which factory to use based on configuration or runtime
        IGUIFactory factory;

        // For demonstration, let's assume the platform is Windows.
        // In a real scenario, you might have a configuration setting or runtime check.
        string os = "Windows";

        if (os == "Windows")
        {
            factory = new WindowsFactory();
        }
        else
        {
            factory = new MacOSFactory();
        }

        Application app = new Application(factory);
        app.Render();

        // Alternatively, if you want to test macOS appearance, uncomment the following:
        // factory = new MacOSFactory();
        // app = new Application(factory);
        // app.Render();
    }
}
```

