# C# Exception Handling

Exception handling is used to handle **runtime errors** without unexpectedly terminating a program.

Instead of allowing an application to crash, C# provides mechanisms such as:

* `try`
* `catch`
* `finally`
* `throw`
* Custom exceptions

---

## 1. What Is an Exception?

An **exception** is an unexpected condition that occurs while a program is running.

For example:

```csharp
int a = 10;
int b = 0;

int result = a / b;
```

This causes:

```text
System.DivideByZeroException
```

Without exception handling, the application may terminate.

With exception handling, we can handle the problem gracefully.

---

## 2. Common Exceptions

| Exception                   | Example                             |
| --------------------------- | ----------------------------------- |
| `DivideByZeroException`     | Dividing a number by zero           |
| `FormatException`           | Invalid string-to-number conversion |
| `OverflowException`         | Value exceeds numeric range         |
| `NullReferenceException`    | Accessing a member of `null`        |
| `IndexOutOfRangeException`  | Invalid array index                 |
| `ArgumentException`         | Invalid method argument             |
| `ArgumentNullException`     | `null` argument where not allowed   |
| `InvalidOperationException` | Invalid operation for current state |
| `KeyNotFoundException`      | Missing dictionary key              |
| `FileNotFoundException`     | Requested file does not exist       |
| `IOException`               | General input/output problem        |

---

# 3. `try` Statement

The `try` block contains code that **might throw an exception**.

```csharp
try
{
    int a = 10;
    int b = 0;

    int result = a / b;
}
```

If an exception occurs inside `try`, C# looks for a matching `catch`.

---

# 4. `catch` Statement

The `catch` block handles an exception.

```csharp
try
{
    int a = 10;
    int b = 0;

    int result = a / b;
}
catch (DivideByZeroException)
{
    Console.WriteLine("Cannot divide by zero.");
}
```

Output:

```text
Cannot divide by zero.
```

The application can continue instead of immediately terminating.

---

# 5. Getting Exception Information

A `catch` block can receive an exception object.

```csharp
try
{
    int a = 10;
    int b = 0;

    int result = a / b;
}
catch (DivideByZeroException ex)
{
    Console.WriteLine(ex.Message);
}
```

### Important properties

```csharp
ex.Message
```

Returns the error message.

```csharp
ex.GetType()
```

Returns the exception type.

```csharp
ex.StackTrace
```

Provides information about where the exception occurred.

Example:

```csharp
catch (Exception ex)
{
    Console.WriteLine($"Type: {ex.GetType()}");
    Console.WriteLine($"Message: {ex.Message}");
}
```

---

# 6. Multiple `catch` Blocks

A single `try` can have multiple `catch` blocks.

```csharp
try
{
    Console.Write("Enter a number: ");

    int number = int.Parse(Console.ReadLine());

    int result = 100 / number;

    Console.WriteLine($"Result: {result}");
}
catch (FormatException)
{
    Console.WriteLine("Please enter a valid number.");
}
catch (DivideByZeroException)
{
    Console.WriteLine("Number cannot be zero.");
}
```

Different exceptions can be handled differently.

---

# 7. Order of `catch` Blocks

Specific exceptions should come before general exceptions.

Correct:

```csharp
try
{
    // code
}
catch (FormatException)
{
    // handle format problem
}
catch (Exception)
{
    // handle other exceptions
}
```

Incorrect:

```csharp
try
{
    // code
}
catch (Exception)
{
}
catch (FormatException)
{
}
```

The second example is invalid because `Exception` can already catch `FormatException`.

### Rule

```text
Specific Exception
        ↓
More General Exception
```

---

# 8. The `Exception` Base Class

Most C# exceptions derive from:

```csharp
System.Exception
```

Example:

```csharp
try
{
    // risky code
}
catch (Exception ex)
{
    Console.WriteLine(ex.Message);
}
```

This can catch many different exception types.

However, avoid using `catch (Exception)` everywhere.

Prefer catching the specific exception that your application can actually handle.

---

# 9. `finally`

The `finally` block is used for cleanup code.

It executes after `try`/`catch` processing when normal exception handling flow is possible.

```csharp
try
{
    Console.WriteLine("Trying...");
}
catch (Exception)
{
    Console.WriteLine("Exception occurred.");
}
finally
{
    Console.WriteLine("Finally executed.");
}
```

Output:

```text
Trying...
Finally executed.
```

If an exception occurs:

```csharp
try
{
    int a = 10;
    int b = 0;

    int result = a / b;
}
catch (DivideByZeroException)
{
    Console.WriteLine("Cannot divide by zero.");
}
finally
{
    Console.WriteLine("Cleanup completed.");
}
```

Output:

```text
Cannot divide by zero.
Cleanup completed.
```

---

# 10. Why Use `finally`?

`finally` is commonly associated with cleanup operations.

For example:

```text
Open file
   ↓
Read file
   ↓
Exception?
   ↓
Close/cleanup resource
```

However, modern C# usually uses `using` for disposable resources.

---

# 11. `throw`

The `throw` keyword is used to manually throw an exception.

```csharp
throw new Exception("Something went wrong.");
```

Example:

```csharp
static void CheckAge(int age)
{
    if (age < 0)
    {
        throw new ArgumentException("Age cannot be negative.");
    }

    Console.WriteLine($"Age: {age}");
}
```

Calling:

```csharp
CheckAge(-5);
```

throws:

```text
ArgumentException
```

---

# 12. `throw new`

You can create and throw a specific exception.

```csharp
throw new ArgumentException("Invalid argument.");
```

Another example:

```csharp
throw new InvalidOperationException("Order cannot be cancelled.");
```

Choosing the correct exception type makes your code easier to understand.

---

# 13. `throw;` vs `throw ex;`

This is important in professional C#.

### Recommended

```csharp
catch (Exception ex)
{
    Console.WriteLine(ex.Message);

    throw;
}
```

`throw;` rethrows the original exception while preserving its original stack trace.

### Avoid

```csharp
catch (Exception ex)
{
    throw ex;
}
```

`throw ex;` can reset the stack-trace information and make debugging harder.

### Remember

```text
throw;       → preserve original exception information
throw ex;    → avoid when rethrowing
```

---

# 14. Exception Flow

The basic flow is:

```text
             ┌─────────────┐
             │    try      │
             └──────┬──────┘
                    │
             Exception?
              /          \
            No            Yes
            │              │
            │         Matching catch
            │              │
            └──────┬───────┘
                   ↓
             ┌─────────────┐
             │   finally   │
             └─────────────┘
```

---

# 15. Example: User Input

A common example is converting user input.

```csharp
try
{
    Console.Write("Enter your age: ");

    int age = int.Parse(Console.ReadLine());

    Console.WriteLine($"Your age is {age}");
}
catch (FormatException)
{
    Console.WriteLine("Please enter a valid number.");
}
```

If the user enters:

```text
22
```

Output:

```text
Your age is 22
```

If the user enters:

```text
abc
```

Output:

```text
Please enter a valid number.
```

---

# 16. `Parse()` vs `TryParse()`

For normal user input, `TryParse()` is often better than relying on exceptions.

### Using `Parse()`

```csharp
int age = int.Parse("22");
```

Invalid input can throw an exception.

### Using `TryParse()`

```csharp
Console.Write("Enter your age: ");

bool success = int.TryParse(
    Console.ReadLine(),
    out int age
);

if (success)
{
    Console.WriteLine($"Age: {age}");
}
else
{
    Console.WriteLine("Invalid age.");
}
```

`TryParse()` returns:

```text
true  → conversion successful
false → conversion failed
```

### Important idea

Use exceptions for **exceptional situations**, not as the normal way to validate expected user input.

---

# 17. `FormatException`

Occurs when a string cannot be converted to the expected format.

```csharp
try
{
    int number = int.Parse("abc");
}
catch (FormatException)
{
    Console.WriteLine("Invalid number format.");
}
```

---

# 18. `OverflowException`

Occurs when a value is outside the supported numeric range.

```csharp
try
{
    int number = int.Parse("999999999999999999999");
}
catch (OverflowException)
{
    Console.WriteLine("Number is too large.");
}
```

---

# 19. `IndexOutOfRangeException`

Occurs when accessing an invalid array index.

```csharp
int[] numbers = { 10, 20, 30 };

try
{
    Console.WriteLine(numbers[5]);
}
catch (IndexOutOfRangeException)
{
    Console.WriteLine("Invalid array index.");
}
```

Valid indexes are:

```text
0 → 10
1 → 20
2 → 30
```

---

# 20. `NullReferenceException`

Occurs when code attempts to access a member through a `null` reference.

```csharp
string? name = null;

try
{
    Console.WriteLine(name.Length);
}
catch (NullReferenceException)
{
    Console.WriteLine("Name is null.");
}
```

A better approach is to check for `null` before accessing the object.

```csharp
if (name != null)
{
    Console.WriteLine(name.Length);
}
```

Or use null-conditional access:

```csharp
Console.WriteLine(name?.Length);
```

Modern C# also provides **nullable reference types** to help identify many possible null problems during development.

---

# 21. `ArgumentException`

Used when a method receives an invalid argument.

```csharp
static void SetPrice(decimal price)
{
    if (price < 0)
    {
        throw new ArgumentException("Price cannot be negative.");
    }

    Console.WriteLine($"Price: {price}");
}
```

Usage:

```csharp
SetPrice(-100);
```

---

# 22. `ArgumentNullException`

Used when an argument must not be `null`.

```csharp
static void RegisterUser(string name)
{
    if (name == null)
    {
        throw new ArgumentNullException(nameof(name));
    }

    Console.WriteLine($"User: {name}");
}
```

`nameof(name)` gives the parameter name safely.

---

# 23. `InvalidOperationException`

Used when an operation is not valid for the object's current state.

Example:

```csharp
class Order
{
    public string Status { get; private set; } = "Completed";

    public void Cancel()
    {
        if (Status == "Completed")
        {
            throw new InvalidOperationException(
                "Completed orders cannot be cancelled."
            );
        }

        Status = "Cancelled";
    }
}
```

The problem is not necessarily the argument.

The operation itself is invalid for the current state.

---

# 24. Dictionary and `KeyNotFoundException`

```csharp
Dictionary<int, string> users = new()
{
    { 1, "Sandip" },
    { 2, "Ram" }
};

try
{
    Console.WriteLine(users[10]);
}
catch (KeyNotFoundException)
{
    Console.WriteLine("User not found.");
}
```

A better approach when appropriate:

```csharp
if (users.TryGetValue(10, out string? user))
{
    Console.WriteLine(user);
}
else
{
    Console.WriteLine("User not found.");
}
```

---

# 25. Custom Exceptions

Sometimes built-in exceptions do not clearly represent a domain-specific problem.

You can create your own exception.

Example:

```csharp
public class InsufficientBalanceException : Exception
{
    public InsufficientBalanceException(string message)
        : base(message)
    {
    }
}
```

Now it can be used like:

```csharp
throw new InsufficientBalanceException(
    "Insufficient account balance."
);
```

---

# 26. Complete Custom Exception Example

```csharp
public class InsufficientBalanceException : Exception
{
    public InsufficientBalanceException(string message)
        : base(message)
    {
    }
}

public class BankAccount
{
    public decimal Balance { get; private set; }

    public BankAccount(decimal balance)
    {
        Balance = balance;
    }

    public void Withdraw(decimal amount)
    {
        if (amount <= 0)
        {
            throw new ArgumentException(
                "Amount must be greater than zero."
            );
        }

        if (amount > Balance)
        {
            throw new InsufficientBalanceException(
                "Insufficient balance."
            );
        }

        Balance -= amount;
    }
}
```

Usage:

```csharp
try
{
    BankAccount account = new BankAccount(1000);

    account.Withdraw(1500);
}
catch (InsufficientBalanceException ex)
{
    Console.WriteLine(ex.Message);
}
```

Output:

```text
Insufficient balance.
```

---

# 27. Inner Exception

An exception can contain another exception called an **inner exception**.

Example:

```csharp
try
{
    try
    {
        int number = int.Parse("abc");
    }
    catch (FormatException ex)
    {
        throw new Exception(
            "Failed while processing user input.",
            ex
        );
    }
}
catch (Exception ex)
{
    Console.WriteLine(ex.Message);
    Console.WriteLine(ex.InnerException?.Message);
}
```

The outer exception provides additional context while `InnerException` preserves the original cause.

---

# 28. Exception Filters

C# allows conditions on `catch` using `when`.

```csharp
try
{
    // code
}
catch (Exception ex) when (ex.Message.Contains("database"))
{
    Console.WriteLine("Database-related error.");
}
```

General syntax:

```csharp
catch (Exception ex) when (condition)
{
    // handling
}
```

Exception filters are useful when the same exception type needs different handling based on additional information.

---

# 29. `using` and Resource Cleanup

Many C# resources implement:

```csharp
IDisposable
```

Examples:

* Files
* Database connections
* Streams
* Network resources

Instead of manually cleaning them up with `finally`, use `using`.

```csharp
using StreamReader reader = new StreamReader("data.txt");

string content = reader.ReadToEnd();

Console.WriteLine(content);
```

The resource is disposed automatically when it leaves scope.

This is especially important when working with files and databases.

---

# 30. `using` with a Block

Another form is:

```csharp
using (StreamReader reader = new StreamReader("data.txt"))
{
    string content = reader.ReadToEnd();

    Console.WriteLine(content);
}
```

Once the block ends, the reader is disposed.

---

# 31. Exception Handling with File Operations

```csharp
try
{
    string content = File.ReadAllText("data.txt");

    Console.WriteLine(content);
}
catch (FileNotFoundException)
{
    Console.WriteLine("File was not found.");
}
catch (IOException ex)
{
    Console.WriteLine($"File error: {ex.Message}");
}
```

---

# 32. Real-World Hotel Management Example

Suppose a hotel application receives an order.

An order ID must be valid before processing.

```csharp
public class OrderService
{
    public void ConfirmOrder(int orderId)
    {
        if (orderId <= 0)
        {
            throw new ArgumentException(
                "Order ID must be greater than zero."
            );
        }

        Console.WriteLine(
            $"Order {orderId} confirmed."
        );
    }
}
```

Usage:

```csharp
try
{
    OrderService service = new OrderService();

    service.ConfirmOrder(101);
}
catch (ArgumentException ex)
{
    Console.WriteLine(ex.Message);
}
```

---

# 33. Hotel Order Custom Exception

We can create a domain-specific exception.

```csharp
public class OrderNotFoundException : Exception
{
    public OrderNotFoundException(string message)
        : base(message)
    {
    }
}
```

Then:

```csharp
public class OrderService
{
    public void GetOrder(int orderId)
    {
        bool orderExists = false;

        if (!orderExists)
        {
            throw new OrderNotFoundException(
                $"Order {orderId} was not found."
            );
        }
    }
}
```

Usage:

```csharp
try
{
    OrderService service = new OrderService();

    service.GetOrder(101);
}
catch (OrderNotFoundException ex)
{
    Console.WriteLine(ex.Message);
}
```

Output:

```text
Order 101 was not found.
```

---

# 34. Exception Handling in Layers

In a real application such as an ASP.NET Core API, exception handling can be organized into layers.

```text
Controller
    ↓
Service
    ↓
Repository
    ↓
Database
```

For example:

```text
Database error
      ↓
Repository
      ↓
Service
      ↓
Exception handling / logging
      ↓
API response
```

You generally should not expose internal exception details directly to users.

Bad:

```json
{
    "error": "MongoServerError: connection string..."
}
```

Better:

```json
{
    "success": false,
    "message": "Something went wrong. Please try again."
}
```

Detailed exception information should be logged securely on the server.

---

# 35. Logging Exceptions

In production applications, exceptions should normally be logged.

Conceptually:

```csharp
try
{
    ProcessOrder();
}
catch (Exception ex)
{
    logger.LogError(ex, "Error while processing order.");

    throw;
}
```

Logging helps developers investigate:

* What happened?
* When did it happen?
* Which operation failed?
* What was the exception?
* Where did it happen?

Do not log passwords, tokens, payment secrets, or other sensitive information.

---

# 36. Don't Swallow Exceptions

Avoid this:

```csharp
try
{
    ProcessOrder();
}
catch (Exception)
{
}
```

This is called **swallowing an exception**.

The error disappears and debugging becomes difficult.

If you can handle it:

```csharp
catch (Exception ex)
{
    Console.WriteLine(ex.Message);
}
```

If you cannot handle it at that level, consider logging and rethrowing:

```csharp
catch (Exception ex)
{
    logger.LogError(ex, "Order processing failed.");

    throw;
}
```

---

# 37. Don't Use Exceptions for Normal Control Flow

Avoid using exceptions for normal expected operations.

Not recommended:

```csharp
try
{
    int number = int.Parse(userInput);
}
catch (FormatException)
{
    // normal validation
}
```

For simple user input, prefer:

```csharp
if (int.TryParse(userInput, out int number))
{
    Console.WriteLine(number);
}
else
{
    Console.WriteLine("Invalid number.");
}
```

Exceptions are better for unexpected or exceptional conditions.

---

# 38. Exception Handling Best Practices

### 1. Catch specific exceptions

Prefer:

```csharp
catch (FileNotFoundException)
{
}
```

instead of:

```csharp
catch (Exception)
{
}
```

when you know the specific failure you can handle.

---

### 2. Don't swallow exceptions

Avoid:

```csharp
catch
{
}
```

---

### 3. Preserve the stack trace

Use:

```csharp
throw;
```

instead of:

```csharp
throw ex;
```

when rethrowing the same exception.

---

### 4. Validate expected input

Use:

```csharp
int.TryParse()
```

for normal numeric input.

---

### 5. Use meaningful exception types

For example:

```csharp
ArgumentException
```

for invalid arguments.

```csharp
InvalidOperationException
```

for invalid object state.

```csharp
FileNotFoundException
```

for missing files.

---

### 6. Use custom exceptions carefully

Create custom exceptions when they provide meaningful domain information.

Don't create custom exceptions for every small validation.

---

### 7. Use `using` for disposable resources

```csharp
using StreamReader reader = new StreamReader("data.txt");
```

---

### 8. Log exceptions properly

Production applications should record useful diagnostic information.

---

### 9. Don't expose internal errors

Users should receive safe, understandable messages.

---

### 10. Don't overuse `try-catch`

Not every method needs its own `try-catch`.

Handle an exception at the level where you can actually recover from it or convert it into an appropriate application response.

---

# 39. Complete Example

```csharp
using System;

class Program
{
    static void Main()
    {
        try
        {
            Console.Write("Enter first number: ");
            int first = int.Parse(Console.ReadLine());

            Console.Write("Enter second number: ");
            int second = int.Parse(Console.ReadLine());

            int result = first / second;

            Console.WriteLine($"Result: {result}");
        }
        catch (FormatException)
        {
            Console.WriteLine(
                "Please enter valid numbers."
            );
        }
        catch (DivideByZeroException)
        {
            Console.WriteLine(
                "Second number cannot be zero."
            );
        }
        catch (Exception ex)
        {
            Console.WriteLine(
                $"Unexpected error: {ex.Message}"
            );
        }
        finally
        {
            Console.WriteLine(
                "Program execution completed."
            );
        }
    }
}
```

---

# 40. Exception Handling Mental Model

Think of exception handling like this:

```text
try
 ↓
Run risky code
 ↓
Did an exception occur?
 ├── No → Continue
 │
 └── Yes
       ↓
   Find matching catch
       ↓
   Handle/log/rethrow
       ↓
     finally
       ↓
    Continue/exit
```

---

# 41. Quick Syntax Revision

### Basic

```csharp
try
{
    // risky code
}
catch (Exception ex)
{
    // handle exception
}
```

### With finally

```csharp
try
{
    // risky code
}
catch (Exception ex)
{
    // handle
}
finally
{
    // cleanup
}
```

### Throw exception

```csharp
throw new ArgumentException("Invalid value.");
```

### Rethrow

```csharp
catch (Exception)
{
    throw;
}
```

### Custom exception

```csharp
public class MyException : Exception
{
    public MyException(string message)
        : base(message)
    {
    }
}
```

---

# 42. Summary

C# exception handling provides a structured way to deal with runtime problems.

The most important keywords are:

```text
try
catch
finally
throw
```

The basic pattern is:

```csharp
try
{
    // risky code
}
catch (SpecificException ex)
{
    // handle error
}
finally
{
    // cleanup
}
```

For manually creating an error:

```csharp
throw new Exception("Something went wrong.");
```

For rethrowing an existing exception:

```csharp
throw;
```

For normal input validation:

```csharp
int.TryParse(...)
```

For resources:

```csharp
using
```

### Core idea

> **Exceptions should help your application handle unexpected problems safely, not replace normal validation and program logic.**
