# C# Fundamentals

This document contains the basic C# concepts for quick revision and reference.

---

## Step 1: What is C#?

**C# (C-Sharp)** is a modern, general-purpose programming language developed by **Microsoft**.

C# is commonly used for:

* Web applications
* REST APIs
* Desktop applications
* Games with Unity
* Cloud applications
* Enterprise software
* Backend development

C# runs on **.NET**, Microsoft's development platform.

### C# and .NET Ecosystem

A simple way to understand the relationship:

```text
C#       → Programming language
.NET     → Platform / Runtime
ASP.NET  → Web development framework
EF Core  → Database / ORM framework
```

For example:

```text
C# + .NET + ASP.NET Core + SQL Server
                    ↓
             Web Application
```

---

# Step 2: Your First C# Program

A simple C# program can be written as:

```csharp
Console.WriteLine("Hello, World!");
```

### Output

```text
Hello, World!
```

### What does it mean?

`Console` refers to the console.

`WriteLine()` prints text or values to the console and then moves to the next line.

So:

```csharp
Console.WriteLine("Hello, World!");
```

means:

```text
Print "Hello, World!" on the screen.
```

---

## Printing Multiple Values

```csharp
Console.WriteLine("My name is Sandip");
Console.WriteLine("I am learning C#");
Console.WriteLine("C# is powerful");
```

### Output

```text
My name is Sandip
I am learning C#
C# is powerful
```

---

## Console.Write()

You can also use `Console.Write()`:

```csharp
Console.Write("Hello ");
Console.Write("World");
```

### Output

```text
Hello World
```

### Difference Between WriteLine() and Write()

| Method                | Description                                          |
| --------------------- | ---------------------------------------------------- |
| `Console.WriteLine()` | Prints and moves to the next line                    |
| `Console.Write()`     | Prints without automatically moving to the next line |

Example:

```csharp
Console.WriteLine("Hello");
Console.WriteLine("World");
```

Output:

```text
Hello
World
```

While:

```csharp
Console.Write("Hello ");
Console.Write("World");
```

Output:

```text
Hello World
```

---

# Step 3: Comments

**Comments** are ignored by the C# compiler.

They are mainly used to explain code and make programs easier to understand.

## Single-Line Comment

Use `//` for a single-line comment.

```csharp
// This is a comment

Console.WriteLine("Hello");
```

## Multi-Line Comment

Use `/* */` for multi-line comments.

```csharp
/*
   This is a
   multi-line comment
*/

Console.WriteLine("Hello");
```

### Why Use Comments?

Comments can be useful for:

* Explaining code
* Documenting logic
* Making code easier to understand
* Temporarily disabling code during development

---

# Step 4: Variables

A **variable** is a named storage location used to hold data.

Example:

```csharp
string name = "Sandip";
int age = 22;
double salary = 50000.50;
bool isStudent = true;
```

You can think of a variable like a box that stores a value:

```text
name
 └── "Sandip"

age
 └── 22

salary
 └── 50000.50

isStudent
 └── true
```

## Printing Variables

```csharp
string name = "Sandip";
int age = 22;

Console.WriteLine(name);
Console.WriteLine(age);
```

### Output

```text
Sandip
22
```

---

## Important C# Data Types

| Type      | Example   | Used For                         |
| --------- | --------- | -------------------------------- |
| `int`     | `10`      | Whole numbers                    |
| `double`  | `10.5`    | Decimal numbers                  |
| `float`   | `10.5f`   | Decimal numbers                  |
| `decimal` | `100.50m` | Financial/precise decimal values |
| `char`    | `'A'`     | Single character                 |
| `string`  | `"Hello"` | Text                             |
| `bool`    | `true`    | True/false values                |

### Example

```csharp
int age = 22;
double height = 5.8;
decimal price = 999.99m;
char grade = 'A';
string name = "Sandip";
bool isActive = true;
```

---

# Step 5: String Interpolation

**String interpolation** is a convenient way to combine text with variables or expressions.

It uses the `$` symbol before the string.

Example:

```csharp
string name = "Sandip";
int age = 22;

Console.WriteLine($"My name is {name} and I am {age} years old.");
```

### Output

```text
My name is Sandip and I am 22 years old.
```

The `$` tells C# that expressions inside `{}` should be evaluated.

For example:

```csharp
int a = 10;
int b = 20;

Console.WriteLine($"Sum = {a + b}");
```

### Output

```text
Sum = 30
```

---

# Quick Revision

```text
C#          → Programming language
.NET        → Development platform/runtime
Console     → Used for console input/output
WriteLine() → Prints and moves to a new line
Write()     → Prints without moving to a new line
//          → Single-line comment
/* */       → Multi-line comment
Variable    → Stores data
$"..."      → String interpolation
```

## Key Points

* C# is developed by Microsoft.
* C# runs on the .NET platform.
* `Console.WriteLine()` prints output and moves to the next line.
* `Console.Write()` prints output without automatically moving to the next line.
* Comments are ignored by the compiler.
* Variables store data.
* C# provides different data types such as `int`, `double`, `decimal`, `char`, `string`, and `bool`.
* String interpolation uses `$` and `{}` to insert variables or expressions into strings.
