# C# LINQ

**LINQ = Language Integrated Query**

LINQ allows you to **query, filter, sort, transform, and group data** using C# syntax.

LINQ can work with:

* Arrays
* `List<T>`
* `Dictionary<TKey,TValue>`
* Objects
* Databases
* JSON/XML data

Namespace:

```csharp
using System.Linq;
```

---

## 1. Basic Example

```csharp
List<int> numbers = [10, 20, 30, 40, 50];

var result = numbers.Where(x => x > 25);

foreach (var number in result)
{
    Console.WriteLine(number);
}
```

Output:

```text
30
40
50
```

---

# 2. Where()

Filters data.

```csharp
var result = numbers.Where(x => x > 20);
```

Example:

```csharp
List<string> foods =
[
    "Pizza",
    "Burger",
    "Momo",
    "Pasta"
];

var result = foods.Where(food => food.Length > 5);
```

---

# 3. Select()

Transforms each item.

```csharp
var prices = numbers.Select(x => x * 2);
```

Example:

```csharp
var names = foods.Select(food => food.ToUpper());
```

---

# 4. OrderBy()

Sort ascending.

```csharp
var result = numbers.OrderBy(x => x);
```

Descending:

```csharp
var result = numbers.OrderByDescending(x => x);
```

---

# 5. ThenBy()

Used for secondary sorting.

```csharp
var result = foods
    .OrderBy(x => x.Length)
    .ThenBy(x => x);
```

---

# 6. First() and FirstOrDefault()

### First()

Returns the first item.

```csharp
int number = numbers.First();
```

Throws an exception if the collection is empty.

### FirstOrDefault()

```csharp
int number = numbers.FirstOrDefault();
```

Returns the default value if no item exists.

For `int`, default is:

```text
0
```

---

# 7. Last() and LastOrDefault()

```csharp
int last = numbers.Last();
```

Safer:

```csharp
int last = numbers.LastOrDefault();
```

---

# 8. Single() and SingleOrDefault()

`Single()` expects exactly one matching item.

```csharp
var result = numbers.Single(x => x == 30);
```

`SingleOrDefault()` allows zero or one matching item.

```csharp
var result =
    numbers.SingleOrDefault(x => x == 30);
```

Use these when uniqueness is part of the requirement.

---

# 9. Any()

Checks whether at least one item matches.

```csharp
bool exists = numbers.Any(x => x > 40);
```

Without condition:

```csharp
bool hasData = numbers.Any();
```

---

# 10. All()

Checks whether every item matches.

```csharp
bool result = numbers.All(x => x > 0);
```

---

# 11. Count()

Counts items.

```csharp
int count = numbers.Count();
```

With condition:

```csharp
int count =
    numbers.Count(x => x > 20);
```

For a `List<T>`, `Count` property is often preferable when you simply need the total:

```csharp
int count = numbers.Count;
```

---

# 12. Sum()

```csharp
int total = numbers.Sum();
```

Example:

```csharp
List<decimal> prices =
[
    100,
    200,
    300
];

decimal total = prices.Sum();
```

---

# 13. Average()

```csharp
double average = numbers.Average();
```

---

# 14. Min() and Max()

```csharp
int minimum = numbers.Min();
int maximum = numbers.Max();
```

---

# 15. Distinct()

Removes duplicate values.

```csharp
List<int> numbers =
[
    10, 20, 20, 30, 30
];

var result = numbers.Distinct();
```

Result:

```text
10
20
30
```

---

# 16. Take()

Takes the first specified number of items.

```csharp
var result = numbers.Take(3);
```

---

# 17. Skip()

Skips the first specified number of items.

```csharp
var result = numbers.Skip(2);
```

---

# 18. Take + Skip

Useful for pagination.

```csharp
int page = 2;
int pageSize = 10;

var result = numbers
    .Skip((page - 1) * pageSize)
    .Take(pageSize);
```

For page 2:

```text
Skip 10
Take 10
```

This pattern is common in backend APIs.

---

# 19. Contains()

```csharp
bool exists = numbers.Contains(30);
```

---

# 20. Select() with Objects

Suppose:

```csharp
class Food
{
    public string Name { get; set; } = "";
    public decimal Price { get; set; }
}
```

Data:

```csharp
List<Food> foods =
[
    new Food { Name = "Pizza", Price = 500 },
    new Food { Name = "Momo", Price = 250 },
    new Food { Name = "Burger", Price = 350 }
];
```

Get only names:

```csharp
var names = foods.Select(x => x.Name);
```

Get only prices:

```csharp
var prices = foods.Select(x => x.Price);
```

---

# 21. Where() with Objects

Foods above Rs. 300:

```csharp
var expensiveFoods =
    foods.Where(x => x.Price > 300);
```

---

# 22. OrderBy() with Objects

Sort by price:

```csharp
var result =
    foods.OrderBy(x => x.Price);
```

Descending:

```csharp
var result =
    foods.OrderByDescending(x => x.Price);
```

---

# 23. Select Anonymous Objects

You can create a new shape:

```csharp
var result = foods.Select(x => new
{
    x.Name,
    x.Price
});
```

Or:

```csharp
var result = foods.Select(x => new
{
    FoodName = x.Name,
    Cost = x.Price
});
```

---

# 24. Query Syntax

LINQ also supports SQL-like syntax.

```csharp
var result =
    from food in foods
    where food.Price > 300
    select food;
```

Method syntax:

```csharp
var result =
    foods.Where(x => x.Price > 300);
```

Both are LINQ.

Modern C# code commonly uses **method syntax**.

---

# 25. Chaining LINQ Methods

LINQ methods can be combined.

```csharp
var result = foods
    .Where(x => x.Price > 200)
    .OrderBy(x => x.Price)
    .Select(x => x.Name);
```

Flow:

```text
foods
  ↓
Where()
  ↓
OrderBy()
  ↓
Select()
  ↓
Result
```

---

# 26. ToList()

Many LINQ operations return `IEnumerable<T>`.

Convert to a list:

```csharp
List<Food> result =
    foods
        .Where(x => x.Price > 300)
        .ToList();
```

---

# 27. ToArray()

```csharp
Food[] result =
    foods
        .Where(x => x.Price > 300)
        .ToArray();
```

---

# 28. ToDictionary()

Convert data into a dictionary.

```csharp
Dictionary<string, decimal> result =
    foods.ToDictionary(
        x => x.Name,
        x => x.Price
    );
```

Result conceptually:

```text
Pizza  → 500
Momo   → 250
Burger → 350
```

Keys must be unique.

---

# 29. GroupBy()

Groups items based on a property.

Example:

```csharp
class Food
{
    public string Name { get; set; } = "";
    public string Category { get; set; } = "";
    public decimal Price { get; set; }
}
```

Group foods by category:

```csharp
var groups =
    foods.GroupBy(x => x.Category);
```

Loop:

```csharp
foreach (var group in groups)
{
    Console.WriteLine(group.Key);

    foreach (var food in group)
    {
        Console.WriteLine(food.Name);
    }
}
```

---

# 30. Join()

`Join()` combines related data from two collections.

Example:

```csharp
class Category
{
    public int Id { get; set; }
    public string Name { get; set; } = "";
}

class Food
{
    public string Name { get; set; } = "";
    public int CategoryId { get; set; }
}
```

Join:

```csharp
var result =
    foods.Join(
        categories,
        food => food.CategoryId,
        category => category.Id,
        (food, category) => new
        {
            Food = food.Name,
            Category = category.Name
        }
    );
```

This is conceptually similar to a SQL `JOIN`.

---

# 31. Aggregate()

`Aggregate()` performs custom accumulation.

Example:

```csharp
List<int> numbers = [1, 2, 3, 4];

int result = numbers.Aggregate(
    0,
    (total, number) => total + number
);
```

Result:

```text
10
```

For simple sums, prefer:

```csharp
numbers.Sum();
```

Use `Aggregate()` when you need custom accumulation logic.

---

# 32. Deferred Execution

Many LINQ queries don't execute immediately.

Example:

```csharp
var result =
    foods.Where(x => x.Price > 300);
```

The query may execute when you enumerate it:

```csharp
foreach (var food in result)
{
    Console.WriteLine(food.Name);
}
```

Or when you materialize it:

```csharp
var list = result.ToList();
```

This behavior is called **deferred execution**.

---

# 33. Immediate Execution

Methods such as:

```text
ToList()
ToArray()
ToDictionary()
Count()
Sum()
Average()
First()
```

can cause the query to execute immediately.

Example:

```csharp
var result =
    foods
        .Where(x => x.Price > 300)
        .ToList();
```

Now `result` is a materialized `List<Food>`.

---

# 34. LINQ and Database

LINQ is not limited to in-memory collections.

In ASP.NET Core with Entity Framework Core:

```csharp
var foods = await dbContext.Foods
    .Where(x => x.IsAvailable)
    .OrderBy(x => x.Name)
    .ToListAsync();
```

Entity Framework Core can translate many LINQ expressions into SQL.

Conceptually:

```text
C# LINQ
   ↓
EF Core
   ↓
SQL
   ↓
Database
```

---

# 35. Hotel Management Example

Get available foods:

```csharp
var availableFoods =
    foods.Where(x => x.IsAvailable);
```

Get vegetarian foods:

```csharp
var vegetarianFoods =
    foods.Where(x => x.IsVeg);
```

Get foods below Rs. 500:

```csharp
var affordableFoods =
    foods.Where(x => x.Price < 500);
```

Sort by cheapest:

```csharp
var cheapest =
    foods.OrderBy(x => x.Price);
```

Get top 5 foods:

```csharp
var topFoods =
    foods.Take(5);
```

Get food names:

```csharp
var foodNames =
    foods.Select(x => x.Name);
```

---

# 36. Complete Example

```csharp
using System;
using System.Collections.Generic;
using System.Linq;

class Food
{
    public string Name { get; set; } = "";
    public decimal Price { get; set; }
    public bool IsVeg { get; set; }
}

class Program
{
    static void Main()
    {
        List<Food> foods =
        [
            new Food
            {
                Name = "Pizza",
                Price = 500,
                IsVeg = true
            },
            new Food
            {
                Name = "Momo",
                Price = 250,
                IsVeg = true
            },
            new Food
            {
                Name = "Chicken Burger",
                Price = 450,
                IsVeg = false
            }
        ];

        var result = foods
            .Where(x => x.IsVeg)
            .OrderBy(x => x.Price)
            .Select(x => x.Name)
            .ToList();

        foreach (string food in result)
        {
            Console.WriteLine(food);
        }
    }
}
```

Output:

```text
Momo
Pizza
```

---

# 37. Common LINQ Methods

```text
Where()              → Filter
Select()             → Transform
OrderBy()            → Sort ascending
OrderByDescending()  → Sort descending
ThenBy()             → Secondary sort
First()              → First item
FirstOrDefault()     → First/default
Last()               → Last item
Single()             → Exactly one item
Any()                → At least one?
All()                → Do all match?
Count()              → Count items
Sum()                → Total
Average()            → Average
Min()                → Minimum
Max()                → Maximum
Distinct()           → Remove duplicates
Take()               → Take first N
Skip()               → Skip first N
GroupBy()            → Group
Join()               → Combine collections
ToList()             → Convert to List
ToArray()            → Convert to Array
ToDictionary()       → Convert to Dictionary
```

---

# 38. LINQ Mental Model

Remember:

```text
Collection
    ↓
Where()       → Filter
    ↓
Select()      → Transform
    ↓
OrderBy()     → Sort
    ↓
GroupBy()     → Group
    ↓
ToList()      → Materialize
```

Example:

```csharp
var result = foods
    .Where(x => x.Price > 300)
    .OrderBy(x => x.Price)
    .Select(x => x.Name)
    .ToList();
```

Think:

```text
Filter → Sort → Select → List
```

---

# 39. Best Practices

* Use LINQ when it makes collection operations clearer.
* Prefer method syntax for common operations.
* Use `Any()` instead of `Count() > 0` when checking existence.
* Use `FirstOrDefault()` when an item may not exist.
* Use `TryGetValue()` for dictionary lookups.
* Avoid unnecessary multiple enumerations.
* Use `ToList()` when you need to materialize results.
* Be careful with deferred execution.
* With EF Core, inspect generated SQL for complex queries.
* Don't create complicated LINQ chains when a simple loop is clearer.

---

# 40. Quick Revision

### Filter

```csharp
foods.Where(x => x.Price > 300);
```

### Transform

```csharp
foods.Select(x => x.Name);
```

### Sort

```csharp
foods.OrderBy(x => x.Price);
```

### Reverse sort

```csharp
foods.OrderByDescending(x => x.Price);
```

### Check existence

```csharp
foods.Any();
```

### Check condition

```csharp
foods.Any(x => x.Price > 500);
```

### Count

```csharp
foods.Count();
```

### First

```csharp
foods.FirstOrDefault();
```

### Remove duplicates

```csharp
foods.Distinct();
```

### Pagination

```csharp
foods
    .Skip(10)
    .Take(10);
```

### Convert to List

```csharp
foods.ToList();
```

---
## Summary

The most important LINQ methods are:

```text
Where()   → Filter
Select()  → Transform
OrderBy() → Sort
Any()     → Check existence
First()   → Get first
Count()   → Count
Sum()     → Total
GroupBy() → Group
Join()    → Combine
ToList()  → Materialize
```

The most common LINQ pattern is:

```csharp
var result = collection
    .Where(x => condition)
    .OrderBy(x => x.Property)
    .Select(x => x.Property)
    .ToList();
```

**LINQ = Query and manipulate data using C# expressions.**
