### Step 1: What is C#?

C# (C-Sharp) is a programming language developed by Microsoft.

It is commonly used for:

Web applications
REST APIs
Desktop applications
Games with Unity
Cloud applications
Enterprise software
Backend development

C# runs on .NET, Microsoft's development platform.

A simple way to understand it:

C#       → Programming language
.NET     → Platform/runtime
ASP.NET  → Web development framework
EF Core  → Database/ORM framework

For example:

C# + .NET + ASP.NET Core + SQL Server
                    ↓
             Web Application
### Step 2: Your First C# Program

Modern C# allows a very simple program:

Console.WriteLine("Hello, World!");

Output:

Hello, World!
What does it mean?

Console refers to the console.

WriteLine() prints something to the console.

So:

Console.WriteLine("Hello, World!");

means:

Print "Hello, World!" on the screen.

Printing Multiple Values
Console.WriteLine("My name is Sandip");
Console.WriteLine("I am learning C#");
Console.WriteLine("C# is powerful");

Output:

My name is Sandip
I am learning C#
C# is powerful

You can also use:

Console.Write("Hello ");
Console.Write("World");

Output:

Hello World

The difference is:

Console.WriteLine();

prints and moves to the next line.

Console.Write();

prints without automatically moving to the next line.

### Step 3: Comments

Comments are ignored by C#.

Single-line comment
// This is a comment

Console.WriteLine("Hello");
Multi-line comment
/*
   This is a
   multi-line comment
*/

Console.WriteLine("Hello");

Comments are useful for explaining your code.

### Step 4: Variables

A variable stores data.

For example:

string name = "Sandip";
int age = 22;
double salary = 50000.50;
bool isStudent = true;

Think of a variable like a box:

name
 └── "Sandip"

age
 └── 22

salary
 └── 50000.50

isStudent
 └── true

You can print them:

string name = "Sandip";
int age = 22;

Console.WriteLine(name);
Console.WriteLine(age);

Output:

Sandip
22
Important C# Data Types
Type	Example	Used for
int	10	Whole numbers
double	10.5	Decimal numbers
float	10.5f	Decimal numbers
decimal	100.50m	Financial values
char	'A'	Single character
string	"Hello"	Text
bool	true	True/false

Example:

int age = 22;
double height = 5.8;
decimal price = 999.99m;
char grade = 'A';
string name = "Sandip";
bool isActive = true;
Step 5: String Interpolation

One of the most useful ways to combine variables and text is string interpolation.

string name = "Sandip";
int age = 22;

Console.WriteLine($"My name is {name} and I am {age} years old.");

Output:

My name is Sandip and I am 22 years old.

The $ tells C# that {} contains variables or expressions.

For example:

int a = 10;
int b = 20;

Console.WriteLine($"Sum = {a + b}");

Output:

Sum = 30

### C# Tutorial —Step 2: User Input & Type Conversion

Now let's learn how to take input from the user. This is important because real programs don't just display fixed values—they receive data from users.

1. Console.ReadLine()

We use Console.ReadLine() to get input.

Console.Write("Enter your name: ");

string name = Console.ReadLine();

Console.WriteLine($"Hello, {name}!");

Example:

Enter your name: Sandip
Hello, Sandip!
How it works
string name = Console.ReadLine();

The user enters something:

Sandip

and it gets stored in:

name
 ↓
"Sandip"
2. Important: ReadLine() Returns a String

Suppose we want the user's age:

Console.Write("Enter your age: ");

string age = Console.ReadLine();

Even if the user enters:

22

C# initially treats it as:

"22"

That's a string, not an integer.

So this won't work as expected:

Console.WriteLine(age + 5);

because you're trying to perform numeric operations on text.

3. Converting String to Integer

Use int.Parse():

Console.Write("Enter your age: ");

int age = int.Parse(Console.ReadLine());

Console.WriteLine($"You are {age} years old.");

Input:

Enter your age: 22

Output:

You are 22 years old.

The conversion is:

"22"
 ↓
int.Parse()
 ↓
22
4. Example: Add Two Numbers

Let's make a small calculator.

Console.Write("Enter first number: ");
int num1 = int.Parse(Console.ReadLine());

Console.Write("Enter second number: ");
int num2 = int.Parse(Console.ReadLine());

int sum = num1 + num2;

Console.WriteLine($"Sum = {sum}");

Example:

Enter first number: 10
Enter second number: 20

Sum = 30
5. Different Types of Conversion
String → int
int age = int.Parse("22");
String → double
double price = double.Parse("99.50");
String → decimal
decimal salary = decimal.Parse("50000.50");
String → bool
bool result = bool.Parse("true");
6. Convert Methods

You can also use Convert.

int age = Convert.ToInt32(Console.ReadLine());

double price = Convert.ToDouble(Console.ReadLine());

decimal salary = Convert.ToDecimal(Console.ReadLine());

For beginners, you'll commonly see both:

int.Parse()

and

Convert.ToInt32()
7. Parse() vs TryParse()

There is an important problem with Parse().

Suppose the user enters:

abc

when your program expects an integer:

int age = int.Parse(Console.ReadLine());

The program throws an exception.

A safer approach is TryParse():

Console.Write("Enter your age: ");

bool success = int.TryParse(Console.ReadLine(), out int age);

Console.WriteLine($"Age = {age}");

TryParse() returns:

true  → conversion successful
false → conversion failed

We'll study TryParse() properly when we reach exception handling and validation.

8. Mini Project: Simple Calculator

Let's combine what we've learned.

Console.Write("Enter first number: ");
double num1 = double.Parse(Console.ReadLine());

Console.Write("Enter second number: ");
double num2 = double.Parse(Console.ReadLine());

Console.WriteLine($"Addition = {num1 + num2}");
Console.WriteLine($"Subtraction = {num1 - num2}");
Console.WriteLine($"Multiplication = {num1 * num2}");
Console.WriteLine($"Division = {num1 / num2}");

Example:

Enter first number: 20
Enter second number: 5

Addition = 25
Subtraction = 15
Multiplication = 100
Division = 4
