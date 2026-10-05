# C# OOP — Object-Oriented Programming

**OOP = Object-Oriented Programming**

OOP is one of the most important concepts in C#. Almost all real-world C# applications use object-oriented programming.

OOP organizes programs around **classes and objects**.

---

# 1. What is OOP?

OOP is a programming approach where we organize programs around **objects and classes**.

For example, in a Hotel Management System:

```text
Hotel
 ├── Room
 ├── Customer
 ├── Employee
 ├── Food
 ├── Order
 └── Payment
```

Each of these can be represented as a **class**.

For example:

```csharp
class Room
{
    public int RoomNumber { get; set; }
    public string RoomType { get; set; }
    public double Price { get; set; }
}
```

---

# 2. Class

A **class** is a blueprint or template for creating objects.

Example:

```csharp
class Student
{
    public string Name;
    public int Age;

    public void Study()
    {
        Console.WriteLine($"{Name} is studying.");
    }
}
```

Think of it like:

```text
Class = Blueprint
```

A class describes:

* What an object has → **Data / Properties**
* What an object can do → **Methods**

---

# 3. Object

An **object** is an actual instance of a class.

Example:

```csharp
Student student1 = new Student();

student1.Name = "Sandip";
student1.Age = 22;

student1.Study();
```

Output:

```text
Sandip is studying.
```

You can create multiple objects from the same class:

```csharp
Student student1 = new Student();
Student student2 = new Student();

student1.Name = "Sandip";
student2.Name = "Ram";
```

Think of it like:

```text
             Student Class
                  │
        ┌─────────┴─────────┐
        ↓                   ↓
   student1              student2
   Sandip                  Ram
```

The class is the blueprint, while the objects are the actual instances created from that blueprint.

---

# 4. Properties

A **property** is a controlled way to read and write data inside a class.

Instead of directly exposing fields:

```csharp
class Student
{
    public string Name;
    public int Age;
}
```

we commonly use properties:

```csharp
class Student
{
    public string Name { get; set; }
    public int Age { get; set; }
}
```

## `get` and `set`

```csharp
public string Name { get; set; }
```

means:

* `get` → allows reading the value
* `set` → allows changing the value

Example:

```csharp
Student student = new Student();

student.Name = "Sandip";             // set
Console.WriteLine(student.Name);     // get
```

Output:

```text
Sandip
```

---

# 5. Field vs Property

## Field

```csharp
public string name;
```

## Property

```csharp
public string Name { get; set; }
```

In modern C# applications, properties are generally preferred for data exposed by classes.

Example:

```csharp
class Student
{
    public string Name { get; set; }
    public int Age { get; set; }
    public double Marks { get; set; }
}
```

Usage:

```csharp
Student s1 = new Student();

s1.Name = "Sandip";
s1.Age = 22;
s1.Marks = 85.5;

Console.WriteLine(s1.Name);
Console.WriteLine(s1.Age);
Console.WriteLine(s1.Marks);
```

Output:

```text
Sandip
22
85.5
```

---

# 6. Why Properties Are Important

Properties allow us to control how data is accessed.

For example:

```csharp
public int Age { get; private set; }
```

Here:

* Anyone can read `Age`
* Only the class itself can change `Age`

Example:

```csharp
class Student
{
    public string Name { get; set; }

    public int Age { get; private set; }

    public void SetAge(int age)
    {
        Age = age;
    }
}
```

Usage:

```csharp
Student student = new Student();

student.Name = "Sandip";

student.SetAge(22);

Console.WriteLine(student.Age);
```

But this is not allowed:

```csharp
student.Age = 25;
```

because the `set` accessor is private.

This concept is important for **encapsulation**.

---

# 7. Constructor

A **constructor** is a special member of a class that runs automatically when an object is created.

Example:

```csharp
class Student
{
    public Student()
    {
        Console.WriteLine("Student object created!");
    }
}
```

Now:

```csharp
Student student = new Student();
```

Output:

```text
Student object created!
```

You did not call the constructor manually.

It runs automatically when `new` is used.

---

# 8. Constructor with Parameters

Constructors become more useful when we give them parameters.

```csharp
class Student
{
    public string Name { get; set; }
    public int Age { get; set; }

    public Student(string name, int age)
    {
        Name = name;
        Age = age;
    }
}
```

Now create an object:

```csharp
Student student = new Student("Sandip", 22);

Console.WriteLine(student.Name);
Console.WriteLine(student.Age);
```

Output:

```text
Sandip
22
```

Instead of:

```csharp
Student student = new Student();

student.Name = "Sandip";
student.Age = 22;
```

we can initialize everything through the constructor:

```csharp
Student student = new Student("Sandip", 22);
```

---

# 9. Understanding `this`

You will frequently see:

```csharp
public Student(string name, int age)
{
    this.Name = name;
    this.Age = age;
}
```

Here:

```csharp
this.Name
```

means the `Name` property belonging to the **current object**.

For example:

```csharp
class Student
{
    public string Name { get; set; }

    public Student(string Name)
    {
        this.Name = Name;
    }
}
```

The first:

```csharp
this.Name
```

is the class property.

The second:

```csharp
Name
```

is the constructor parameter.

A common professional style avoids this naming conflict:

```csharp
public Student(string name)
{
    Name = name;
}
```

Both approaches are valid.

---

# 10. Multiple Objects

One class can create many objects.

```csharp
class Student
{
    public string Name { get; set; }
    public int Age { get; set; }

    public Student(string name, int age)
    {
        Name = name;
        Age = age;
    }
}
```

Create three students:

```csharp
Student s1 = new Student("Sandip", 22);
Student s2 = new Student("Ram", 21);
Student s3 = new Student("Hari", 23);

Console.WriteLine(s1.Name);
Console.WriteLine(s2.Name);
Console.WriteLine(s3.Name);
```

Output:

```text
Sandip
Ram
Hari
```

Each object has its own data.

Think of it like:

```text
Student class
     │
     ├── s1 → Name = Sandip, Age = 22
     │
     ├── s2 → Name = Ram,    Age = 21
     │
     └── s3 → Name = Hari,   Age = 23
```

The class is the blueprint.

The objects are the actual instances created from that blueprint.

---

# 11. Methods

A class can contain methods that define what an object can do.

Example:

```csharp
class Student
{
    public string Name { get; set; }

    public void Study()
    {
        Console.WriteLine($"{Name} is studying.");
    }

    public void Sleep()
    {
        Console.WriteLine($"{Name} is sleeping.");
    }
}
```

Usage:

```csharp
Student student = new Student();

student.Name = "Sandip";

student.Study();
student.Sleep();
```

Methods represent **behavior**.

For example:

```text
Student
 ├── Name
 ├── Age
 ├── Study()
 └── Sleep()
```

---

# 12. Constructor + Method

We can combine properties, constructors, and methods.

```csharp
class Student
{
    public string Name { get; set; }
    public int Age { get; set; }

    public Student(string name, int age)
    {
        Name = name;
        Age = age;
    }

    public void Introduce()
    {
        Console.WriteLine(
            $"My name is {Name} and I am {Age} years old."
        );
    }
}
```

Usage:

```csharp
Student student = new Student("Sandip", 22);

student.Introduce();
```

Output:

```text
My name is Sandip and I am 22 years old.
```

---

# 13. The 4 Main Pillars of OOP ⭐

The four major pillars of OOP are:

```text
                    OOP
                     │
        ┌────────────┼────────────┐
        │            │            │
        ↓            ↓            ↓
 Encapsulation   Inheritance   Polymorphism
        │
        └──────────────→ Abstraction
```

More clearly:

```text
1. Encapsulation
2. Inheritance
3. Polymorphism
4. Abstraction
```

---

# 14. Encapsulation

**Encapsulation** means wrapping data and methods together and controlling access to the data.

Suppose we have a bank account.

We don't want anyone to directly modify the balance:

```csharp
account.Balance = -50000;
```

Instead, we can protect the balance:

```csharp
class BankAccount
{
    private double balance;

    public void Deposit(double amount)
    {
        if (amount > 0)
        {
            balance += amount;
        }
    }

    public double GetBalance()
    {
        return balance;
    }
}
```

Usage:

```csharp
BankAccount account = new BankAccount();

account.Deposit(5000);

Console.WriteLine(account.GetBalance());
```

Output:

```text
5000
```

Here:

```csharp
private double balance;
```

protects the data.

This is **encapsulation**.

### Why use encapsulation?

It helps us:

* Protect data
* Control how data changes
* Validate values
* Reduce unwanted access
* Keep class logic organized

---

# 15. Inheritance

**Inheritance** allows one class to reuse another class.

Example:

```csharp
class Animal
{
    public void Eat()
    {
        Console.WriteLine("Animal is eating.");
    }
}
```

Create a child class:

```csharp
class Dog : Animal
{
    public void Bark()
    {
        Console.WriteLine("Dog is barking.");
    }
}
```

Now:

```csharp
Dog dog = new Dog();

dog.Eat();
dog.Bark();
```

Output:

```text
Animal is eating.
Dog is barking.
```

Here:

```text
Animal
   ↑
   │
  Dog
```

`Dog` inherits from `Animal`.

### Parent and Child

```text
Animal → Parent/Base class
Dog    → Child/Derived class
```

Inheritance is useful when classes have an **is-a relationship**.

For example:

```text
Dog is an Animal
Cat is an Animal
Manager is an Employee
Admin is a User
```

---

# 16. Polymorphism

**Polymorphism** means:

> One thing can have multiple forms.

For example:

```csharp
class Animal
{
    public virtual void MakeSound()
    {
        Console.WriteLine("Animal makes a sound.");
    }
}
```

Dog:

```csharp
class Dog : Animal
{
    public override void MakeSound()
    {
        Console.WriteLine("Dog says Woof.");
    }
}
```

Cat:

```csharp
class Cat : Animal
{
    public override void MakeSound()
    {
        Console.WriteLine("Cat says Meow.");
    }
}
```

Now:

```csharp
Animal dog = new Dog();
Animal cat = new Cat();

dog.MakeSound();
cat.MakeSound();
```

Output:

```text
Dog says Woof.
Cat says Meow.
```

The same method:

```csharp
MakeSound()
```

behaves differently depending on the actual object.

That's **polymorphism**.

### Important keywords

```text
virtual  → allows a method to be overridden
override → provides a new implementation
```

---

# 17. Abstraction

**Abstraction** means hiding unnecessary implementation details and showing only what is necessary.

Suppose you use a car.

You know:

```text
Start()
Accelerate()
Brake()
```

But you don't need to know every internal engine operation.

That's abstraction.

C# provides `abstract` classes.

Example:

```csharp
abstract class Animal
{
    public abstract void MakeSound();
}
```

Child class:

```csharp
class Dog : Animal
{
    public override void MakeSound()
    {
        Console.WriteLine("Woof");
    }
}
```

Usage:

```csharp
Dog dog = new Dog();

dog.MakeSound();
```

Output:

```text
Woof
```

An abstract class can define what a child class **must implement** without providing the complete implementation itself.

---

# 18. Default Constructor

You can also have a constructor without parameters.

```csharp
class Student
{
    public string Name { get; set; }

    public Student()
    {
        Name = "Unknown";
    }
}
```

Usage:

```csharp
Student student = new Student();

Console.WriteLine(student.Name);
```

Output:

```text
Unknown
```

---

# 19. Constructor Overloading

Just like normal methods, constructors can be overloaded.

```csharp
class Student
{
    public string Name { get; set; }
    public int Age { get; set; }

    public Student()
    {
        Name = "Unknown";
        Age = 0;
    }

    public Student(string name)
    {
        Name = name;
        Age = 0;
    }

    public Student(string name, int age)
    {
        Name = name;
        Age = age;
    }
}
```

Now all of these are valid:

```csharp
Student s1 = new Student();

Student s2 = new Student("Sandip");

Student s3 = new Student("Sandip", 22);
```

This is called **constructor overloading**.

---

# 20. Real-World Example — Hotel Room

Imagine a Hotel Management System.

We could create a `Room` class:

```csharp
class Room
{
    public int RoomNumber { get; set; }
    public string RoomType { get; set; }
    public double Price { get; set; }

    public Room(int roomNumber, string roomType, double price)
    {
        RoomNumber = roomNumber;
        RoomType = roomType;
        Price = price;
    }

    public void ShowRoom()
    {
        Console.WriteLine($"Room: {RoomNumber}");
        Console.WriteLine($"Type: {RoomType}");
        Console.WriteLine($"Price: {Price}");
    }
}
```

Create an object:

```csharp
Room room = new Room(101, "Deluxe", 2500);

room.ShowRoom();
```

Output:

```text
Room: 101
Type: Deluxe
Price: 2500
```

This is similar to how real applications model data.

---

# 21. Professional C# Classes

You will frequently see classes like:

```csharp
public class User
{
    public string Username { get; set; }
    public string Email { get; set; }
    public int Age { get; set; }
}
```

This style is extremely common in:

* ASP.NET Core
* Web APIs
* Entity Framework Core
* DTOs
* Database models
* JSON serialization

---

# 22. C# Class and JSON

For example, JSON data:

```json
{
    "username": "sandip",
    "email": "sandip@example.com",
    "age": 22
}
```

can be represented by a C# class:

```csharp
public class User
{
    public string Username { get; set; }
    public string Email { get; set; }
    public int Age { get; set; }
}
```

This connection becomes very important when learning **ASP.NET Core Web API**.

---

# 23. Class Structure

A typical C# class can contain:

```text
Class
 │
 ├── Fields
 │
 ├── Properties
 │
 ├── Constructors
 │
 ├── Methods
 │
 └── Other members
```

Example:

```csharp
public class Student
{
    // Property
    public string Name { get; set; }

    // Property
    public int Age { get; set; }

    // Constructor
    public Student(string name, int age)
    {
        Name = name;
        Age = age;
    }

    // Method
    public void Study()
    {
        Console.WriteLine($"{Name} is studying.");
    }
}
```

---

# 24. OOP Quick Comparison

| Concept       | Meaning                            | Example              |
| ------------- | ---------------------------------- | -------------------- |
| Class         | Blueprint                          | `class Student`      |
| Object        | Instance of a class                | `new Student()`      |
| Property      | Stores/controls data               | `Name { get; set; }` |
| Constructor   | Initializes an object              | `Student(...)`       |
| Method        | Defines behavior                   | `Study()`            |
| Encapsulation | Protects and controls data         | `private balance`    |
| Inheritance   | Reuses another class               | `class Dog : Animal` |
| Polymorphism  | Same interface, different behavior | `override`           |
| Abstraction   | Hides implementation details       | `abstract class`     |

---

# 25. OOP Mental Model

A simple way to remember OOP:

```text
                 CLASS
                   │
        ┌──────────┼──────────┐
        ↓          ↓          ↓
     Data       Behavior    Rules
   Properties   Methods    Access
        │          │          │
        └──────────┼──────────┘
                   ↓
                 OBJECT
```

For example:

```text
Student
 │
 ├── Name
 ├── Age
 ├── Marks
 │
 ├── Study()
 ├── Sleep()
 └── Introduce()
```

---

# 26. OOP in a Real Application

For a Hotel Management System, you might have:

```text
Hotel System
 │
 ├── User
 │    ├── Admin
 │    ├── Waiter
 │    └── KitchenStaff
 │
 ├── Table
 │
 ├── Room
 │
 ├── Food
 │
 ├── Category
 │
 ├── Order
 │
 ├── Customer
 │
 └── Payment
```

Each can be represented by a class.

For example:

```csharp
public class Food
{
    public string Name { get; set; }
    public double Price { get; set; }
    public bool IsAvailable { get; set; }

    public void MarkUnavailable()
    {
        IsAvailable = false;
    }
}
```

Then:

```csharp
Food food = new Food();

food.Name = "Chicken Momo";
food.Price = 250;
food.IsAvailable = true;

food.MarkUnavailable();
```

This is the foundation of object-oriented application design.

---

# 27. Properties + Constructors + Methods

These three concepts commonly work together.

```csharp
public class User
{
    // Properties
    public string Username { get; set; }
    public string Email { get; set; }

    // Constructor
    public User(string username, string email)
    {
        Username = username;
        Email = email;
    }

    // Method
    public void ShowUser()
    {
        Console.WriteLine($"Username: {Username}");
        Console.WriteLine($"Email: {Email}");
    }
}
```

Usage:

```csharp
User user = new User(
    "sandip",
    "sandip@example.com"
);

user.ShowUser();
```

Output:

```text
Username: sandip
Email: sandip@example.com
```

---

# 28. Important OOP Keywords

| Keyword     | Purpose                                     |
| ----------- | ------------------------------------------- |
| `class`     | Defines a class                             |
| `new`       | Creates an object                           |
| `public`    | Accessible from outside                     |
| `private`   | Accessible only inside the class            |
| `protected` | Accessible inside class and derived classes |
| `this`      | Refers to the current object                |
| `virtual`   | Allows overriding                           |
| `override`  | Overrides inherited behavior                |
| `abstract`  | Defines incomplete abstraction              |
| `base`      | Refers to the parent class                  |

---

# 29. OOP Learning Order

A good order for learning C# OOP is:

```text
1. Class
      ↓
2. Object
      ↓
3. Properties
      ↓
4. Methods
      ↓
5. Constructors
      ↓
6. Encapsulation
      ↓
7. Inheritance
      ↓
8. Polymorphism
      ↓
9. Abstraction
      ↓
10. Interfaces
      ↓
11. Generics
      ↓
12. SOLID Principles
```

After these concepts, you can move toward:

```text
C# OOP
   ↓
.NET
   ↓
ASP.NET Core
   ↓
Web API
   ↓
Entity Framework Core
   ↓
Database
   ↓
Authentication & Authorization
   ↓
Real-world backend applications
```

---

# Summary

C# OOP is based on four major principles:

```text
Encapsulation
Inheritance
Polymorphism
Abstraction
```

The most important building blocks are:

```text
Class
Object
Property
Constructor
Method
```

A simple mental model is:

```text
Class
  ↓
Object
  ↓
Properties + Methods
  ↓
Encapsulation
  ↓
Inheritance
  ↓
Polymorphism
  ↓
Abstraction
  ↓
Real-world applications
```

These concepts form the foundation for building larger C# applications, especially with:

```text
ASP.NET Core
Web API
Entity Framework Core
Database Applications
Backend Systems
```
