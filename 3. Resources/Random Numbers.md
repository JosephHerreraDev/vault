---
date: 2025-02-04
tags: []
---
> Declare only once

```csharp
Random random = new Random();

for(int i = 0; i < 10; i++)
{
	// go to next random number
	console.WriteLine(random.Next());

	// go to next random number with limit
	console.WriteLine(random.Next(11));

	// go to next random number with a range
	console.WriteLine(random.Next(5, 11));
}
```

> Useful to **shuffle** small lists

```csharp
Random random = new Random();

List<PersonModel> people = new List<PersonModel>
{
	new PersonModel{FirstName = "joe"},
	new PersonModel{FirstName = "bob"},
	new PersonModel{FirstName = "max"},
}

people.OrderBy(x => random.Next());

foreach (var p in people)
{
	console.WriteLine(p.FirstName);
}

public class PersonModel
{
	 public string FirstName {get; set;}
}
```

Note: To shuffle large list, [Fisher-Yates shuffle](https://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle)