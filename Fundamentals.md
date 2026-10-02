# C# Fundamentals

A collection of C# fundamentals for learning, revision, and quick reference.

---

## Step 1: What is C#?

**C# (C-Sharp)** is a programming language developed by **Microsoft**.

It is commonly used for:

* Web applications
* REST APIs
* Desktop applications
* Games with Unity
* Cloud applications
* Enterprise software
* Backend development

C# runs on **.NET**, Microsoft's development platform.

### C# and .NET Ecosystem

A simple way to understand it:

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

Modern C# allows a very simple program:

```csharp
Console.WriteLine("Hello, World!");
```

### Output

```text
Hello, World!
```

### What does it mean?

`Console` refers to the console.

`WriteLine()` prints something to the console.

So:

```csharp
Console.WriteLine("Hello, World!");
```

means:

```text
Print "Hello, World!" on the screen.
```

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

## Console.Write()

You can also use:

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

---

# Step 3: Comments

Comments are ignored by C#.

They are useful for explaining your code.

## Single-Line Comment

```csharp
// This is a comment

Console.WriteLine("Hello");
```

## Multi-Line Comment

```csharp
/*
   This is a
   multi-line comment
*/

Console.WriteLine("Hello");
```

---

# Step 4: Variables

A **variable** stores data.

For example:

```csharp
string name = "Sandip";
int age = 22;
double salary = 50000.50;
bool isStudent = true;
```

Think of a variable like a box:

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

## Important C# Data Types

| Type      | Example   | Used For         |
| --------- | --------- | ---------------- |
| `int`     | `10`      | Whole numbers    |
| `double`  | `10.5`    | Decimal numbers  |
| `float`   | `10.5f`   | Decimal numbers  |
| `decimal` | `100.50m` | Financial values |
| `char`    | `'A'`     | Single character |
| `string`  | `"Hello"` | Text             |
| `bool`    | `true`    | True/false       |

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

One of the most useful ways to combine variables and text is **string interpolation**.

```csharp
string name = "Sandip";
int age = 22;

Console.WriteLine($"My name is {name} and I am {age} years old.");
```

### Output

```text
My name is Sandip and I am 22 years old.
```

The `$` tells C# that `{}` contains variables or expressions.

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

# C# Tutorial — User Input & Type Conversion

Now let's learn how to take input from the user.

Real programs don't just display fixed values. They also receive and process data from users.

---

## 1. Console.ReadLine()

We use `Console.ReadLine()` to get input from the user.

```csharp
Console.Write("Enter your name: ");

string name = Console.ReadLine();

Console.WriteLine($"Hello, {name}!");
```

### Example

```text
Enter your name: Sandip
Hello, Sandip!
```

### How It Works

```csharp
string name = Console.ReadLine();
```

The user enters:

```text
Sandip
```

and it gets stored in:

```text
name
 ↓
"Sandip"
```

---

# 2. Important: ReadLine() Returns a String

Suppose we want the user's age:

```csharp
Console.Write("Enter your age: ");

string age = Console.ReadLine();
```

Even if the user enters:

```text
22
```

C# initially treats it as:

```text
"22"
```

That's a **string**, not an integer.

So this won't work as expected for numeric addition:

```csharp
Console.WriteLine(age + 5);
```

Because `age` is text.

---

# 3. Converting String to Integer

Use `int.Parse()`:

```csharp
Console.Write("Enter your age: ");

int age = int.Parse(Console.ReadLine());

Console.WriteLine($"You are {age} years old.");
```

### Input

```text
Enter your age: 22
```

### Output

```text
You are 22 years old.
```

The conversion is:

```text
"22"
 ↓
int.Parse()
 ↓
22
```

---

# 4. Example: Add Two Numbers

Let's make a small calculator:

```csharp
Console.Write("Enter first number: ");
int num1 = int.Parse(Console.ReadLine());

Console.Write("Enter second number: ");
int num2 = int.Parse(Console.ReadLine());

int sum = num1 + num2;

Console.WriteLine($"Sum = {sum}");
```

### Example

```text
Enter first number: 10
Enter second number: 20

Sum = 30
```

---

# 5. Different Types of Conversion

## String → int

```csharp
int age = int.Parse("22");
```

## String → double

```csharp
double price = double.Parse("99.50");
```

## String → decimal

```csharp
decimal salary = decimal.Parse("50000.50");
```

## String → bool

```csharp
bool result = bool.Parse("true");
```

---

# 6. Convert Methods

You can also use the `Convert` class.

```csharp
int age = Convert.ToInt32(Console.ReadLine());

double price = Convert.ToDouble(Console.ReadLine());

decimal salary = Convert.ToDecimal(Console.ReadLine());
```

For beginners, you'll commonly see both:

```csharp
int.Parse()
```

and:

```csharp
Convert.ToInt32()
```

---

# 7. Parse() vs TryParse()

There is an important problem with `Parse()`.

Suppose the user enters:

```text
abc
```

when your program expects an integer:

```csharp
int age = int.Parse(Console.ReadLine());
```

The program throws an exception because `"abc"` cannot be converted to an integer.

A safer approach is `TryParse()`:

```csharp
Console.Write("Enter your age: ");

bool success = int.TryParse(Console.ReadLine(), out int age);

Console.WriteLine($"Age = {age}");
```

`TryParse()` returns:

```text
true  → Conversion successful
false → Conversion failed
```

> `TryParse()` is especially useful when working with user input because it allows you to handle invalid input without immediately throwing a conversion exception.

---

# 8. Mini Project: Simple Calculator

Let's combine what we've learned.

```csharp
Console.Write("Enter first number: ");
double num1 = double.Parse(Console.ReadLine());

Console.Write("Enter second number: ");
double num2 = double.Parse(Console.ReadLine());

Console.WriteLine($"Addition = {num1 + num2}");
Console.WriteLine($"Subtraction = {num1 - num2}");
Console.WriteLine($"Multiplication = {num1 * num2}");
Console.WriteLine($"Division = {num1 / num2}");
```

### Example

```text
Enter first number: 20
Enter second number: 5

Addition = 25
Subtraction = 15
Multiplication = 100
Division = 4
```

---

# Quick Revision

| Concept               | Remember                                    |
| --------------------- | ------------------------------------------- |
| C#                    | Programming language developed by Microsoft |
| .NET                  | Platform/runtime for building applications  |
| `Console.WriteLine()` | Prints and moves to the next line           |
| `Console.Write()`     | Prints without moving to the next line      |
| `//`                  | Single-line comment                         |
| `/* */`               | Multi-line comment                          |
| Variable              | Stores data                                 |
| `Console.ReadLine()`  | Reads user input as a string                |
| `int.Parse()`         | Converts a string to an integer             |
| `double.Parse()`      | Converts a string to a double               |
| `Convert.ToInt32()`   | Converts a value to an integer              |
| `TryParse()`          | Safely attempts conversion                  |
| `$"..."`              | String interpolation                        |

---

## Key Takeaways

* C# is a programming language developed by Microsoft.
* C# runs on the .NET platform.
* `Console.WriteLine()` is used to display output.
* `Console.ReadLine()` is used to receive user input.
* `Console.ReadLine()` returns input as a string.
* String values can be converted to numeric types using methods such as `Parse()` and `Convert`.
* `TryParse()` is useful for safely handling invalid input.
* Variables are used to store data.
* String interpolation makes it easy to combine text with variables and expressions.
