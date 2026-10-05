# C# Interfaces

An **interface** is a contract that defines what a class must do, without necessarily defining how it does it.

Interfaces are one of the most important concepts in C# because they are heavily used in:

* Object-Oriented Programming
* Polymorphism
* ASP.NET Core
* Dependency Injection
* Web APIs
* Entity Framework Core
* Testing
* Large-scale applications

---

# 1. What is an Interface?

An interface defines a set of members that implementing classes must provide.

Think of an interface as a **contract**.

```text
Interface
    ↓
Defines what must be done
    ↓
Class
    ↓
Defines how it is done
```

For example, a payment system may require every payment method to have a `Pay()` method.

```csharp
interface IPayment
{
    void Pay();
}
```

A class can implement the interface:

```csharp
class CreditCardPayment : IPayment
{
    public void Pay()
    {
        Console.WriteLine("Payment using credit card.");
    }
}
```

Another class can implement the same interface differently:

```csharp
class EsewaPayment : IPayment
{
    public void Pay()
    {
        Console.WriteLine("Payment using eSewa.");
    }
}
```

---

# 2. Interface Naming Convention

C# interfaces commonly start with the letter `I`.

Examples:

```text
IPayment
IUserService
IProductRepository
IEmailService
ILogger
IEnumerable
IDisposable
```

For example:

```csharp
interface IPayment
{
    void Pay();
}
```

The `I` prefix is a widely used C# naming convention.

---

# 3. Creating an Interface

Basic syntax:

```csharp
interface IAnimal
{
    void MakeSound();
}
```

The interface defines:

```text
MakeSound()
```

but does not define its implementation.

A class implements the interface:

```csharp
class Dog : IAnimal
{
    public void MakeSound()
    {
        Console.WriteLine("Dog says Woof.");
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
Dog says Woof.
```

---

# 4. Interface as a Contract

Suppose we have:

```csharp
interface IPayment
{
    void Pay(double amount);
}
```

Any class implementing `IPayment` must provide:

```csharp
Pay(double amount)
```

For example:

```csharp
class CreditCardPayment : IPayment
{
    public void Pay(double amount)
    {
        Console.WriteLine($"Paid {amount} using credit card.");
    }
}
```

Another implementation:

```csharp
class CashPayment : IPayment
{
    public void Pay(double amount)
    {
        Console.WriteLine($"Paid {amount} using cash.");
    }
}
```

The interface guarantees that both classes have the same required operation:

```text
IPayment
   │
   ├── Pay()
   │
   ├── CreditCardPayment
   │
   └── CashPayment
```

---

# 5. Implementing an Interface

A class implements an interface using `:`.

```csharp
interface IStudent
{
    void Study();
}
```

Implementation:

```csharp
class Student : IStudent
{
    public void Study()
    {
        Console.WriteLine("Student is studying.");
    }
}
```

Usage:

```csharp
Student student = new Student();

student.Study();
```

---

# 6. Interface Members

An interface can define members such as:

* Methods
* Properties
* Events
* Indexers

Example:

```csharp
interface IUser
{
    string Name { get; set; }

    void Login();
}
```

Implementation:

```csharp
class User : IUser
{
    public string Name { get; set; }

    public void Login()
    {
        Console.WriteLine($"{Name} logged in.");
    }
}
```

Usage:

```csharp
User user = new User();

user.Name = "Sandip";
user.Login();
```

Output:

```text
Sandip logged in.
```

---

# 7. Interface Properties

Interfaces can define properties.

```csharp
interface IProduct
{
    string Name { get; set; }
    double Price { get; set; }
}
```

Implementation:

```csharp
class Product : IProduct
{
    public string Name { get; set; }
    public double Price { get; set; }
}
```

Usage:

```csharp
Product product = new Product();

product.Name = "Laptop";
product.Price = 85000;

Console.WriteLine(product.Name);
Console.WriteLine(product.Price);
```

---

# 8. Interface Methods

Interfaces can define methods.

```csharp
interface ICalculator
{
    int Add(int a, int b);
    int Subtract(int a, int b);
}
```

Implementation:

```csharp
class Calculator : ICalculator
{
    public int Add(int a, int b)
    {
        return a + b;
    }

    public int Subtract(int a, int b)
    {
        return a - b;
    }
}
```

Usage:

```csharp
Calculator calculator = new Calculator();

Console.WriteLine(calculator.Add(10, 5));
Console.WriteLine(calculator.Subtract(10, 5));
```

Output:

```text
15
5
```

---

# 9. Interface and Polymorphism

One of the most important uses of interfaces is **polymorphism**.

Consider:

```csharp
interface IPayment
{
    void Pay(double amount);
}
```

Implementations:

```csharp
class CreditCardPayment : IPayment
{
    public void Pay(double amount)
    {
        Console.WriteLine($"Paid {amount} using Credit Card.");
    }
}
```

```csharp
class CashPayment : IPayment
{
    public void Pay(double amount)
    {
        Console.WriteLine($"Paid {amount} using Cash.");
    }
}
```

Now:

```csharp
IPayment payment;

payment = new CreditCardPayment();
payment.Pay(1000);

payment = new CashPayment();
payment.Pay(1000);
```

Output:

```text
Paid 1000 using Credit Card.
Paid 1000 using Cash.
```

The variable type is:

```csharp
IPayment
```

but the actual object can be different.

This is **polymorphism**.

---

# 10. Why Interfaces Are Useful for Polymorphism

Without an interface, you might write code tightly connected to a specific class:

```csharp
CreditCardPayment payment = new CreditCardPayment();

payment.Pay(1000);
```

With an interface:

```csharp
IPayment payment = new CreditCardPayment();

payment.Pay(1000);
```

Now the code can work with any `IPayment` implementation.

For example:

```csharp
IPayment payment;

payment = new CreditCardPayment();
payment.Pay(1000);

payment = new CashPayment();
payment.Pay(1000);

payment = new EsewaPayment();
payment.Pay(1000);
```

This makes applications easier to extend.

---

# 11. Multiple Interfaces

A C# class can implement multiple interfaces.

For example:

```csharp
interface IPrintable
{
    void Print();
}
```

```csharp
interface IScannable
{
    void Scan();
}
```

A class can implement both:

```csharp
class Printer : IPrintable, IScannable
{
    public void Print()
    {
        Console.WriteLine("Printing...");
    }

    public void Scan()
    {
        Console.WriteLine("Scanning...");
    }
}
```

Usage:

```csharp
Printer printer = new Printer();

printer.Print();
printer.Scan();
```

Output:

```text
Printing...
Scanning...
```

Unlike classes, C# allows a class to implement multiple interfaces.

---

# 12. Interface Inheritance

Interfaces can inherit from other interfaces.

Example:

```csharp
interface IAnimal
{
    void Eat();
}
```

Another interface:

```csharp
interface IDog : IAnimal
{
    void Bark();
}
```

A class implementing `IDog` must implement both:

```csharp
class Dog : IDog
{
    public void Eat()
    {
        Console.WriteLine("Dog is eating.");
    }

    public void Bark()
    {
        Console.WriteLine("Dog is barking.");
    }
}
```

Usage:

```csharp
Dog dog = new Dog();

dog.Eat();
dog.Bark();
```

---

# 13. Explicit Interface Implementation

Sometimes two interfaces contain methods with the same name.

Example:

```csharp
interface IPrinter
{
    void Start();
}
```

```csharp
interface IScanner
{
    void Start();
}
```

A class can implement both explicitly:

```csharp
class Machine : IPrinter, IScanner
{
    void IPrinter.Start()
    {
        Console.WriteLine("Printer started.");
    }

    void IScanner.Start()
    {
        Console.WriteLine("Scanner started.");
    }
}
```

Usage:

```csharp
Machine machine = new Machine();

IPrinter printer = machine;
IScanner scanner = machine;

printer.Start();
scanner.Start();
```

Output:

```text
Printer started.
Scanner started.
```

Explicit implementation is useful when different interfaces contain members with the same names.

---

# 14. Default Interface Methods

Modern C# allows interfaces to contain default implementations.

Example:

```csharp
interface ILogger
{
    void Log(string message)
    {
        Console.WriteLine(message);
    }
}
```

A class can use the default implementation:

```csharp
class Application : ILogger
{
}
```

To access the default implementation:

```csharp
Application app = new Application();

ILogger logger = app;

logger.Log("Application started.");
```

Output:

```text
Application started.
```

Default interface implementations are useful when adding new behavior to existing interfaces without immediately requiring every implementation to provide the method.

---

# 15. Interface vs Abstract Class

Interfaces and abstract classes can look similar, but they serve different purposes.

| Interface                                 | Abstract Class                        |
| ----------------------------------------- | ------------------------------------- |
| Defines a contract                        | Defines a base class                  |
| Uses `interface`                          | Uses `abstract class`                 |
| A class can implement multiple interfaces | A class can inherit only one class    |
| Focuses on capabilities                   | Can contain shared state and behavior |
| Commonly used for loose coupling          | Commonly used for related classes     |
| Great for dependency injection            | Useful for shared base functionality  |

Example interface:

```csharp
interface IPayment
{
    void Pay();
}
```

Example abstract class:

```csharp
abstract class Animal
{
    public void Eat()
    {
        Console.WriteLine("Animal is eating.");
    }

    public abstract void MakeSound();
}
```

A simple way to remember:

```text
Interface
    ↓
"What can this class do?"

Abstract class
    ↓
"What is this class?"
```

For example:

```text
Dog
 ├── is an Animal
 ├── can be a Pet
 └── can be ITrainable
```

---

# 16. Real-World Example — Hotel Management System

Suppose your hotel application supports different payment methods.

Create an interface:

```csharp
public interface IPaymentService
{
    void Pay(double amount);
}
```

Cash:

```csharp
public class CashPaymentService : IPaymentService
{
    public void Pay(double amount)
    {
        Console.WriteLine($"Cash payment: {amount}");
    }
}
```

Card:

```csharp
public class CardPaymentService : IPaymentService
{
    public void Pay(double amount)
    {
        Console.WriteLine($"Card payment: {amount}");
    }
}
```

Digital wallet:

```csharp
public class EsewaPaymentService : IPaymentService
{
    public void Pay(double amount)
    {
        Console.WriteLine($"eSewa payment: {amount}");
    }
}
```

Now your application can use:

```csharp
IPaymentService paymentService;

paymentService = new CashPaymentService();
paymentService.Pay(2500);

paymentService = new CardPaymentService();
paymentService.Pay(3000);

paymentService = new EsewaPaymentService();
paymentService.Pay(3500);
```

The rest of your application does not need to know the internal payment implementation.

---

# 17. Interface for Notification Services

Another practical example is notifications.

```csharp
interface INotificationService
{
    void Send(string message);
}
```

Email:

```csharp
class EmailNotification : INotificationService
{
    public void Send(string message)
    {
        Console.WriteLine($"Email: {message}");
    }
}
```

SMS:

```csharp
class SmsNotification : INotificationService
{
    public void Send(string message)
    {
        Console.WriteLine($"SMS: {message}");
    }
}
```

Push notification:

```csharp
class PushNotification : INotificationService
{
    public void Send(string message)
    {
        Console.WriteLine($"Push Notification: {message}");
    }
}
```

Now:

```csharp
INotificationService notification;

notification = new EmailNotification();
notification.Send("Order confirmed.");

notification = new SmsNotification();
notification.Send("Order confirmed.");
```

This is interface-based polymorphism.

---

# 18. Interface and Dependency Injection

Interfaces are heavily used with **Dependency Injection**.

Suppose we have:

```csharp
public interface IUserService
{
    void CreateUser();
}
```

Implementation:

```csharp
public class UserService : IUserService
{
    public void CreateUser()
    {
        Console.WriteLine("User created.");
    }
}
```

Another class can depend on the interface:

```csharp
public class UserController
{
    private readonly IUserService _userService;

    public UserController(IUserService userService)
    {
        _userService = userService;
    }

    public void Create()
    {
        _userService.CreateUser();
    }
}
```

Notice that `UserController` depends on:

```csharp
IUserService
```

instead of:

```csharp
UserService
```

This creates **loose coupling**.

---

# 19. Loose Coupling

Loose coupling means classes do not depend heavily on specific implementations.

Bad example:

```csharp
class OrderService
{
    private EmailNotification _notification =
        new EmailNotification();
}
```

`OrderService` is tightly connected to `EmailNotification`.

Better:

```csharp
class OrderService
{
    private readonly INotificationService _notification;

    public OrderService(INotificationService notification)
    {
        _notification = notification;
    }
}
```

Now `OrderService` can work with:

```text
EmailNotification
SMSNotification
PushNotification
```

as long as they implement:

```text
INotificationService
```

---

# 20. Interfaces and Testing

Interfaces also make applications easier to test.

Suppose:

```csharp
interface IPaymentService
{
    void Pay(double amount);
}
```

Production implementation:

```csharp
class EsewaPaymentService : IPaymentService
{
    public void Pay(double amount)
    {
        Console.WriteLine("Real eSewa payment.");
    }
}
```

During testing, we can create another implementation:

```csharp
class FakePaymentService : IPaymentService
{
    public void Pay(double amount)
    {
        Console.WriteLine("Fake payment for testing.");
    }
}
```

Now the application can use the fake implementation during tests.

This is one reason interfaces are important in professional applications.

---

# 21. Interface Example with a List

Interfaces can also be used with collections.

```csharp
interface IAnimal
{
    void MakeSound();
}
```

Implementations:

```csharp
class Dog : IAnimal
{
    public void MakeSound()
    {
        Console.WriteLine("Woof");
    }
}
```

```csharp
class Cat : IAnimal
{
    public void MakeSound()
    {
        Console.WriteLine("Meow");
    }
}
```

Now:

```csharp
List<IAnimal> animals = new List<IAnimal>
{
    new Dog(),
    new Cat()
};

foreach (IAnimal animal in animals)
{
    animal.MakeSound();
}
```

Output:

```text
Woof
Meow
```

This is a very useful combination of:

```text
Interface
   +
Polymorphism
   +
Collections
```

---

# 22. Common Built-in C# Interfaces

C# and .NET provide many interfaces.

Some important examples:

| Interface                  | Purpose                         |
| -------------------------- | ------------------------------- |
| `IEnumerable<T>`           | Enables iteration               |
| `ICollection<T>`           | Represents a collection         |
| `IList<T>`                 | Represents a list               |
| `IDictionary<TKey,TValue>` | Represents key/value data       |
| `IComparable<T>`           | Defines comparison              |
| `IEquatable<T>`            | Defines equality                |
| `IDisposable`              | Provides cleanup/disposal       |
| `IAsyncEnumerable<T>`      | Supports asynchronous iteration |

You will encounter these frequently when learning .NET.

---

# 23. `IDisposable`

One important built-in interface is:

```csharp
IDisposable
```

It is used for objects that need explicit cleanup.

Example:

```csharp
class Resource : IDisposable
{
    public void Dispose()
    {
        Console.WriteLine("Resource released.");
    }
}
```

Usage:

```csharp
using (Resource resource = new Resource())
{
    Console.WriteLine("Using resource.");
}
```

When the `using` block ends, `Dispose()` is called.

This becomes especially important with:

* Files
* Streams
* Database connections
* Network resources

---

# 24. Interface Best Practices

### 1. Use meaningful names

Good:

```text
IUserService
IPaymentService
IEmailService
```

Avoid unclear names:

```text
ITest
IThing
IData
```

unless the purpose is genuinely generic.

---

### 2. Keep interfaces focused

Instead of one huge interface:

```csharp
interface IUser
{
    void Login();
    void Logout();
    void SendEmail();
    void GenerateReport();
    void ProcessPayment();
}
```

prefer smaller interfaces when appropriate:

```csharp
interface IAuthenticatable
{
    void Login();
    void Logout();
}
```

```csharp
interface IReportGenerator
{
    void GenerateReport();
}
```

```csharp
interface IPaymentProcessor
{
    void ProcessPayment();
}
```

This follows the **Interface Segregation Principle** from SOLID.

---

# 25. Interface vs Class

A simple comparison:

```text
Class
  ↓
Provides implementation

Interface
  ↓
Defines a contract
```

Example:

```csharp
interface IVehicle
{
    void Start();
}
```

```csharp
class Car : IVehicle
{
    public void Start()
    {
        Console.WriteLine("Car started.");
    }
}
```

The interface says:

```text
Every IVehicle must be able to Start().
```

The class decides **how** it starts.

---

# 26. Complete Example

Here is a complete example combining several concepts:

```csharp
interface IEmployee
{
    string Name { get; set; }

    void Work();
}
```

Developer:

```csharp
class Developer : IEmployee
{
    public string Name { get; set; }

    public Developer(string name)
    {
        Name = name;
    }

    public void Work()
    {
        Console.WriteLine($"{Name} is writing code.");
    }
}
```

Designer:

```csharp
class Designer : IEmployee
{
    public string Name { get; set; }

    public Designer(string name)
    {
        Name = name;
    }

    public void Work()
    {
        Console.WriteLine($"{Name} is designing UI.");
    }
}
```

Now use polymorphism:

```csharp
List<IEmployee> employees = new List<IEmployee>
{
    new Developer("Sandip"),
    new Designer("Ram")
};

foreach (IEmployee employee in employees)
{
    employee.Work();
}
```

Output:

```text
Sandip is writing code.
Ram is designing UI.
```

The program works with `IEmployee`, not with a specific implementation.

---

# 27. Key Advantages of Interfaces

Interfaces provide:

### Loose Coupling

Classes depend on contracts rather than concrete implementations.

### Polymorphism

Different classes can be treated through the same interface.

### Extensibility

New implementations can be added without changing existing code extensively.

### Testability

Fake or mock implementations can be used during testing.

### Multiple Contracts

A class can implement multiple interfaces.

### Maintainability

Large applications become easier to organize and change.

---

# 28. Interface Mental Model

Remember:

```text
             INTERFACE
                 │
          Defines a contract
                 │
       ┌─────────┼─────────┐
       ↓         ↓         ↓
     Class A   Class B   Class C
       │         │         │
       ↓         ↓         ↓
   Different implementations
                 │
                 ↓
            Polymorphism
```

For example:

```text
              IPaymentService
                     │
        ┌────────────┼────────────┐
        ↓            ↓            ↓
      Cash          Card         eSewa
        │            │            │
        └────────────┼────────────┘
                     ↓
              Pay(amount)
```

---

# 29. Summary

An interface is a **contract** that describes what a class should be able to do.

The basic pattern is:

```csharp
interface IPayment
{
    void Pay(double amount);
}
```

Then different classes implement it:

```csharp
class CashPayment : IPayment
{
    public void Pay(double amount)
    {
        Console.WriteLine("Cash payment.");
    }
}
```

```csharp
class CardPayment : IPayment
{
    public void Pay(double amount)
    {
        Console.WriteLine("Card payment.");
    }
}
```

And polymorphism allows:

```csharp
IPayment payment = new CashPayment();

payment.Pay(1000);

payment = new CardPayment();

payment.Pay(1000);
```

The core idea is:

```text
Interface
    ↓
Contract
    ↓
Multiple Implementations
    ↓
Polymorphism
    ↓
Loose Coupling
    ↓
Extensible Applications
```

Interfaces are fundamental to modern C# development and become especially important when working with **ASP.NET Core, Web APIs, Dependency Injection, Entity Framework Core, and automated testing**.
