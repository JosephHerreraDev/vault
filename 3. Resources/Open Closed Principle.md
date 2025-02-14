---
date: 2025-02-10
tags: []
---
> [!note]
> "Software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification."

You should be able to add new functionality to a system by extending existing code without altering or modifying it. 

#### Why?

1. Stability of Existing Code
2. Flexibility for Future Requirements
3. Improved Maintainability

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

4. OCP doesn't mean no changes ever
5. OCP is not just for large systems
6. Overengineering
