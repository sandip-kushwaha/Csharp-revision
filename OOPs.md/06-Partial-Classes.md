# C# Partial Classes

A **partial class** allows one class to be divided into **multiple files**.

Keyword:

```csharp
partial
```

---

## 1. Basic Example

### Student.Part1.cs

```csharp
public partial class Student
{
    public string Name { get; set; } = "";
}
```

### Student.Part2.cs

```csharp
public partial class Student
{
    public void Display()
    {
        Console.WriteLine(Name);
    }
}
```

Both files together form one `Student` class.

---

## 2. Creating Object

```csharp
Student student = new Student();

student.Name = "Sandip";
student.Display();
```

Output:

```text
Sandip
```

---

## 3. Why Use Partial Classes?

Useful when:

* A class is very large.
* Code needs to be organized into files.
* Generated code must be separated from custom code.
* Multiple developers work on different parts of a class.

---

## 4. Partial Methods

A partial class can also contain partial methods.

```csharp
public partial class User
{
    partial void OnUserCreated();
}
```

Implementation:

```csharp
public partial class User
{
    partial void OnUserCreated()
    {
        Console.WriteLine("User created");
    }
}
```

---

## 5. Real-World Example

### Food.Model.cs

```csharp
public partial class Food
{
    public string Name { get; set; } = "";
    public decimal Price { get; set; }
}
```

### Food.Methods.cs

```csharp
public partial class Food
{
    public void Display()
    {
        Console.WriteLine($"{Name} - Rs. {Price}");
    }
}
```

Usage:

```csharp
Food food = new()
{
    Name = "Momo",
    Price = 250
};

food.Display();
```

---

## 6. Important Rules

All parts must have:

* Same class name
* Same namespace
* `partial` keyword

Example:

```csharp
public partial class User
{
}
```

```csharp
public partial class User
{
}
```

The compiler combines them into one class.

---

## 7. Partial Class vs Normal Class

| Partial Class                    | Normal Class            |
| -------------------------------- | ----------------------- |
| Can be split into multiple files | Usually one file        |
| Uses `partial`                   | No `partial`            |
| Still one class                  | One class               |
| Useful for large/generated code  | Good for normal classes |

---

## 8. Common Uses

### Entity Framework

Generated models can be extended without modifying generated files.

### Windows Forms / Designer

Designer-generated code is commonly separated from custom code.

### Large Applications

Separate:

```text
User.Properties.cs
User.Methods.cs
User.Validation.cs
```

while keeping:

```csharp
public partial class User
```

---

## 9. Quick Revision

```text
partial class
     ↓
One class
     ↓
Multiple files
     ↓
Compiler combines them
```

Example:

```csharp
public partial class Hotel
{
    public string Name { get; set; } = "";
}
```

```csharp
public partial class Hotel
{
    public void Open()
    {
        Console.WriteLine("Hotel opened");
    }
}
```

### Remember

**Partial class = One class divided into multiple files.**
