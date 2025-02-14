---
date: 2025-02-10
tags: []
---
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