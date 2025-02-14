---
date: 2025-02-05
---
It allows you to create objects by duplicating an existing object, rather than creating a new instance from scratch.

# Problem
---
Sometimes a direct copy from a class in needed. A direct copy can't be done because the new class will be too dependent on the one it's copying and the dependencies might not be accesible.

# Solution
---
The Prototype pattern delegates the cloning process to the actual objects that are being cloned. The pattern declares a common interface for all objects that support cloning.
An object that supports cloning is called a _prototype_. When your objects there are dozens of fields and hundreds of possible configurations, cloning them might serve as an alternative to subclassing.

# Structure
---
![[Prototype Diagram.png]]
1. The **Prototype** interface declares the cloning methods. In most cases, it’s a single `clone` method.
2. The **Concrete Prototype** class implements the cloning method. In addition to copying the original object’s data to the clone, this method may also handle some edge cases of the cloning process related to cloning linked objects, untangling recursive dependencies, etc.
3. The **Client** can produce a copy of any object that follows the prototype interface.
# Applicability
---
- [i] The cost of creating a new instance of an object is more expensive than copying an existing instance.
- [i] When you want to avoid the complexities of subclassing and want to create objects dynamically.
- [i] When you need to create a large number of similar objects.
# Pros and Cons
---
- [p] Objects can be cloned without coupling to their concrete classes.
- [p] Repeated initialization code can be removed in favor of cloning pre-built prototypes. 
- [p] Complex objects can be produced more conveniently
- [p] Alternative to inheritance when dealing with configuration presets for complex objects.
- [c] Cloning complex objects that have circular references might be very tricky. 
# Relations with Other Patterns
---
 - Many designs start by using [[Factory Method]] (less complicated more customizable via subclasses) and evolve toward [[Abstract Factory]], Prototype, or [[Builder]] (more flexible, but more complicated).
 - [[Abstract Factory]] classes are often based on Factory Methods, but can also use Prototype to compose the methods on these classes.
 - Prototype can help when you need to save copies of [[Command]] into history.
 - Designs that make heavy use of [[Composite]] and [[Decorator]] can often benefit from using Prototype. Applying the pattern lets you clone complex structures instead of re-constructing them from scratch.
 - Prototype isn’t based on inheritance, so it doesn’t have its drawbacks. On the other hand, _Prototype_ requires a complicated initialization of the cloned object. [[Factory Method]] is based on inheritance but doesn’t require an initialization step.
 - Sometimes Prototype can be a simpler alternative to [[Memento]]. This works if the object, the state of which you want to store in the history, is fairly straightforward and doesn’t have links to external resources, or the links are easy to re-establish.
 - [[Abstract Factory]], [[Builder]] and Prototypes can all implemented as [[Singleton]]
# Implementation
---
### Define the Prototype Interface
```csharp
public interface IShape
{
    IShape Clone();
    void Draw();
}
```

### Create Concrete Prototypes
```csharp
public class Circle : IShape
{
    public string Color { get; set; }

    public Circle(string color)
    {
        Color = color;
    }

    // Implement the Clone method
    public IShape Clone()
    {
        return new Circle(Color);
    }

    public void Draw()
    {
        Console.WriteLine($"Drawing a {Color} circle.");
    }
}

public class Square : IShape
{
    public string Color { get; set; }

    public Square(string color)
    {
        Color = color;
    }

    // Implement the Clone method
    public IShape Clone()
    {
        return new Square(Color);
    }

    public void Draw()
    {
        Console.WriteLine($"Drawing a {Color} square.");
    }
}
```

### Client Code
```csharp
class Program
{
    static void Main(string[] args)
    {
        // Create a prototype of Circle
        IShape circlePrototype = new Circle("Red");
        IShape clonedCircle = circlePrototype.Clone();
        
        // Draw the cloned circle
        clonedCircle.Draw();

        // Create a prototype of Square
        IShape squarePrototype = new Square("Blue");
        IShape clonedSquare = squarePrototype.Clone();
        
        // Draw the cloned square
        clonedSquare.Draw();
    }
}
```