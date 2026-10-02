# C# Fundamentals

A collection of C# fundamentals, examples, and practice programs for revision and quick reference.

---

# Step 1: What is C#?

**C# (C-Sharp)** is a programming language developed by Microsoft.

It is commonly used for:

* Web applications
* REST APIs
* Desktop applications
* Games with Unity
* Cloud applications
* Enterprise software
* Backend development

C# runs on **.NET**, Microsoft's development platform.

## C# and .NET Ecosystem

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

## What does it mean?

`Console` refers to the console.

`WriteLine()` prints something to the console and moves to the next line.

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

## WriteLine() vs Write()

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

A variable stores data.

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

String interpolation is a useful way to combine variables and text.

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

# Step 6: User Input & Type Conversion

Real programs don't just display fixed values. They receive data from users.

## 1. Console.ReadLine()

We use `Console.ReadLine()` to get input.

```csharp
Console.Write("Enter your name: ");

string name = Console.ReadLine();

Console.WriteLine($"Hello, {name}!");
```

Example:

```text
Enter your name: Sandip
Hello, Sandip!
```

### How it works

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

## 2. ReadLine() Returns a String

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

which is a string, not an integer.

---

## 3. Converting String to Integer

Use `int.Parse()`:

```csharp
Console.Write("Enter your age: ");

int age = int.Parse(Console.ReadLine());

Console.WriteLine($"You are {age} years old.");
```

Input:

```text
Enter your age: 22
```

Output:

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

## 4. Example: Add Two Numbers

```csharp
Console.Write("Enter first number: ");
int num1 = int.Parse(Console.ReadLine());

Console.Write("Enter second number: ");
int num2 = int.Parse(Console.ReadLine());

int sum = num1 + num2;

Console.WriteLine($"Sum = {sum}");
```

Example:

```text
Enter first number: 10
Enter second number: 20

Sum = 30
```

---

## 5. Different Types of Conversion

### String → int

```csharp
int age = int.Parse("22");
```

### String → double

```csharp
double price = double.Parse("99.50");
```

### String → decimal

```csharp
decimal salary = decimal.Parse("50000.50");
```

### String → bool

```csharp
bool result = bool.Parse("true");
```

---

## 6. Convert Methods

You can also use `Convert`:

```csharp
int age = Convert.ToInt32(Console.ReadLine());

double price = Convert.ToDouble(Console.ReadLine());

decimal salary = Convert.ToDecimal(Console.ReadLine());
```

For beginners, you'll commonly see:

```csharp
int.Parse()
```

and:

```csharp
Convert.ToInt32()
```

---

## 7. Parse() vs TryParse()

There is an important problem with `Parse()`.

If the user enters:

```text
abc
```

when your program expects an integer:

```csharp
int age = int.Parse(Console.ReadLine());
```

the program throws an exception.

A safer approach is `TryParse()`:

```csharp
Console.Write("Enter your age: ");

bool success = int.TryParse(Console.ReadLine(), out int age);

Console.WriteLine($"Age = {age}");
```

`TryParse()` returns:

```text
true  → conversion successful
false → conversion failed
```

---

## Mini Project: Simple Calculator

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

Example:

```text
Enter first number: 20
Enter second number: 5

Addition = 25
Subtraction = 15
Multiplication = 100
Division = 4
```

---

# Step 7: Operators

Operators are symbols that tell C# to perform an operation on values.

For example:

```csharp
int a = 10;
int b = 5;

Console.WriteLine(a + b);
```

Output:

```text
15
```

Here, `+` is an operator.

---

## 1. Arithmetic Operators

| Operator | Meaning        | Example  | Result |
| -------- | -------------- | -------- | -----: |
| `+`      | Addition       | `10 + 5` |   `15` |
| `-`      | Subtraction    | `10 - 5` |    `5` |
| `*`      | Multiplication | `10 * 5` |   `50` |
| `/`      | Division       | `10 / 5` |    `2` |
| `%`      | Remainder      | `10 % 3` |    `1` |

Example:

```csharp
int a = 10;
int b = 3;

Console.WriteLine(a + b);
Console.WriteLine(a - b);
Console.WriteLine(a * b);
Console.WriteLine(a / b);
Console.WriteLine(a % b);
```

Output:

```text
13
7
30
3
1
```

### The `%` Operator

The `%` operator returns the remainder.

```text
10 ÷ 3

3 × 3 = 9
remainder = 1
```

Therefore:

```csharp
Console.WriteLine(10 % 3);
```

Output:

```text
1
```

It is useful for checking even and odd numbers:

```csharp
int number = 10;

Console.WriteLine(number % 2);
```

Output:

```text
0
```

Therefore, `10` is even.

---

## 2. Integer Division

Be careful when dividing integers:

```csharp
int result = 5 / 2;

Console.WriteLine(result);
```

Output:

```text
2
```

Because both values are `int`, C# performs integer division.

If you want `2.5`:

```csharp
double result = 5.0 / 2;

Console.WriteLine(result);
```

Output:

```text
2.5
```

---

## 3. Assignment Operator

The basic assignment operator is:

```text
=
```

Example:

```csharp
int age = 22;
```

It means:

```text
Put the value 22 into the variable age.
```

### Compound Assignment

Instead of:

```csharp
int number = 10;

number = number + 5;
```

you can write:

```csharp
int number = 10;

number += 5;
```

Result:

```text
15
```

Other compound assignment operators:

```csharp
number -= 5;
number *= 5;
number /= 5;
number %= 5;
```

---

## 4. Increment and Decrement

### Increment `++`

```csharp
int number = 10;

number++;

Console.WriteLine(number);
```

Output:

```text
11
```

Equivalent to:

```csharp
number = number + 1;
```

### Decrement `--`

```csharp
int number = 10;

number--;

Console.WriteLine(number);
```

Output:

```text
9
```

Equivalent to:

```csharp
number = number - 1;
```

---

## 5. Comparison Operators

Comparison operators compare two values.

The result is always:

```text
true
```

or:

```text
false
```

| Operator | Meaning               |
| -------- | --------------------- |
| `==`     | Equal                 |
| `!=`     | Not equal             |
| `>`      | Greater than          |
| `<`      | Less than             |
| `>=`     | Greater than or equal |
| `<=`     | Less than or equal    |

Example:

```csharp
int a = 10;
int b = 20;

Console.WriteLine(a == b);
Console.WriteLine(a != b);
Console.WriteLine(a > b);
Console.WriteLine(a < b);
```

Output:

```text
False
True
False
True
```

### `=` vs `==`

This is very important.

`=` means assignment:

```csharp
int age = 22;
```

`==` means comparison:

```csharp
age == 22
```

Example:

```csharp
int age = 22;

Console.WriteLine(age == 22);
```

Output:

```text
True
```

---

## 6. Logical Operators

Logical operators combine conditions.

### AND `&&`

Both conditions must be true.

```csharp
int age = 22;

Console.WriteLine(age >= 18 && age <= 60);
```

Output:

```text
True
```

Because:

```text
age >= 18 → true
age <= 60 → true

true && true → true
```

### OR `||`

At least one condition must be true.

```csharp
int age = 16;

Console.WriteLine(age < 18 || age > 60);
```

Output:

```text
True
```

Because:

```text
age < 18 → true
age > 60 → false

true || false → true
```

### NOT `!`

Reverses the result.

```csharp
bool isStudent = true;

Console.WriteLine(!isStudent);
```

Output:

```text
False
```

Because:

```text
!true → false
```

---

## Real Example: Voting Age

```csharp
Console.Write("Enter your age: ");

int age = int.Parse(Console.ReadLine());

bool canVote = age >= 18;

Console.WriteLine($"Can vote: {canVote}");
```

Input:

```text
20
```

Output:

```text
Can vote: True
```

Input:

```text
15
```

Output:

```text
Can vote: False
```

---

# Step 8: Decision Making

Decision-making allows a program to execute different code depending on conditions.

Common decision-making statements include:

* `if`
* `if...else`
* `else if`
* `switch`
* Nested `if`

---

## 1. Basic if

Syntax:

```csharp
if (condition)
{
    // code
}
```

Example:

```csharp
int age = 20;

if (age >= 18)
{
    Console.WriteLine("You are an adult.");
}
```

Output:

```text
You are an adult.
```

If the condition is false, the code inside `{ }` doesn't execute.

---

## 2. if...else

```csharp
int age = 15;

if (age >= 18)
{
    Console.WriteLine("You are an adult.");
}
else
{
    Console.WriteLine("You are a minor.");
}
```

Output:

```text
You are a minor.
```

Flow:

```text
             age >= 18?
             /       \
          true       false
           ↓           ↓
       Adult         Minor
```

---

## 3. Taking Input

```csharp
Console.Write("Enter your age: ");

int age = int.Parse(Console.ReadLine());

if (age >= 18)
{
    Console.WriteLine("You are an adult.");
}
else
{
    Console.WriteLine("You are a minor.");
}
```

Example:

```text
Enter your age: 22
You are an adult.
```

---

## 4. else if

Sometimes there are multiple conditions.

Example:

```text
90+ → A
80+ → B
70+ → C
60+ → D
below 60 → F
```

Code:

```csharp
Console.Write("Enter your marks: ");

int marks = int.Parse(Console.ReadLine());

if (marks >= 90)
{
    Console.WriteLine("Grade A");
}
else if (marks >= 80)
{
    Console.WriteLine("Grade B");
}
else if (marks >= 70)
{
    Console.WriteLine("Grade C");
}
else if (marks >= 60)
{
    Console.WriteLine("Grade D");
}
else
{
    Console.WriteLine("Grade F");
}
```

If the user enters:

```text
85
```

Output:

```text
Grade B
```

C# checks conditions from top to bottom and executes the first matching branch.

---

## 5. Even or Odd

```csharp
Console.Write("Enter a number: ");

int number = int.Parse(Console.ReadLine());

if (number % 2 == 0)
{
    Console.WriteLine("Even");
}
else
{
    Console.WriteLine("Odd");
}
```

Input:

```text
17
```

Output:

```text
Odd
```

Because:

```text
17 % 2 = 1
```

---

## 6. Positive, Negative, or Zero

```csharp
Console.Write("Enter a number: ");

int number = int.Parse(Console.ReadLine());

if (number > 0)
{
    Console.WriteLine("Positive");
}
else if (number < 0)
{
    Console.WriteLine("Negative");
}
else
{
    Console.WriteLine("Zero");
}
```

Example:

```text
Enter a number: -10
Negative
```

---

## 7. Multiple Conditions

You can combine conditions using `&&` and `||`.

```csharp
Console.Write("Enter your age: ");

int age = int.Parse(Console.ReadLine());

if (age >= 18 && age <= 60)
{
    Console.WriteLine("Age is between 18 and 60.");
}
else
{
    Console.WriteLine("Age is outside the range.");
}
```

---

## 8. Nested if

An `if` inside another `if` is called a nested `if`.

```csharp
Console.Write("Enter your age: ");
int age = int.Parse(Console.ReadLine());

if (age >= 18)
{
    Console.Write("Do you have an ID? (yes/no): ");
    string hasId = Console.ReadLine();

    if (hasId == "yes")
    {
        Console.WriteLine("Access granted.");
    }
    else
    {
        Console.WriteLine("ID required.");
    }
}
else
{
    Console.WriteLine("You are under 18.");
}
```

Flow:

```text
age >= 18?
    |
    ├── No → Under 18
    |
    └── Yes
         |
         └── has ID?
               |
               ├── yes → Access granted
               └── no  → ID required
```

---

## 9. switch

`switch` is useful when you have several fixed choices.

Example:

```text
1 → Add
2 → Subtract
3 → Multiply
4 → Divide
```

Code:

```csharp
Console.WriteLine("1. Add");
Console.WriteLine("2. Subtract");
Console.WriteLine("3. Multiply");
Console.WriteLine("4. Divide");

Console.Write("Choose an option: ");

int choice = int.Parse(Console.ReadLine());

switch (choice)
{
    case 1:
        Console.WriteLine("Addition selected");
        break;

    case 2:
        Console.WriteLine("Subtraction selected");
        break;

    case 3:
        Console.WriteLine("Multiplication selected");
        break;

    case 4:
        Console.WriteLine("Division selected");
        break;

    default:
        Console.WriteLine("Invalid choice");
        break;
}
```

If the user enters:

```text
3
```

Output:

```text
Multiplication selected
```

### Why `break`?

`break` tells C# to stop the current `switch` branch.

---

## 10. if vs switch

### Use `if/else`

When checking conditions or ranges:

```csharp
if (marks >= 80)
```

```csharp
if (age >= 18 && age <= 60)
```

### Use `switch`

When selecting between specific values or options:

```csharp
switch (choice)
{
    case 1:
    case 2:
    case 3:
}
```

---

## Mini Project: Student Grade Calculator

```csharp
Console.Write("Enter your name: ");
string name = Console.ReadLine();

Console.Write("Enter your marks: ");
double marks = double.Parse(Console.ReadLine());

Console.WriteLine();

Console.WriteLine($"Student: {name}");
Console.WriteLine($"Marks: {marks}");

if (marks >= 90)
{
    Console.WriteLine("Grade: A+");
}
else if (marks >= 80)
{
    Console.WriteLine("Grade: A");
}
else if (marks >= 70)
{
    Console.WriteLine("Grade: B");
}
else if (marks >= 60)
{
    Console.WriteLine("Grade: C");
}
else if (marks >= 40)
{
    Console.WriteLine("Grade: D");
}
else
{
    Console.WriteLine("Grade: F");
}
```

Example:

```text
Enter your name: Sandip
Enter your marks: 82

Student: Sandip
Marks: 82
Grade: A
```

---

# Step 9: Loops

A loop allows you to execute the same block of code repeatedly.

Without a loop:

```csharp
Console.WriteLine(1);
Console.WriteLine(2);
Console.WriteLine(3);
Console.WriteLine(4);
Console.WriteLine(5);
```

With a loop:

```csharp
for (int i = 1; i <= 5; i++)
{
    Console.WriteLine(i);
}
```

There are three main loops in C#:

* `for`
* `while`
* `do-while`

---

## 1. for Loop

The `for` loop is commonly used when you know how many times you want to repeat something.

Syntax:

```csharp
for (initialization; condition; update)
{
    // code
}
```

Example:

```csharp
for (int i = 1; i <= 5; i++)
{
    Console.WriteLine(i);
}
```

Output:

```text
1
2
3
4
5
```

### Three Parts

```csharp
for (int i = 1; i <= 5; i++)
```

**Initialization:**

```csharp
int i = 1;
```

The loop starts with `i = 1`.

**Condition:**

```csharp
i <= 5
```

The loop continues while this condition is true.

**Update:**

```csharp
i++
```

After each iteration, `i` increases by 1.

Flow:

```text
i = 1 → print 1
i = 2 → print 2
i = 3 → print 3
i = 4 → print 4
i = 5 → print 5
i = 6 → condition false → stop
```

---

## 2. Counting Backwards

```csharp
for (int i = 5; i >= 1; i--)
{
    Console.WriteLine(i);
}
```

Output:

```text
5
4
3
2
1
```

---

## 3. Print Even Numbers

```csharp
for (int i = 2; i <= 10; i += 2)
{
    Console.WriteLine(i);
}
```

Output:

```text
2
4
6
8
10
```

Here:

```csharp
i += 2;
```

means:

```csharp
i = i + 2;
```

---

## 4. Multiplication Table

```csharp
Console.Write("Enter a number: ");

int number = int.Parse(Console.ReadLine());

for (int i = 1; i <= 10; i++)
{
    Console.WriteLine($"{number} × {i} = {number * i}");
}
```

Input:

```text
5
```

Output:

```text
5 × 1 = 5
5 × 2 = 10
5 × 3 = 15
5 × 4 = 20
5 × 5 = 25
5 × 6 = 30
5 × 7 = 35
5 × 8 = 40
5 × 9 = 45
5 × 10 = 50
```

---

## 5. while Loop

A `while` loop repeats while a condition is true.

Syntax:

```csharp
while (condition)
{
    // code
}
```

Example:

```csharp
int i = 1;

while (i <= 5)
{
    Console.WriteLine(i);

    i++;
}
```

Output:

```text
1
2
3
4
5
```

Flow:

```text
i = 1
 ↓
i <= 5?
 ↓
true → print
 ↓
i++
 ↓
check again
```

Eventually:

```text
i = 6
6 <= 5 → false
       ↓
      stop
```

### Infinite Loop Warning

Be careful with:

```csharp
int i = 1;

while (i <= 5)
{
    Console.WriteLine(i);
}
```

This is an infinite loop because `i` never changes.

You need:

```csharp
i++;
```

---

## 6. do-while Loop

The difference between `while` and `do-while` is important.

### while

It checks the condition before executing.

```csharp
int i = 10;

while (i < 5)
{
    Console.WriteLine(i);
}
```

Nothing is printed because:

```text
10 < 5 → false
```

### do-while

It executes at least once before checking the condition.

```csharp
int i = 10;

do
{
    Console.WriteLine(i);

    i++;
}
while (i < 5);
```

Output:

```text
10
```

Even though:

```text
10 < 5 → false
```

the code executed once.

---

## 7. for vs while vs do-while

| Loop       | Best Used When                        |
| ---------- | ------------------------------------- |
| `for`      | Number of repetitions is known        |
| `while`    | Repeat while a condition remains true |
| `do-while` | Code must execute at least once       |

Examples:

### for

```csharp
for (int i = 1; i <= 10; i++)
```

Good for printing numbers 1–10.

### while

```csharp
while (password != "1234")
```

Good for repeatedly asking for input until a condition is satisfied.

### do-while

```csharp
do
{
    // show menu
}
while (choice != 0);
```

Good for menus that should be displayed at least once.

---

## 8. break

`break` immediately stops a loop.

```csharp
for (int i = 1; i <= 10; i++)
{
    if (i == 5)
    {
        break;
    }

    Console.WriteLine(i);
}
```

Output:

```text
1
2
3
4
```

When `i` becomes 5:

```text
break
 ↓
loop stops
```

---

## 9. continue

`continue` skips the current iteration and moves to the next one.

```csharp
for (int i = 1; i <= 5; i++)
{
    if (i == 3)
    {
        continue;
    }

    Console.WriteLine(i);
}
```

Output:

```text
1
2
4
5
```

When `i == 3`, C# skips the current iteration.

---

## 10. Nested Loops

A loop inside another loop is called a nested loop.

```csharp
for (int i = 1; i <= 3; i++)
{
    for (int j = 1; j <= 3; j++)
    {
        Console.WriteLine($"i = {i}, j = {j}");
    }
}
```

Output:

```text
i = 1, j = 1
i = 1, j = 2
i = 1, j = 3

i = 2, j = 1
i = 2, j = 2
i = 2, j = 3

i = 3, j = 1
i = 3, j = 2
i = 3, j = 3
```

Nested loops are useful for:

* Patterns
* Matrices
* Tables
* 2D arrays
* Algorithms

---

## Mini Project: Number Guessing Game

```csharp
int secretNumber = 7;
int guess = 0;

while (guess != secretNumber)
{
    Console.Write("Guess the number: ");

    guess = int.Parse(Console.ReadLine());

    if (guess > secretNumber)
    {
        Console.WriteLine("Too high!");
    }
    else if (guess < secretNumber)
    {
        Console.WriteLine("Too low!");
    }
    else
    {
        Console.WriteLine("Correct!");
    }
}
```

Example:

```text
Guess the number: 10
Too high!

Guess the number: 5
Too low!

Guess the number: 7
Correct!
```

---

# Step 10: Arrays

An array stores multiple values of the same data type in a single variable.

Without an array:

```csharp
int mark1 = 80;
int mark2 = 75;
int mark3 = 90;
int mark4 = 65;
int mark5 = 88;
```

With an array:

```csharp
int[] marks = { 80, 75, 90, 65, 88 };
```

Much easier.

---

## 1. Creating an Array

```csharp
int[] numbers = { 10, 20, 30, 40, 50 };
```

Think of it like:

```text
Index:    0    1    2    3    4
          ↓    ↓    ↓    ↓    ↓
Value:   10   20   30   40   50
```

C# arrays start at **index 0**, not 1.

---

## 2. Accessing Array Elements

```csharp
int[] numbers = { 10, 20, 30, 40, 50 };

Console.WriteLine(numbers[0]);
Console.WriteLine(numbers[1]);
Console.WriteLine(numbers[4]);
```

Output:

```text
10
20
50
```

Because:

```text
numbers[0] → 10
numbers[1] → 20
numbers[2] → 30
numbers[3] → 40
numbers[4] → 50
```

Trying:

```csharp
numbers[5]
```

will cause an `IndexOutOfRangeException` because index 5 doesn't exist.

---

## 3. Changing an Array Value

Arrays are mutable.

```csharp
int[] numbers = { 10, 20, 30 };

numbers[1] = 100;

Console.WriteLine(numbers[1]);
```

Output:

```text
100
```

The array becomes:

```text
10
100
30
```

---

## 4. Array Length

Use `.Length` to find the number of elements.

```csharp
int[] numbers = { 10, 20, 30, 40, 50 };

Console.WriteLine(numbers.Length);
```

Output:

```text
5
```

---

## 5. Loop Through an Array

```csharp
int[] numbers = { 10, 20, 30, 40, 50 };

for (int i = 0; i < numbers.Length; i++)
{
    Console.WriteLine(numbers[i]);
}
```

Output:

```text
10
20
30
40
50
```

Why:

```csharp
i < numbers.Length
```

For an array of length 5:

```text
Length = 5
Indexes = 0, 1, 2, 3, 4
```

---

## 6. foreach Loop

C# provides an easier way to loop through arrays:

```csharp
int[] numbers = { 10, 20, 30, 40, 50 };

foreach (int number in numbers)
{
    Console.WriteLine(number);
}
```

Output:

```text
10
20
30
40
50
```

Read it as:

> For each number inside `numbers`, execute the following code.

### for vs foreach

Use `for` when you need the index:

```csharp
for (int i = 0; i < numbers.Length; i++)
{
    Console.WriteLine($"Index {i}: {numbers[i]}");
}
```

Use `foreach` when you simply need each value:

```csharp
foreach (int number in numbers)
{
    Console.WriteLine(number);
}
```

---

## 7. Creating an Array with a Fixed Size

You don't have to provide the values immediately.

```csharp
int[] marks = new int[5];
```

This creates space for 5 integers.

Initially:

```text
0
0
0
0
0
```

You can assign values:

```csharp
marks[0] = 80;
marks[1] = 75;
marks[2] = 90;
marks[3] = 65;
marks[4] = 88;
```

---

## 8. Taking Array Input from User

```csharp
Console.Write("How many numbers? ");

int size = int.Parse(Console.ReadLine());

int[] numbers = new int[size];

for (int i = 0; i < numbers.Length; i++)
{
    Console.Write($"Enter number {i + 1}: ");
    numbers[i] = int.Parse(Console.ReadLine());
}

Console.WriteLine("Numbers:");

foreach (int number in numbers)
{
    Console.WriteLine(number);
}
```

Example:

```text
How many numbers? 3
Enter number 1: 10
Enter number 2: 20
Enter number 3: 30

Numbers:
10
20
30
```

---

## 9. Find Sum of Array

```csharp
int[] numbers = { 10, 20, 30, 40, 50 };

int sum = 0;

foreach (int number in numbers)
{
    sum += number;
}

Console.WriteLine($"Sum = {sum}");
```

Output:

```text
Sum = 150
```

Calculation:

```text
sum = 0

0 + 10 = 10
10 + 20 = 30
30 + 30 = 60
60 + 40 = 100
100 + 50 = 150
```

---

## 10. Calculate Average

```csharp
int[] marks = { 80, 75, 90, 65, 88 };

int sum = 0;

foreach (int mark in marks)
{
    sum += mark;
}

double average = (double)sum / marks.Length;

Console.WriteLine($"Average = {average}");
```

Output:

```text
Average = 79.6
```

`(double)` converts the integer value to a `double` before division.

---

## 11. Find Largest Number

```csharp
int[] numbers = { 10, 50, 25, 80, 35 };

int largest = numbers[0];

foreach (int number in numbers)
{
    if (number > largest)
    {
        largest = number;
    }
}

Console.WriteLine($"Largest = {largest}");
```

Output:

```text
Largest = 80
```

---

## 12. Find Smallest Number

```csharp
int[] numbers = { 10, 50, 25, 80, 35 };

int smallest = numbers[0];

foreach (int number in numbers)
{
    if (number < smallest)
    {
        smallest = number;
    }
}

Console.WriteLine($"Smallest = {smallest}");
```

Output:

```text
Smallest = 10
```

---

## 13. Search for a Number

```csharp
int[] numbers = { 10, 20, 30, 40, 50 };

Console.Write("Enter number to search: ");
int search = int.Parse(Console.ReadLine());

bool found = false;

foreach (int number in numbers)
{
    if (number == search)
    {
        found = true;
        break;
    }
}

if (found)
{
    Console.WriteLine("Number found.");
}
else
{
    Console.WriteLine("Number not found.");
}
```

Example:

```text
Enter number to search: 30
Number found.
```

---

## 14. String Arrays

Arrays aren't only for numbers.

```csharp
string[] names =
{
    "Sandip",
    "Ram",
    "Hari",
    "Sita"
};
```

Loop:

```csharp
foreach (string name in names)
{
    Console.WriteLine(name);
}
```

Output:

```text
Sandip
Ram
Hari
Sita
```

---

## Mini Project: Student Marks System

```csharp
Console.Write("Enter number of students: ");
int size = int.Parse(Console.ReadLine());

double[] marks = new double[size];

for (int i = 0; i < marks.Length; i++)
{
    Console.Write($"Enter marks for student {i + 1}: ");
    marks[i] = double.Parse(Console.ReadLine());
}

double sum = 0;
double highest = marks[0];
double lowest = marks[0];

foreach (double mark in marks)
{
    sum += mark;

    if (mark > highest)
    {
        highest = mark;
    }

    if (mark < lowest)
    {
        lowest = mark;
    }
}

double average = sum / marks.Length;

Console.WriteLine();
Console.WriteLine("===== Result =====");
Console.WriteLine($"Total Marks = {sum}");
Console.WriteLine($"Average = {average}");
Console.WriteLine($"Highest = {highest}");
Console.WriteLine($"Lowest = {lowest}");
```

Example:

```text
Enter number of students: 4
Enter marks for student 1: 80
Enter marks for student 2: 65
Enter marks for student 3: 90
Enter marks for student 4: 75

===== Result =====
Total Marks = 310
Average = 77.5
Highest = 90
Lowest = 65
```

---

# Step 11: Strings

A string is a sequence of characters used to store text.

```csharp
string name = "Sandip";
string message = "Hello World!";
```

Strings are commonly used for:

* Names
* Emails
* Passwords
* URLs
* Messages
* JSON
* Database values

---

## 1. String Length

Use `.Length` to find the number of characters.

```csharp
string name = "Sandip";

Console.WriteLine(name.Length);
```

Output:

```text
6
```

---

## 2. Convert to Uppercase

Use `.ToUpper()`:

```csharp
string name = "Sandip";

Console.WriteLine(name.ToUpper());
```

Output:

```text
SANDIP
```

---

## 3. Convert to Lowercase

Use `.ToLower()`:

```csharp
string name = "SANDIP";

Console.WriteLine(name.ToLower());
```

Output:

```text
sandip
```

Example:

```csharp
string answer = "YES";

if (answer.ToLower() == "yes")
{
    Console.WriteLine("You selected yes.");
}
```

---

## 4. Access Individual Characters

Strings have indexes starting from 0.

```csharp
string name = "Sandip";

Console.WriteLine(name[0]);
Console.WriteLine(name[1]);
Console.WriteLine(name[5]);
```

Output:

```text
S
a
p
```

Indexes:

```text
String:  S  a  n  d  i  p
Index:   0  1  2  3  4  5
```

You can also loop through a string:

```csharp
string name = "Sandip";

for (int i = 0; i < name.Length; i++)
{
    Console.WriteLine(name[i]);
}
```

---

## 5. Contains()

Checks whether a string contains specific text.

```csharp
string message = "I am learning C#";

Console.WriteLine(message.Contains("C#"));
```

Output:

```text
True
```

Example:

```csharp
string email = "sandip@gmail.com";

if (email.Contains("@"))
{
    Console.WriteLine("Valid email format.");
}
```

This is only a basic check and is not complete email validation.

---

## 6. StartsWith()

Checks whether a string starts with specific text.

```csharp
string website = "https://example.com";

Console.WriteLine(website.StartsWith("https"));
```

Output:

```text
True
```

---

## 7. EndsWith()

Checks whether a string ends with specific text.

```csharp
string file = "photo.jpg";

Console.WriteLine(file.EndsWith(".jpg"));
```

Output:

```text
True
```

---

## 8. Replace()

Replaces part of a string.

```csharp
string message = "I like Java";

string result = message.Replace("Java", "C#");

Console.WriteLine(result);
```

Output:

```text
I like C#
```

Another example:

```csharp
string phone = "9841-234-567";

phone = phone.Replace("-", "");

Console.WriteLine(phone);
```

Output:

```text
9841234567
```

---

## 9. Trim()

`Trim()` removes whitespace from the beginning and end.

```csharp
string name = "   Sandip   ";

Console.WriteLine(name.Trim());
```

Output:

```text
Sandip
```

This is especially useful for user input:

```csharp
Console.Write("Enter your name: ");

string name = Console.ReadLine().Trim();

Console.WriteLine($"Hello {name}");
```

---

## 10. Substring()

`Substring()` extracts part of a string.

```csharp
string name = "Sandip";

string result = name.Substring(0, 3);

Console.WriteLine(result);
```

Output:

```text
San
```

Syntax:

```csharp
Substring(startIndex, length)
```

So:

```csharp
name.Substring(0, 3)
```

means:

```text
Start at index 0 and take 3 characters.
```

---

## 11. Split()

`Split()` divides a string into multiple pieces.

```csharp
string fruits = "Apple,Banana,Mango";

string[] result = fruits.Split(',');
```

Now:

```text
result[0] → Apple
result[1] → Banana
result[2] → Mango
```

Loop:

```csharp
foreach (string fruit in result)
{
    Console.WriteLine(fruit);
}
```

Output:

```text
Apple
Banana
Mango
```

---

## 12. Comparing Strings

You can use `==`:

```csharp
string username = "admin";

if (username == "admin")
{
    Console.WriteLine("Correct username");
}
```

### Case Sensitivity

Normally:

```text
"Admin" == "admin"
```

is:

```text
False
```

For a simple case-insensitive comparison:

```csharp
string username = "ADMIN";

if (username.Equals("admin", StringComparison.OrdinalIgnoreCase))
{
    Console.WriteLine("Username matches.");
}
```

---

## 13. String Interpolation

```csharp
string name = "Sandip";
int age = 22;

Console.WriteLine($"My name is {name} and I am {age} years old.");
```

Output:

```text
My name is Sandip and I am 22 years old.
```

You can also perform calculations:

```csharp
int a = 10;
int b = 20;

Console.WriteLine($"Sum = {a + b}");
```

Output:

```text
Sum = 30
```

---

## 14. String Concatenation

You can combine strings using `+`:

```csharp
string firstName = "Sandip";
string lastName = "Kushwaha";

string fullName = firstName + " " + lastName;

Console.WriteLine(fullName);
```

Output:

```text
Sandip Kushwaha
```

String interpolation is often easier to read:

```csharp
string fullName = $"{firstName} {lastName}";
```

---

## Useful String Methods

| Method          | Purpose                       |
| --------------- | ----------------------------- |
| `.Length`       | Get number of characters      |
| `.ToUpper()`    | Convert to uppercase          |
| `.ToLower()`    | Convert to lowercase          |
| `.Contains()`   | Check if text exists          |
| `.StartsWith()` | Check beginning               |
| `.EndsWith()`   | Check ending                  |
| `.Replace()`    | Replace text                  |
| `.Trim()`       | Remove surrounding whitespace |
| `.Substring()`  | Extract part of string        |
| `.Split()`      | Divide string                 |
| `.Equals()`     | Compare strings               |

---

## Mini Project: Username Validator

```csharp
Console.Write("Enter username: ");

string username = Console.ReadLine().Trim();

if (username.Length < 3)
{
    Console.WriteLine("Username must contain at least 3 characters.");
}
else if (username.Contains(" "))
{
    Console.WriteLine("Username cannot contain spaces.");
}
else
{
    Console.WriteLine($"Username '{username}' is valid.");
}
```

Example:

```text
Enter username: sa
Username must contain at least 3 characters.
```

Another:

```text
Enter username: sandip
Username 'sandip' is valid.
```

---

## Mini Project: Word Counter

```csharp
Console.Write("Enter a sentence: ");

string sentence = Console.ReadLine().Trim();

string[] words = sentence.Split(
    ' ',
    StringSplitOptions.RemoveEmptyEntries
);

Console.WriteLine($"Number of words: {words.Length}");
```

Input:

```text
I am learning C Sharp
```

Output:

```text
Number of words: 5
```

`RemoveEmptyEntries` prevents multiple spaces from creating empty elements.

---

# Step 12: Methods / Functions

A method lets us put a specific task into a reusable block of code.

Think of it like:

```text
Method
  ↓
Does one specific job
  ↓
Can be called whenever needed
```

Example:

```csharp
static void SayHello()
{
    Console.WriteLine("Hello!");
}
```

Call it:

```csharp
SayHello();
```

Output:

```text
Hello!
```

---

## 1. Why Do We Need Methods?

Without methods:

```csharp
Console.WriteLine("Welcome!");
Console.WriteLine("Welcome!");

int a = 10;
int b = 20;
Console.WriteLine(a + b);

int x = 30;
int y = 40;
Console.WriteLine(x + y);
```

With methods:

```csharp
static void Welcome()
{
    Console.WriteLine("Welcome!");
}

static int Add(int a, int b)
{
    return a + b;
}

Welcome();
Welcome();

Console.WriteLine(Add(10, 20));
Console.WriteLine(Add(30, 40));
```

Methods make code:

* Reusable
* Easier to read
* Easier to test
* Easier to maintain

---

## 2. Creating a Basic Method

```csharp
static void SayHello()
{
    Console.WriteLine("Hello, Sandip!");
}
```

Here:

```text
static    → Can be called without creating an object
void      → Doesn't return a value
SayHello  → Method name
()        → No parameters
```

Call it:

```csharp
SayHello();
```

Complete example:

```csharp
class Program
{
    static void SayHello()
    {
        Console.WriteLine("Hello, Sandip!");
    }

    static void Main()
    {
        SayHello();
    }
}
```

Modern C# also supports top-level statements, so methods may appear alongside top-level code depending on the project style.

---

## 3. Method with Parameters

A parameter allows us to send data into a method.

```csharp
static void Greet(string name)
{
    Console.WriteLine($"Hello, {name}!");
}
```

Call:

```csharp
Greet("Sandip");
Greet("Ram");
```

Output:

```text
Hello, Sandip!
Hello, Ram!
```

Here:

```csharp
string name
```

is the parameter.

And:

```text
"Sandip"
```

is the argument passed to the method.

---

## 4. Multiple Parameters

```csharp
static void PrintStudent(string name, int age)
{
    Console.WriteLine($"Name: {name}");
    Console.WriteLine($"Age: {age}");
}
```

Call:

```csharp
PrintStudent("Sandip", 22);
```

Output:

```text
Name: Sandip
Age: 22
```

---

## 5. Methods That Return a Value

Methods can return data.

```csharp
static int Add(int a, int b)
{
    return a + b;
}
```

Call:

```csharp
int result = Add(10, 20);

Console.WriteLine(result);
```

Output:

```text
30
```

Flow:

```text
Add(10, 20)
     ↓
10 + 20
     ↓
30
     ↓
return 30
     ↓
result
```

---

## 6. Understanding return

```csharp
static int Square(int number)
{
    return number * number;
}
```

When we call:

```csharp
int result = Square(5);
```

C# calculates:

```text
5 × 5
 ↓
25
```

and returns:

```text
25
```

---

## 7. Different Return Types

### int

```csharp
static int GetAge()
{
    return 22;
}
```

### double

```csharp
static double CalculateArea(double length, double width)
{
    return length * width;
}
```

### string

```csharp
static string GetName()
{
    return "Sandip";
}
```

### bool

```csharp
static bool IsAdult(int age)
{
    return age >= 18;
}
```

Usage:

```csharp
bool result = IsAdult(22);

Console.WriteLine(result);
```

Output:

```text
True
```

---

## 8. void vs Return Value

### void

Does something but doesn't return a value.

```csharp
static void PrintMessage()
{
    Console.WriteLine("Hello");
}
```

### int

Returns an integer.

```csharp
static int Add(int a, int b)
{
    return a + b;
}
```

Think:

```text
void
 ↓
Do something

int
 ↓
Calculate something
 ↓
Give me an integer back
```

---

## 9. Method with User Input

```csharp
static int Add(int a, int b)
{
    return a + b;
}

Console.Write("Enter first number: ");
int num1 = int.Parse(Console.ReadLine());

Console.Write("Enter second number: ");
int num2 = int.Parse(Console.ReadLine());

int result = Add(num1, num2);

Console.WriteLine($"Result = {result}");
```

Example:

```text
Enter first number: 20
Enter second number: 30
Result = 50
```

---

## 10. Calculator Using Methods

```csharp
static double Add(double a, double b)
{
    return a + b;
}

static double Subtract(double a, double b)
{
    return a - b;
}

static double Multiply(double a, double b)
{
    return a * b;
}

static double Divide(double a, double b)
{
    return a / b;
}
```

Then:

```csharp
Console.Write("Enter first number: ");
double a = double.Parse(Console.ReadLine());

Console.Write("Enter second number: ");
double b = double.Parse(Console.ReadLine());

Console.WriteLine($"Addition = {Add(a, b)}");
Console.WriteLine($"Subtraction = {Subtract(a, b)}");
Console.WriteLine($"Multiplication = {Multiply(a, b)}");
Console.WriteLine($"Division = {Divide(a, b)}");
```

This is easier to maintain than putting every calculation together.

---

## 11. Method Overloading

C# allows multiple methods with the same name as long as their parameter lists are different.

```csharp
static int Add(int a, int b)
{
    return a + b;
}

static int Add(int a, int b, int c)
{
    return a + b + c;
}
```

Now:

```csharp
Console.WriteLine(Add(10, 20));
Console.WriteLine(Add(10, 20, 30));
```

Output:

```text
30
60
```

C# determines which method to use based on the arguments.

---

## 12. Another Overloading Example

```csharp
static void Print(int number)
{
    Console.WriteLine($"Number: {number}");
}

static void Print(string text)
{
    Console.WriteLine($"Text: {text}");
}
```

Then:

```csharp
Print(100);
Print("Hello");
```

Output:

```text
Number: 100
Text: Hello
```

This is called **method overloading**.

---

## 13. ref Parameter

Normally, C# passes arguments by value.

Example:

```csharp
static void ChangeNumber(int number)
{
    number = 100;
}

int x = 10;

ChangeNumber(x);

Console.WriteLine(x);
```

Output:

```text
10
```

The original `x` did not change.

With `ref`:

```csharp
static void ChangeNumber(ref int number)
{
    number = 100;
}

int x = 10;

ChangeNumber(ref x);

Console.WriteLine(x);
```

Output:

```text
100
```

`ref` is an important parameter-passing concept.

---

## 14. Expression-Bodied Methods

For very simple methods, C# provides shorter syntax.

Instead of:

```csharp
static int Square(int number)
{
    return number * number;
}
```

you can write:

```csharp
static int Square(int number) => number * number;
```

Then:

```csharp
Console.WriteLine(Square(5));
```

Output:

```text
25
```

---

## Mini Project: Student Result System

This project combines:

* Input
* Methods
* `if/else`
* Return values

```csharp
static string GetGrade(double marks)
{
    if (marks >= 90)
        return "A+";
    else if (marks >= 80)
        return "A";
    else if (marks >= 70)
        return "B";
    else if (marks >= 60)
        return "C";
    else if (marks >= 40)
        return "D";
    else
        return "F";
}

static bool IsPassed(double marks)
{
    return marks >= 40;
}

Console.Write("Enter student name: ");
string name = Console.ReadLine();

Console.Write("Enter marks: ");
double marks = double.Parse(Console.ReadLine());

string grade = GetGrade(marks);
bool passed = IsPassed(marks);

Console.WriteLine();
Console.WriteLine("===== Result =====");
Console.WriteLine($"Name: {name}");
Console.WriteLine($"Marks: {marks}");
Console.WriteLine($"Grade: {grade}");
Console.WriteLine($"Passed: {passed}");
```

Example:

```text
Enter student name: Sandip
Enter marks: 85

===== Result =====
Name: Sandip
Marks: 85
Grade: A
Passed: True
```

The responsibilities are separated:

```text
GetGrade()
    ↓
Calculates grade

IsPassed()
    ↓
Checks pass/fail
```

This is the beginning of writing clean and modular C# code.

---

# Quick Revision

## C# Basics

```text
C#              → Programming language
.NET            → Platform / Runtime
Console         → Console input/output
WriteLine()     → Prints and moves to a new line
Write()         → Prints without moving to a new line
//              → Single-line comment
/* */           → Multi-line comment
Variable        → Stores data
$"..."          → String interpolation
```

## Input & Conversion

```text
Console.ReadLine() → Reads user input as a string
int.Parse()        → Converts string to int
double.Parse()     → Converts string to double
decimal.Parse()    → Converts string to decimal
bool.Parse()       → Converts string to bool
TryParse()         → Safely attempts conversion
```

## Operators

```text
+   → Addition
-   → Subtraction
*   → Multiplication
/   → Division
%   → Remainder

=   → Assignment
==  → Equal
!=  → Not equal
>   → Greater than
<   → Less than
>=  → Greater than or equal
<=  → Less than or equal

&&  → AND
||  → OR
!   → NOT

++  → Increment
--  → Decrement
```

## Decision Making

```text
if
else
else if
switch
nested if
```

## Loops

```text
for
while
do-while
break
continue
nested loops
```

## Arrays

```text
int[] numbers = { 10, 20, 30 };
```

Important concepts:

```text
Index
Length
for
foreach
Array input
Sum
Average
Largest
Smallest
Search
```

## Strings

Important methods:

```text
.Length
.ToUpper()
.ToLower()
.Contains()
.StartsWith()
.EndsWith()
.Replace()
.Trim()
.Substring()
.Split()
.Equals()
```

## Methods

```text
void methods
Parameters
Arguments
Return values
Method overloading
ref parameters
Expression-bodied methods
```
