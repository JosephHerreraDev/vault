---
date: 2025-02-04
tags: []
---
1. **public**: access in all the program
2. **private:** can only access in the class it is declared in
3. **protected**: can only be access in the class declared and in the classes it inherits
4. **internal**: can only access in the assembly
5. **private protected**: can only access in inherit classes in the same assembly
6. **protected internal**: can access in the same assembly and in inherit classes anywhere
### Why?

**Security** 
Can only access methods and properties available

```csharp
public class BadClass
{
	private string _ssn;

	public string SSN
	{
		get {return "****-***-1234";} // only access formated ssn
		set {_ssn = value;}
	}
}
```

**Check for errors**
Can check functions before entering data

```csharp
public class BadClass
{
	private string _age;

	public string Age
	{
		get {return _age;}
		set 
		{
			if(value >= 0 && value < 130)
			{
				age = value;
			}
		}
	}
}
```
