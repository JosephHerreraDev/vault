---
date: 2025-02-04
tags:
  - csharp
  - design-patterns
  - study
---
Build complex objects step by step, separating the construction from a particular representation of a object.

## When to use?

- Separation of the construction and representation
- Flexibility

### Implementation

```csharp
using System;
using System.Collections.Generic;

// Producto final
public class Pizza
{
    public string Size { get; set; }
    public string Dough { get; set; }
    public List<string> Ingredients { get; } = new List<string>();

    public void ShowPizza()
    {
        Console.WriteLine($"Pizza size: {Size}");
        Console.WriteLine($"Dough: {Dough}");
        Console.WriteLine("Ingredients:");
        foreach (string ingredient in Ingredients)
        {
            Console.WriteLine($" - {ingredient}");
        }
    }
}

// Clase Builder abstracta (o interfaz)
public interface IPizzaBuilder
{
    void SelectSize(string size);
    void SelectDough(string dough);
    void AddIngredient(string ingredient);
    Pizza Build();
}

// Implementación concreta del Builder
public class PizzaBuilder : IPizzaBuilder
{
    private Pizza _pizza = new Pizza();

    public void SelectSize(string size)
    {
        _pizza.Size = size;
    }

    public void SelectDough(string dough)
    {
        _pizza.Dough = dough;
    }

    public void AddIngredient(string ingredient)
    {
        _pizza.Ingredients.Add(ingredient);
    }

    public Pizza Build()
    {
        // Puedes agregar validaciones o lógica adicional aquí
        Pizza result = _pizza;
        // Reiniciamos el builder para potencialmente construir otra pizza
        _pizza = new Pizza();
        return result;
    }
}

// Director: Opcional, se usa para encapsular la secuencia de pasos
public class PizzaDirector
{
    private readonly IPizzaBuilder _builder;

    public PizzaDirector(IPizzaBuilder builder)
    {
        _builder = builder;
    }

    // Método que define la secuencia de construcción de una pizza "Clásica"
    public void ConstruirPizzaClasica()
    {
        _builder.SeleccionarTamaño("Mediana");
        _builder.SeleccionarMasa("Fina");
        _builder.AgregarIngrediente("Queso");
        _builder.AgregarIngrediente("Tomate");
        _builder.AgregarIngrediente("Pepperoni");
    }
}

public class Program
{
    public static void Main(string[] args)
    {
        // Uso directo del Builder
        IPizzaBuilder builder = new PizzaBuilder();
        builder.SeleccionarTamaño("Grande");
        builder.SeleccionarMasa("Gruesa");
        builder.AgregarIngrediente("Queso");
        builder.AgregarIngrediente("Tomate");
        builder.AgregarIngrediente("Champiñones");
        Pizza pizzaPersonalizada = builder.Build();
        Console.WriteLine("Pizza Personalizada:");
        pizzaPersonalizada.MostrarPizza();

        Console.WriteLine();

        // Uso a través de un Director para construir una pizza clásica
        builder = new PizzaBuilder(); // Se puede reutilizar o crear otro Builder
        PizzaDirector director = new PizzaDirector(builder);
        director.ConstruirPizzaClasica();
        Pizza pizzaClasica = builder.Build();
        Console.WriteLine("Pizza Clásica:");
        pizzaClasica.MostrarPizza();
    }
}

```