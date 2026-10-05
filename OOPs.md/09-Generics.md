# C# Generics

Generics allow you to write **reusable, type-safe code** that works with different data types.

Instead of writing separate code for:

```text
int
string
double
Food
Order
Customer
```

you can write one generic class, method, or collection that works with many types.

Generics are heavily used throughout modern C# and .NET.

---

# 1. What Are Generics?

Without generics, you might write:

```csharp
int Add(int a, int b)
{
    return a + b;
}
```

If you want to add `double` values, you need another method:

```csharp
double Add(double a, double b)
{
    return a + b;
}
```

With generics:

```csharp
T Add<T>(T a, T b)
{
    // ...
}
```

The same concept can work with different types.

---

# 2. Why Use Generics?

Generics provide:

* Type safety
* Code reuse
* Better performance
* Less duplicate code
* Compile-time type checking
* Flexible classes and methods

For example:

```csharp
List<int> numbers = new();
List<string> names = new();
List<double> prices = new();
```

`List<T>` is a generic collection.

Here:

```text
T = Type
```

So:

```csharp
List<int>
```

means:

```text
T = int
```

and:

```csharp
List<string>
```

means:

```text
T = string
```

---

# 3. Generic Type Parameter

A type parameter is commonly represented using:

```text
T
```

Example:

```csharp
class Box<T>
{
    public T Value { get; set; }
}
```

Here `T` is a placeholder for a type.

Create an integer box:

```csharp
Box<int> numberBox = new();

numberBox.Value = 100;
```

Create a string box:

```csharp
Box<string> nameBox = new();

nameBox.Value = "Sandip";
```

The same class works with different types.

---

# 4. Generic Class

A generic class contains one or more type parameters.

```csharp
class Box<T>
{
    public T Value { get; set; }

    public void Display()
    {
        Console.WriteLine(Value);
    }
}
```

Use it:

```csharp
Box<int> number = new();

number.Value = 100;
number.Display();
```

Output:

```text
100
```

Another type:

```csharp
Box<string> name = new();

name.Value = "Sandip";
name.Display();
```

Output:

```text
Sandip
```

---

# 5. Generic Classes with Multiple Types

You can have multiple generic parameters.

```csharp
class Pair<TKey, TValue>
{
    public TKey Key { get; set; }
    public TValue Value { get; set; }

    public Pair(TKey key, TValue value)
    {
        Key = key;
        Value = value;
    }
}
```

Usage:

```csharp
Pair<int, string> student =
    new Pair<int, string>(1, "Sandip");
```

Access:

```csharp
Console.WriteLine(student.Key);
Console.WriteLine(student.Value);
```

Output:

```text
1
Sandip
```

This is similar to:

```csharp
Dictionary<int, string>
```

---

# 6. Generic Methods

A generic method can work with different types.

```csharp
static void Display<T>(T value)
{
    Console.WriteLine(value);
}
```

Use:

```csharp
Display<int>(100);
Display<string>("Sandip");
Display<double>(99.5);
```

Output:

```text
100
Sandip
99.5
```

C# can often infer the type automatically:

```csharp
Display(100);
Display("Sandip");
Display(99.5);
```

This is called **type inference**.

---

# 7. Generic Method Returning a Value

```csharp
static T GetValue<T>(T value)
{
    return value;
}
```

Usage:

```csharp
int number = GetValue(100);

string name = GetValue("Sandip");
```

The compiler determines the appropriate type.

---

# 8. Generic Method with Two Types

```csharp
static void DisplayPair<T, U>(T first, U second)
{
    Console.WriteLine($"First: {first}");
    Console.WriteLine($"Second: {second}");
}
```

Usage:

```csharp
DisplayPair(100, "Sandip");
```

Output:

```text
First: 100
Second: Sandip
```

Here:

```text
T = int
U = string
```

---

# 9. Generic Interfaces

Interfaces can also be generic.

```csharp
interface IRepository<T>
{
    void Add(T item);
    T? GetById(int id);
}
```

A class can implement it:

```csharp
class UserRepository : IRepository<User>
{
    public void Add(User user)
    {
        Console.WriteLine($"Added: {user.Name}");
    }

    public User? GetById(int id)
    {
        return null;
    }
}
```

Now the repository is strongly typed for `User`.

---

# 10. Generic Constraints

Sometimes you don't want to allow every possible type.

For example:

```csharp
class Repository<T>
{
}
```

allows any type.

But you can restrict `T`.

This is called a **generic constraint**.

Syntax:

```csharp
class Repository<T> where T : class
{
}
```

---

# 11. `where T : class`

This requires `T` to be a reference type.

```csharp
class Repository<T> where T : class
{
}
```

Valid:

```csharp
Repository<string> repository = new();
```

A class type is also valid:

```csharp
Repository<User> repository = new();
```

---

# 12. `where T : struct`

Requires `T` to be a value type.

```csharp
class NumberBox<T> where T : struct
{
    public T Value { get; set; }
}
```

Valid:

```csharp
NumberBox<int> box = new();
```

Valid:

```csharp
NumberBox<double> box = new();
```

---

# 13. `where T : new()`

Requires `T` to have a public parameterless constructor.

```csharp
class Factory<T> where T : new()
{
    public T Create()
    {
        return new T();
    }
}
```

Example:

```csharp
class User
{
    public string Name { get; set; } = "";
}

Factory<User> factory = new();

User user = factory.Create();
```

---

# 14. `where T : BaseClass`

You can require `T` to inherit from a specific class.

```csharp
class Animal
{
    public void Eat()
    {
        Console.WriteLine("Eating...");
    }
}

class AnimalManager<T> where T : Animal
{
    public void Process(T animal)
    {
        animal.Eat();
    }
}
```

Now:

```csharp
class Dog : Animal
{
}
```

is valid:

```csharp
AnimalManager<Dog> manager = new();

manager.Process(new Dog());
```

---

# 15. `where T : Interface`

You can require a type to implement an interface.

```csharp
interface IPayment
{
    void Pay();
}
```

Generic class:

```csharp
class PaymentProcessor<T> where T : IPayment
{
    public void Process(T payment)
    {
        payment.Pay();
    }
}
```

Implementation:

```csharp
class EsewaPayment : IPayment
{
    public void Pay()
    {
        Console.WriteLine("Payment using eSewa");
    }
}
```

Usage:

```csharp
PaymentProcessor<EsewaPayment> processor = new();

processor.Process(new EsewaPayment());
```

---

# 16. Multiple Generic Constraints

You can combine constraints.

```csharp
class Repository<T>
    where T : class, IEntity, new()
{
}
```

This means `T` must:

* Be a reference type
* Implement `IEntity`
* Have a public parameterless constructor

Example:

```csharp
interface IEntity
{
    int Id { get; set; }
}

class User : IEntity
{
    public int Id { get; set; }
    public string Name { get; set; } = "";
}
```

Then:

```csharp
Repository<User> repository = new();
```

---

# 17. Common Generic Constraints

| Constraint            | Meaning                          |
| --------------------- | -------------------------------- |
| `where T : class`     | Reference type                   |
| `where T : struct`    | Value type                       |
| `where T : new()`     | Public parameterless constructor |
| `where T : BaseClass` | Must inherit from class          |
| `where T : Interface` | Must implement interface         |
| `where T : unmanaged` | Unmanaged value type             |
| `where T : notnull`   | Cannot be nullable               |

---

# 18. Generic Collections

Many .NET collections are generic.

Examples:

```csharp
List<int>
List<string>

Dictionary<int, string>

HashSet<string>

Queue<Order>

Stack<string>
```

For example:

```csharp
List<string> foods = new();

foods.Add("Pizza");
foods.Add("Burger");
```

The compiler prevents incorrect types:

```csharp
foods.Add(100); // Error
```

This is one of the major benefits of generics.

---

# 19. Generics Provide Type Safety

Without strong typing, you could accidentally mix values.

With:

```csharp
List<int> numbers = new();
```

you can only add integers:

```csharp
numbers.Add(10);
numbers.Add(20);
numbers.Add(30);
```

This is invalid:

```csharp
numbers.Add("Hello");
```

The compiler catches the problem before the application runs.

---

# 20. Generics and Performance

Generics can also improve performance.

Consider:

```csharp
List<int> numbers = new();
```

The integer values remain strongly typed.

Older non-generic collections such as `ArrayList` store values as `object`, which can introduce boxing and unboxing for value types.

Generic collections avoid unnecessary boxing in many common cases.

---

# 21. Boxing and Unboxing

Value types can be boxed into `object`:

```csharp
int number = 100;

object value = number;
```

This is boxing.

Unboxing:

```csharp
int result = (int)value;
```

Generic collections generally avoid this for value types:

```csharp
List<int> numbers = new();

numbers.Add(100);
```

This is one reason generic collections are preferred.

---

# 22. Generic Delegates

Delegates can also be generic.

Common built-in generic delegates are:

```text
Action
Func
Predicate
```

---

## Action

`Action` represents a method that returns `void`.

```csharp
Action<string> print = name =>
{
    Console.WriteLine(name);
};

print("Sandip");
```

---

## Func

`Func` represents a method that returns a value.

```csharp
Func<int, int, int> add =
    (a, b) => a + b;

int result = add(10, 20);

Console.WriteLine(result);
```

Output:

```text
30
```

---

## Predicate

`Predicate<T>` returns `bool`.

```csharp
Predicate<int> isEven =
    number => number % 2 == 0;

Console.WriteLine(isEven(10));
```

Output:

```text
True
```

---

# 23. Generic Extension Methods

Extension methods can also be generic.

```csharp
static class CollectionExtensions
{
    public static bool IsEmpty<T>(this IEnumerable<T> items)
    {
        return !items.Any();
    }
}
```

Usage:

```csharp
List<string> foods = [];

bool empty = foods.IsEmpty();
```

This is commonly used in utility libraries and application code.

---

# 24. Generic Repository Pattern

Generics are commonly used in backend applications.

For example:

```csharp
interface IRepository<T>
{
    void Add(T entity);

    T? GetById(int id);

    void Delete(int id);

    List<T> GetAll();
}
```

A generic repository:

```csharp
class Repository<T> : IRepository<T>
{
    private readonly List<T> items = new();

    public void Add(T entity)
    {
        items.Add(entity);
    }

    public T? GetById(int id)
    {
        return default;
    }

    public void Delete(int id)
    {
        // Delete implementation
    }

    public List<T> GetAll()
    {
        return items;
    }
}
```

Then:

```csharp
Repository<User> users = new();

Repository<Order> orders = new();

Repository<Food> foods = new();
```

The same repository concept can work with different entity types.

> In real ASP.NET Core applications, repository patterns are a design choice rather than something every project must use.

---

# 25. `default(T)`

Sometimes you need the default value of a generic type.

Use:

```csharp
default(T)
```

Example:

```csharp
static T GetDefault<T>()
{
    return default!;
}
```

Examples:

```text
int       → 0
bool      → false
reference → null
```

Modern nullable-reference-type projects may use `default!` when the surrounding API guarantees an appropriate value.

---

# 26. Generic Class with Default Value

```csharp
class Box<T>
{
    public T GetDefault()
    {
        return default!;
    }
}
```

Usage:

```csharp
Box<int> numberBox = new();

Console.WriteLine(numberBox.GetDefault());
```

Output:

```text
0
```

---

# 27. Generic Type Naming Conventions

Common conventions:

```text
T
TKey
TValue
TItem
TEntity
TResult
TRequest
TResponse
```

Examples:

```csharp
class Repository<TEntity>
{
}
```

```csharp
class Response<TData>
{
}
```

```csharp
class Dictionary<TKey, TValue>
{
}
```

Meaningful names can improve readability when there are multiple type parameters.

---

# 28. Generic Class Example: Hotel

Suppose we want a reusable manager for hotel entities.

```csharp
class Manager<T>
{
    private readonly List<T> items = new();

    public void Add(T item)
    {
        items.Add(item);
    }

    public void DisplayAll()
    {
        foreach (T item in items)
        {
            Console.WriteLine(item);
        }
    }
}
```

Create a food manager:

```csharp
Manager<string> foodManager = new();

foodManager.Add("Pizza");
foodManager.Add("Burger");
foodManager.Add("Momo");

foodManager.DisplayAll();
```

The same class can manage numbers:

```csharp
Manager<int> numberManager = new();

numberManager.Add(10);
numberManager.Add(20);
numberManager.Add(30);
```

---

# 29. Generic API Response

Generics are very useful for API response models.

For example:

```csharp
class ApiResponse<T>
{
    public bool Success { get; set; }
    public string Message { get; set; } = "";
    public T? Data { get; set; }
}
```

A user response:

```csharp
ApiResponse<User> response = new()
{
    Success = true,
    Message = "User fetched successfully",
    Data = new User()
};
```

A food response:

```csharp
ApiResponse<List<Food>> response = new()
{
    Success = true,
    Message = "Foods fetched successfully",
    Data = foods
};
```

The same response structure supports different data types.

---

# 30. Generic Result Example

A common backend design is:

```csharp
class Result<T>
{
    public bool Success { get; set; }
    public string Message { get; set; } = "";
    public T? Data { get; set; }

    public static Result<T> Ok(T data, string message)
    {
        return new Result<T>
        {
            Success = true,
            Message = message,
            Data = data
        };
    }
}
```

Usage:

```csharp
Result<string> result =
    Result<string>.Ok(
        "Sandip",
        "User fetched successfully"
    );
```

Another type:

```csharp
Result<int> result =
    Result<int>.Ok(
        100,
        "Value fetched successfully"
    );
```

---

# 31. Generic Methods with Constraints

Constraints become especially useful when your generic method needs specific functionality.

For example:

```csharp
static T Max<T>(T a, T b) where T : IComparable<T>
{
    return a.CompareTo(b) > 0 ? a : b;
}
```

Usage:

```csharp
int result = Max(10, 20);

Console.WriteLine(result);
```

Output:

```text
20
```

Why do we need:

```csharp
where T : IComparable<T>
```

Because the method calls:

```csharp
a.CompareTo(b)
```

The compiler needs to know that `T` supports `CompareTo()`.

---

# 32. Generic Interfaces and Polymorphism

Generics can work together with interfaces.

```csharp
interface IRepository<T>
{
    void Add(T item);
}
```

Implementation:

```csharp
class Repository<T> : IRepository<T>
{
    public void Add(T item)
    {
        Console.WriteLine($"Added: {item}");
    }
}
```

Usage:

```csharp
IRepository<string> repository =
    new Repository<string>();

repository.Add("Pizza");
```

This combines:

```text
Generics
+
Interfaces
+
Polymorphism
```

This pattern is common in application architecture.

---

# 33. Generic Inheritance

A generic class can inherit from another class.

```csharp
class BaseRepository<T>
{
    public void Add(T item)
    {
        Console.WriteLine("Added item.");
    }
}

class UserRepository : BaseRepository<User>
{
}
```

Now:

```csharp
UserRepository repository = new();

repository.Add(new User());
```

---

# 34. Generic Nested Types

A generic type can contain another generic type.

```csharp
class Response<T>
{
    public class Metadata
    {
        public int StatusCode { get; set; }
    }

    public T? Data { get; set; }
}
```

Usage:

```csharp
Response<string> response = new();

response.Data = "Hello";
```

---

# 35. Generic vs Object

You might wonder why not simply use `object`.

Example:

```csharp
object value = 100;
```

Then:

```csharp
int number = (int)value;
```

With generics:

```csharp
class Box<T>
{
    public T Value { get; set; }
}
```

Usage:

```csharp
Box<int> box = new();

box.Value = 100;

int number = box.Value;
```

Benefits of generics:

```text
Type safety
No unnecessary casting
Better readability
Better reuse
Better performance for value types
```

---

# 36. Generics vs Overloading

Without generics:

```csharp
int Add(int a, int b)
{
    return a + b;
}

double Add(double a, double b)
{
    return a + b;
}
```

This works, but requires multiple implementations.

Generics can reduce duplication when the operation is valid for a broad set of types:

```csharp
static T GetValue<T>(T value)
{
    return value;
}
```

However, generics are **not a replacement for method overloading** in every situation.

Use the approach that best represents the operation.

---

# 37. Complete Generic Example

```csharp
using System;
using System.Collections.Generic;

class Repository<T>
{
    private readonly List<T> items = new();

    public void Add(T item)
    {
        items.Add(item);
    }

    public List<T> GetAll()
    {
        return items;
    }

    public int Count()
    {
        return items.Count;
    }
}

class Food
{
    public string Name { get; set; } = "";
    public decimal Price { get; set; }

    public override string ToString()
    {
        return $"{Name} - Rs. {Price}";
    }
}

class Program
{
    static void Main()
    {
        Repository<Food> foodRepository = new();

        foodRepository.Add(
            new Food
            {
                Name = "Pizza",
                Price = 500
            }
        );

        foodRepository.Add(
            new Food
            {
                Name = "Momo",
                Price = 250
            }
        );

        Console.WriteLine(
            $"Total foods: {foodRepository.Count()}"
        );

        foreach (Food food in foodRepository.GetAll())
        {
            Console.WriteLine(food);
        }
    }
}
```

Output:

```text
Total foods: 2
Pizza - Rs. 500
Momo - Rs. 250
```

---

# 38. Important Generic Concepts

```text
Generic
│
├── Generic Class
│
├── Generic Method
│
├── Generic Interface
│
├── Generic Delegate
│
├── Generic Collection
│
├── Generic Constraints
│
└── Type Inference
```

---

# 39. Generic Mental Model

Think of:

```csharp
Box<T>
```

as:

```text
Box of something
```

Then:

```csharp
Box<int>
```

means:

```text
Box of int
```

and:

```csharp
Box<string>
```

means:

```text
Box of string
```

So:

```text
T = placeholder for a type
```

---

# 40. Quick Revision

### What are generics?

Generics allow reusable, type-safe code that works with different types.

### What does `T` mean?

`T` is commonly used as a generic type parameter.

### Example

```csharp
class Box<T>
{
    public T Value { get; set; }
}
```

### Generic class

```csharp
Box<int>
Box<string>
```

### Generic method

```csharp
static void Display<T>(T value)
{
    Console.WriteLine(value);
}
```

### Generic constraint

```csharp
where T : class
```

### Multiple constraints

```csharp
where T : class, IEntity, new()
```

### Generic collection

```csharp
List<string>
Dictionary<int, string>
```

### Generic interface

```csharp
IRepository<User>
```

### Generic delegate

```csharp
Action<string>
Func<int, int>
Predicate<int>
```

---

# 41. Common Generic Constraints Revision

```csharp
where T : class
```

Reference type.

```csharp
where T : struct
```

Value type.

```csharp
where T : new()
```

Public parameterless constructor.

```csharp
where T : BaseClass
```

Must inherit from `BaseClass`.

```csharp
where T : IInterface
```

Must implement the interface.

```csharp
where T : notnull
```

Must not be nullable.

---

# 42. Best Practices

* Prefer generics over unnecessary `object` usage.
* Use meaningful generic parameter names when appropriate.
* Use constraints when the generic code requires specific capabilities.
* Prefer generic collections such as `List<T>` and `Dictionary<TKey,TValue>`.
* Avoid unnecessary casting.
* Keep generic APIs simple and readable.
* Don't make everything generic without a real reason.
* Combine generics with interfaces when designing reusable components.
* Use type inference when it improves readability.
* Use constraints to communicate requirements to the compiler.

---

# Summary

The main idea of generics is:

```text
Write once → Reuse with different types
```

For example:

```csharp
class Box<T>
{
    public T Value { get; set; }
}
```

Then:

```csharp
Box<int> numberBox = new();
Box<string> nameBox = new();
Box<Food> foodBox = new();
```

The same class works with different types while maintaining **compile-time type safety**.

Generics are one of the foundations of modern C# because many important .NET features—including collections, delegates, interfaces, and reusable application components—use them.
