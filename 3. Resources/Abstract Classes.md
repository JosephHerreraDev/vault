---
date: 2025-02-04
tags:
  - csharp
  - study
source: "[[Videos#Advanced Topics in C Sharp]]"
---
They are a combination of an interface and a base class.

> When to use:
> Use it when "X is a Y" if there is not a direct relation, use a helper class

```csharp
static void Main(string[] args)
{
	List<DataAccess> databases = new List<DataAccess>();
	{
		new SqlDataAccess();
		new SqlLiteDataAccess();
	};

	foreach(var db from databases)
	{
		db.LoadConnectionString("demo");
		db.LoadData("select * from table");
		db.SaveData("insert into table");
		Console.WriteLine();
	}
	Console.ReadLine();
}
```

### Base class for demonstration

```csharp
public class DataAccess
{
	public string LoadConnectionString(string name)
	{
		Console.WriteLine("Load connection String");
		return "testConnectionString";
	}
}
```

```csharp
public class SqlLiteDataAccess : DataAccess
{
	public void LoadData(string sql)
	{
		Console.WriteLine("Loading SQLite data");
	}

	public void SaveData(string sql)
	{
		Console.WriteLine("Saving data to SQLite");
	}
}
```

```csharp
public class SqlDataAccess : DataAccess
{
	public void LoadData(string sql)
	{
		Console.WriteLine("Loading Microsoft SQL data");
	}

	public void SaveData(string sql)
	{
		Console.WriteLine("Saving data to Microsoft SQL");
	}
}
```

### Creating the abstract class

```csharp
public abstract class DataAccess
{
	public virtual string LoadConnectionString(string name)
	{
		Console.WriteLine("Load connection String");
		return "testConnectionString";
	}

	public abstract void LoadData(string sql);

	public abstract void SaveData(string sql);
}
```

```csharp
public class SqlLiteDataAccess : DataAccess
{
	public override string LoadConnectionString(string name)
	{
		string output = base.LoadConnectionString(name);

		output += " (from SQLite)";

		return output;
	}
	
	public override void LoadData(string sql)
	{
		Console.WriteLine("Loading SQLite data");
	}

	public override void SaveData(string sql)
	{
		Console.WriteLine("Saving data to SQLite");
	}
}
```


```csharp
public class SqlDataAccess : DataAccess
{
	public override void LoadData(string sql)
	{
		Console.WriteLine("Loading Microsoft SQL data");
	}

	public override void SaveData(string sql)
	{
		Console.WriteLine("Saving data to Microsoft SQL");
	}
}
```

### Differences with an interface

| Aspect         | Interface                        | Abstract Class                                      |
| -------------- | -------------------------------- | --------------------------------------------------- |
| Implementation | No implementation (until C# 8.0) | Can have both abstract and concrete methods         |
| Inheritance    | Multiple interface inheritance   | Single inheritance (can still implement interfaces) |
| Fields         | Cannot have fields               | Can have fields                                     |
| Constructors   | No constructors                  | Can have constructors                               |
| When to Use    | Unrelated classes, flexibility   | Closely related classes, shared behavior            |
