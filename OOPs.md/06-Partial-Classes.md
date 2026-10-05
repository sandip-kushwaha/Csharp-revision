# C# Partial Classes

A **partial class** allows a single class to be split into multiple files or multiple declarations.

Instead of putting a large class into one file:

```text
User.cs
```

you can divide it into:

```text
User.cs
User.Validation.cs
User.Methods.cs
```

The compiler combines all parts into **one class**.

---

# 1. What Is a Partial Class?

A partial class is declared using the `partial` keyword.

```csharp
public partial class User
{
    public string Name { get; set; }
}
```

Another part can be:

```csharp
public partial class User
{
    public void Login()
    {
        Console.WriteLine("User logged in.");
    }
}
```

Although there are two declarations, C# treats them as one class.

---

# 2. Why Use Partial Classes?

Partial classes are useful when a class becomes large or when different parts of a class need to be maintained separately.

Benefits include:

* Organizing large classes
* Separating responsibilities within one class
* Working with generated code
* Making designer-generated code easier to maintain
* Allowing multiple developers to work on different parts
* Separating validation, properties, and methods
* Working with tools and frameworks that generate C# code

---

# 3. Basic Example

### File 1: `Student.cs`

```csharp
public partial class Student
{
    public string Name { get; set; }

    public int Age { get; set; }
}
```

### File 2: `Student.Methods.cs`

```csharp
public partial class Student
{
    public void DisplayInfo()
    {
        Console.WriteLine($"Name: {Name}");
        Console.WriteLine($"Age: {Age}");
    }
}
```

Now we can use:

```csharp
Student student = new Student();

student.Name = "Sandip";
student.Age = 22;

student.DisplayInfo();
```

The compiler sees:

```text
Student
├── Name
├── Age
└── DisplayInfo()
```

as one class.

---

# 4. How the Compiler Sees a Partial Class

Suppose we have:

```text
Student.cs
```

```csharp
public partial class Student
{
    public string Name { get; set; }
}
```

And:

```text
Student.Methods.cs
```

```csharp
public partial class Student
{
    public void Display()
    {
        Console.WriteLine(Name);
    }
}
```

Conceptually, the compiler combines them:

```csharp
public class Student
{
    public string Name { get; set; }

    public void Display()
    {
        Console.WriteLine(Name);
    }
}
```

You do **not** manually combine the files.

The compiler does it.

---

# 5. The `partial` Keyword

All parts of the class must use:

```csharp
partial
```

Correct:

```csharp
public partial class Student
{
}
```

```csharp
public partial class Student
{
}
```

Incorrect:

```csharp
public class Student
{
}
```

```csharp
public partial class Student
{
}
```

If one part is declared as a normal class and another part as partial, they are not treated as separate partial declarations correctly.

---

# 6. Partial Class Across Files

A common structure is:

```text
Models/
├── Student.cs
├── Student.Validation.cs
└── Student.Methods.cs
```

### `Student.cs`

```csharp
public partial class Student
{
    public string Name { get; set; }

    public int Age { get; set; }
}
```

### `Student.Validation.cs`

```csharp
public partial class Student
{
    public bool IsValidAge()
    {
        return Age >= 18;
    }
}
```

### `Student.Methods.cs`

```csharp
public partial class Student
{
    public void Display()
    {
        Console.WriteLine(
            $"Student: {Name}, Age: {Age}"
        );
    }
}
```

All three files represent the same `Student` class.

---

# 7. Partial Class and Access Modifiers

The parts of a partial class generally need compatible declarations.

For example:

```csharp
public partial class Student
{
}
```

and:

```csharp
public partial class Student
{
}
```

are valid.

The important point is that all declarations refer to the same class.

---

# 8. Partial Class with Fields

Fields can be placed in one part.

```csharp
public partial class User
{
    private string password;
}
```

Methods can be placed in another:

```csharp
public partial class User
{
    public bool ValidatePassword(string input)
    {
        return password == input;
    }
}
```

Both can access the same field because they are part of the same class.

---

# 9. Partial Class with Properties

```csharp
public partial class Product
{
    public string Name { get; set; }

    public decimal Price { get; set; }
}
```

Another file:

```csharp
public partial class Product
{
    public decimal GetDiscountedPrice(
        decimal discount
    )
    {
        return Price - discount;
    }
}
```

Usage:

```csharp
Product product = new Product
{
    Name = "Laptop",
    Price = 100000
};

Console.WriteLine(
    product.GetDiscountedPrice(10000)
);
```

---

# 10. Partial Class with Constructors

Constructors can be defined in one part of the class.

```csharp
public partial class Student
{
    public string Name { get; set; }

    public Student(string name)
    {
        Name = name;
    }
}
```

Another part can contain methods:

```csharp
public partial class Student
{
    public void Display()
    {
        Console.WriteLine(Name);
    }
}
```

Usage:

```csharp
Student student = new Student("Sandip");

student.Display();
```

---

# 11. Partial Class with Methods

Methods can be distributed across different files.

### `Order.cs`

```csharp
public partial class Order
{
    public int Id { get; set; }

    public decimal Total { get; set; }
}
```

### `Order.Calculation.cs`

```csharp
public partial class Order
{
    public decimal CalculateTax()
    {
        return Total * 0.13m;
    }
}
```

### `Order.Display.cs`

```csharp
public partial class Order
{
    public void Display()
    {
        Console.WriteLine($"Order ID: {Id}");
        Console.WriteLine($"Total: {Total}");
    }
}
```

The result is still one `Order` class.

---

# 12. Partial Classes and Encapsulation

Partial classes do not change the normal access rules of a class.

For example:

```csharp
public partial class BankAccount
{
    private decimal balance;
}
```

Another part can access:

```csharp
public partial class BankAccount
{
    public decimal GetBalance()
    {
        return balance;
    }
}
```

Because both declarations are parts of the same class.

The `private` field is still private from outside the class.

---

# 13. Partial Methods

C# also supports **partial methods**.

A partial method allows one part of a partial class to declare a method and another part to implement it.

Basic idea:

```csharp
partial void OnNameChanged();
```

Another part:

```csharp
partial void OnNameChanged()
{
    Console.WriteLine("Name changed.");
}
```

---

# 14. Basic Partial Method Example

### File 1

```csharp
public partial class Student
{
    public string Name
    {
        get => name;

        set
        {
            name = value;
            OnNameChanged();
        }
    }

    private string name = "";

    partial void OnNameChanged();
}
```

### File 2

```csharp
public partial class Student
{
    partial void OnNameChanged()
    {
        Console.WriteLine("Student name changed.");
    }
}
```

Usage:

```csharp
Student student = new Student();

student.Name = "Sandip";
```

Output:

```text
Student name changed.
```

---

# 15. Why Partial Methods Exist

Partial methods are useful when generated code needs to provide optional extension points.

For example:

```text
Generated code
      ↓
Partial method declaration
      ↓
Your custom implementation
```

This allows generated code to provide hooks without requiring you to modify generated files directly.

---

# 16. Important Partial Method Rules

Modern C# allows more flexible partial methods than older C# versions.

Partial methods can have different accessibility and return types when they have an implementation.

For example:

```csharp
partial void OnUpdated();
```

is a common simple form.

With newer C# versions, partial methods can also be declared with accessibility and return values when an implementation is required.

Example:

```csharp
public partial class Calculator
{
    partial int CalculateValue();
}
```

Implementation:

```csharp
public partial class Calculator
{
    partial int CalculateValue()
    {
        return 100;
    }
}
```

The exact restrictions depend on how the partial method is declared, but the main concept remains:

> One part declares the partial method and another part can implement it.

---

# 17. Partial Classes and Generated Code

One of the most important real-world uses of partial classes is **generated code**.

Some tools generate C# files automatically.

You generally should not edit generated files directly because your changes may be overwritten.

Instead, generated code can expose a partial class:

```csharp
public partial class GeneratedUser
{
    public string Name { get; set; }
}
```

You can create your own file:

```csharp
public partial class GeneratedUser
{
    public string GetDisplayName()
    {
        return $"User: {Name}";
    }
}
```

Now you can add custom behavior without modifying the generated file.

---

# 18. Designer-Generated Code

Partial classes are commonly associated with tools that generate UI-related code.

For example, a tool may generate:

```text
Form1.Designer.cs
```

while your custom code is:

```text
Form1.cs
```

Both can represent:

```csharp
public partial class Form1
{
}
```

The generated file contains generated UI code, while your file contains your custom logic.

This prevents you from mixing generated code with manually written code.

---

# 19. Partial Classes in ASP.NET / .NET

Partial classes can also appear in .NET projects when generated code or framework tooling is involved.

For example, a generated model might contain:

```csharp
public partial class User
{
    public int Id { get; set; }

    public string Name { get; set; }
}
```

You can extend it:

```csharp
public partial class User
{
    public string GetDisplayName()
    {
        return $"User: {Name}";
    }
}
```

This is useful when generated models need additional behavior.

---

# 20. Partial Class in Entity Framework

Entity Framework and database scaffolding can generate model classes.

For example:

```csharp
public partial class Product
{
    public int ProductId { get; set; }

    public string Name { get; set; }
}
```

Instead of editing the generated class directly, you can extend it:

```csharp
public partial class Product
{
    public string DisplayName =>
        $"Product: {Name}";
}
```

If the model is regenerated later, your custom file remains separate.

---

# 21. Partial Class and Namespace

All parts of the partial class must belong to the same namespace.

### File 1

```csharp
namespace HotelManagement;

public partial class Room
{
    public int Number { get; set; }
}
```

### File 2

```csharp
namespace HotelManagement;

public partial class Room
{
    public void Display()
    {
        Console.WriteLine(Number);
    }
}
```

They are parts of the same class.

If you accidentally place them in different namespaces:

```csharp
namespace HotelManagement;
```

and:

```csharp
namespace RestaurantManagement;
```

they represent different classes.

---

# 22. Partial Class and Inheritance

A partial class can inherit from a base class.

```csharp
public partial class Manager : Employee
{
    public void Manage()
    {
        Console.WriteLine("Managing...");
    }
}
```

Another part:

```csharp
public partial class Manager
{
    public void Report()
    {
        Console.WriteLine("Generating report...");
    }
}
```

The complete class still inherits from `Employee`.

---

# 23. Partial Class with Interface

A partial class can implement interfaces.

```csharp
public partial class User : IDisposable
{
    public void Dispose()
    {
        Console.WriteLine("Disposed.");
    }
}
```

Another part:

```csharp
public partial class User
{
    public string Name { get; set; }
}
```

The complete `User` class implements `IDisposable`.

---

# 24. Can Different Parts Have Different Base Classes?

No.

All parts represent the **same class**, so you cannot do this:

```csharp
public partial class User : Person
{
}
```

and:

```csharp
public partial class User : Employee
{
}
```

A class cannot inherit from two unrelated base classes.

The base type must be consistent across the partial declarations.

---

# 25. Partial Class and Interfaces

Interfaces can be declared consistently across partial declarations.

For example:

```csharp
public partial class User : IUser
{
}
```

Another part can also contain additional interfaces:

```csharp
public partial class User : IDisposable
{
}
```

The combined class can implement both:

```text
User
├── IUser
└── IDisposable
```

However, keeping inheritance/interface declarations organized consistently is usually better for readability.

---

# 26. Real-World Hotel Management Example

Imagine your hotel management application has a large `HotelRoom` class.

Instead of:

```text
HotelRoom.cs
```

containing everything, organize it:

```text
Models/
├── HotelRoom.cs
├── HotelRoom.Validation.cs
├── HotelRoom.Pricing.cs
└── HotelRoom.Display.cs
```

### `HotelRoom.cs`

```csharp
public partial class HotelRoom
{
    public int RoomNumber { get; set; }

    public string RoomType { get; set; }

    public decimal Price { get; set; }

    public string Status { get; set; }
}
```

### `HotelRoom.Validation.cs`

```csharp
public partial class HotelRoom
{
    public bool IsValid()
    {
        return RoomNumber > 0
            && Price >= 0
            && !string.IsNullOrWhiteSpace(RoomType);
    }
}
```

### `HotelRoom.Pricing.cs`

```csharp
public partial class HotelRoom
{
    public decimal CalculateTax()
    {
        return Price * 0.13m;
    }

    public decimal GetTotalPrice()
    {
        return Price + CalculateTax();
    }
}
```

### `HotelRoom.Display.cs`

```csharp
public partial class HotelRoom
{
    public void Display()
    {
        Console.WriteLine($"Room: {RoomNumber}");
        Console.WriteLine($"Type: {RoomType}");
        Console.WriteLine($"Price: {Price}");
        Console.WriteLine($"Status: {Status}");
    }
}
```

Usage:

```csharp
HotelRoom room = new HotelRoom
{
    RoomNumber = 101,
    RoomType = "Deluxe",
    Price = 5000,
    Status = "Available"
};

room.Display();

Console.WriteLine(
    $"Total: {room.GetTotalPrice()}"
);
```

Even though the functionality is split into different files, it is still one `HotelRoom` class.

---

# 27. Partial Class vs Separate Classes

This is very important.

### Partial class

```text
HotelRoom
├── HotelRoom.cs
├── HotelRoom.Pricing.cs
└── HotelRoom.Validation.cs
```

These are all parts of **one class**.

### Separate classes

```text
HotelRoom
RoomPricingService
RoomValidationService
```

These are **different classes**.

Use partial classes when the pieces naturally belong to the same class.

Use separate classes when the responsibilities should actually be separated.

---

# 28. Partial Class vs Inheritance

Partial class:

```text
One class
   ↓
Split into files
```

Inheritance:

```text
Base class
    ↓
Derived class
```

Example:

```csharp
class Animal
{
}
```

```csharp
class Dog : Animal
{
}
```

Inheritance creates a relationship between different classes.

Partial classes do not create multiple classes.

---

# 29. Partial Class vs Interface

### Partial class

Used to split the implementation of one class.

```csharp
public partial class User
{
}
```

### Interface

Defines a contract:

```csharp
public interface IUser
{
    void Login();
}
```

A class implements the contract:

```csharp
public class User : IUser
{
    public void Login()
    {
    }
}
```

---

# 30. Partial Class and Access to Private Members

All parts of a partial class have access to the same private members.

### File 1

```csharp
public partial class Account
{
    private decimal balance = 1000;
}
```

### File 2

```csharp
public partial class Account
{
    public void DisplayBalance()
    {
        Console.WriteLine(balance);
    }
}
```

Output:

```text
1000
```

The field is private from external code, but all partial declarations are part of the same class.

---

# 31. Partial Class with Nested Types

A partial class can contain nested types.

```csharp
public partial class User
{
    public class Address
    {
        public string City { get; set; }
    }
}
```

Another part can use the nested type:

```csharp
public partial class User
{
    public Address GetAddress()
    {
        return new Address
        {
            City = "Birgunj"
        };
    }
}
```

Both parts belong to the same outer class.

---

# 32. Important Rules

Remember these rules:

### Rule 1

Use the `partial` keyword:

```csharp
partial class User
{
}
```

### Rule 2

All parts represent one class.

### Rule 3

All parts must have the same class name.

### Rule 4

All parts must belong to the same namespace.

### Rule 5

All parts are combined by the compiler.

### Rule 6

Private members are shared across all parts.

### Rule 7

A partial class can implement interfaces and inherit from a base class.

### Rule 8

Partial classes are especially useful with generated code.

---

# 33. Advantages

### 1. Better organization

Large classes can be divided into logical files.

### 2. Generated code support

Custom code can be kept separate from generated code.

### 3. Easier maintenance

Developers can find related functionality more easily.

### 4. Team development

Different developers can work on different parts of a class.

### 5. Separation of concerns within one class

For example:

```text
User.cs
User.Validation.cs
User.Authentication.cs
User.Display.cs
```

---

# 34. Disadvantages

Partial classes are not always the best solution.

### 1. Can hide a large class

If a class has 20 partial files, the class may have too many responsibilities.

### 2. Harder to understand

You may need to search multiple files to understand the complete class.

### 3. Doesn't solve bad architecture

Splitting a huge class into files does not automatically make the design good.

### 4. Can encourage overuse

If responsibilities are genuinely independent, separate classes may be better.

---

# 35. When Should You Use Partial Classes?

Use them when:

* Generated code is involved.
* A framework/tool creates part of the class.
* A large but cohesive class needs logical file separation.
* You want to keep generated and custom code separate.
* Different parts of a class have clearly defined organization.

---

# 36. When Should You Avoid Partial Classes?

Avoid them when:

* The class is already small.
* Different responsibilities should be separate classes.
* You're only using partial classes to hide poor design.
* The number of files makes the class harder to understand.

Instead consider:

* Composition
* Service classes
* Interfaces
* Inheritance
* Dependency Injection

---

# 37. Complete Example

### `Employee.cs`

```csharp
public partial class Employee
{
    public int Id { get; set; }

    public string Name { get; set; }

    public decimal Salary { get; set; }
}
```

### `Employee.Validation.cs`

```csharp
public partial class Employee
{
    public bool IsValid()
    {
        return Id > 0
            && !string.IsNullOrWhiteSpace(Name)
            && Salary >= 0;
    }
}
```

### `Employee.Salary.cs`

```csharp
public partial class Employee
{
    public decimal CalculateAnnualSalary()
    {
        return Salary * 12;
    }
}
```

### `Employee.Display.cs`

```csharp
public partial class Employee
{
    public void Display()
    {
        Console.WriteLine($"ID: {Id}");
        Console.WriteLine($"Name: {Name}");
        Console.WriteLine($"Salary: {Salary}");
    }
}
```

### Usage

```csharp
Employee employee = new Employee
{
    Id = 1,
    Name = "Sandip",
    Salary = 50000
};

employee.Display();

Console.WriteLine(
    $"Valid: {employee.IsValid()}"
);

Console.WriteLine(
    $"Annual Salary: {employee.CalculateAnnualSalary()}"
);
```

Output:

```text
ID: 1
Name: Sandip
Salary: 50000
Valid: True
Annual Salary: 600000
```

---

# 38. Partial Class Mental Model

Think of a partial class like assembling multiple files into one class:

```text
             ┌─────────────────┐
             │   User.cs       │
             │   Properties    │
             └────────┬────────┘
                      │
             ┌────────▼────────┐
             │ User.Validation │
             │ Validation      │
             └────────┬────────┘
                      │
             ┌────────▼────────┐
             │ User.Methods    │
             │ Methods         │
             └────────┬────────┘
                      │
                      ▼
              ┌──────────────┐
              │     User     │
              │   ONE CLASS  │
              └──────────────┘
```

The files are separate, but the class is one.

---

# 39. Quick Revision

### Normal class

```csharp
public class User
{
}
```

### Partial class

```csharp
public partial class User
{
}
```

Another part:

```csharp
public partial class User
{
}
```

### Partial method

```csharp
partial void OnUpdated();
```

Implementation:

```csharp
partial void OnUpdated()
{
    Console.WriteLine("Updated");
}
```

### Common project structure

```text
Models/
├── User.cs
├── User.Validation.cs
├── User.Authentication.cs
└── User.Display.cs
```

---

# Summary

A partial class allows one class to be divided into multiple declarations:

```csharp
public partial class User
{
    public string Name { get; set; }
}
```

Another file:

```csharp
public partial class User
{
    public void Login()
    {
        Console.WriteLine("Login");
    }
}
```

The compiler treats them as:

```csharp
public class User
{
    public string Name { get; set; }

    public void Login()
    {
        Console.WriteLine("Login");
    }
}
```

### Core idea

> **A partial class lets you split one class across multiple files while keeping it as a single class at compile time.**

The most important keyword is:

```csharp
partial
```
