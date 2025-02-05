---
date: 2025-02-04
tags:
  - csharp
  - study
source: "[[Videos#Advanced Topics in C Sharp]]"
---
They are used when we have more than one class that does almost the same thing and we want to use it in a certain way in which we prefer to not repeat or rethink logic.

```csharp
public interface IProductModel
{
    string Title { get; set; }
    bool HasOrderBeenCompleted { get; }

    void ShipItem(CustomerModel customer);
}
```

```csharp
 public class DigitalProductModel : IProductModel
 {
     public string Title { get; set; }

     public bool HasOrderBeenCompleted { get; private set; }

     public void ShipItem(CustomerModel customer)
     {
         if (HasOrderBeenCompleted == false)
         {
             Console.WriteLine($"Sendig course { Title } to {customer.FirstName} at {customer.EmailAddress}");
             HasOrderBeenCompleted = true;
         }
     }
 }
```

```csharp
public class PhysicalProductModel : IProductModel
{
    public string Title { get; set; }

    public bool HasOrderBeenCompleted { get; private set; }

    public void ShipItem(CustomerModel customer)
    {
        if (HasOrderBeenCompleted == false)
        {
            Console.WriteLine($"Simulating shipping {Title} to {customer.FirstName} in {customer.City}");
            HasOrderBeenCompleted = true;
        }
    }
}
```

```csharp
 static void Main(string[] args)
 {
     List<IProductModel> cart = AddSampleData();
     CustomerModel customer = GetCustomer();

     foreach (IProductModel prod in cart)
     {
         prod.ShipItem(customer);
     }

     Console.ReadLine();
 }

 private static CustomerModel GetCustomer()
 {
    ...
 }

 private static List<IProductModel> AddSampleData() 
 { 
     List<IProductModel> output = new List<IProductModel>();

     output.Add(new PhysicalProductModel { Title = "Nerf Football" });
     output.Add(new PhysicalProductModel { Title = "I am Tim Corey t-shirt" });
     output.Add(new PhysicalProductModel { Title = "Hard Drive" });
     output.Add(new DigitalProductModel { Title = "C# course" });

     return output;
 }
```
