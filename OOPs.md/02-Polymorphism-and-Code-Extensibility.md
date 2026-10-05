# C# Polymorphism and Code Extensibility

**Polymorphism** is one of the four main pillars of Object-Oriented Programming.

The word polymorphism means:

> **One interface, multiple forms.**

In C#, polymorphism allows the same code to work with different types of objects.

This is extremely useful for building applications that are:

* Flexible
* Maintainable
* Extensible
* Testable
* Loosely coupled

---

# 1. What is Polymorphism?

Polymorphism means that one operation can behave differently depending on the object.

For example:

```text
Animal
  │
  ├── Dog → Woof
  ├── Cat → Meow
  └── Cow → Moo
```

All are animals, but each makes a different sound.

---

# 2. Basic Polymorphism Example

Create a base class:

```csharp
class Animal
{
    public virtual void MakeSound()
    {
        Console.WriteLine("Animal makes a sound.");
    }
}
```

Create a derived class:

```csharp
class Dog : Animal
{
    public override void MakeSound()
    {
        Console.WriteLine("Dog says Woof.");
    }
}
```

Another derived class:

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

The variable type is:

```csharp
Animal
```

but the actual objects are:

```text
Dog
Cat
```

This is runtime polymorphism.

---

# 3. `virtual` and `override`

Two important keywords are:

```text
virtual
override
```

The parent class uses `virtual`:

```csharp
class Animal
{
    public virtual void MakeSound()
    {
        Console.WriteLine("Animal sound");
    }
}
```

The child class uses `override`:

```csharp
class Dog : Animal
{
    public override void MakeSound()
    {
        Console.WriteLine("Woof");
    }
}
```

### Meaning

```text
virtual
   ↓
Allows derived classes to change the behavior

override
   ↓
Provides the new behavior
```

---

# 4. Runtime Polymorphism

Runtime polymorphism happens when C# determines which method to execute at runtime.

Example:

```csharp
Animal animal;

animal = new Dog();
animal.MakeSound();

animal = new Cat();
animal.MakeSound();
```

Output:

```text
Woof
Meow
```

The same variable:

```csharp
animal
```

can refer to different objects.

---

# 5. Compile-Time Polymorphism

Another type of polymorphism is **compile-time polymorphism**.

The most common example is **method overloading**.

Example:

```csharp
class Calculator
{
    public int Add(int a, int b)
    {
        return a + b;
    }

    public double Add(double a, double b)
    {
        return a + b;
    }

    public int Add(int a, int b, int c)
    {
        return a + b + c;
    }
}
```

Usage:

```csharp
Calculator calculator = new Calculator();

Console.WriteLine(calculator.Add(10, 20));

Console.WriteLine(calculator.Add(10.5, 20.5));

Console.WriteLine(calculator.Add(10, 20, 30));
```

The compiler determines which method to call.

This is compile-time polymorphism.

---

# 6. Two Main Types of Polymorphism

| Type         | Common technique   | Decided         |
| ------------ | ------------------ | --------------- |
| Compile-time | Method overloading | At compile time |
| Runtime      | Method overriding  | At runtime      |

Simple mental model:

```text
Polymorphism
     │
     ├── Compile-time
     │      └── Overloading
     │
     └── Runtime
            └── Overriding
```

---

# 7. Polymorphism with Interfaces

Interfaces are one of the most powerful ways to use polymorphism.

Create an interface:

```csharp
interface IPayment
{
    void Pay(double amount);
}
```

Credit card:

```csharp
class CreditCardPayment : IPayment
{
    public void Pay(double amount)
    {
        Console.WriteLine($"Paid {amount} using Credit Card.");
    }
}
```

Cash:

```csharp
class CashPayment : IPayment
{
    public void Pay(double amount)
    {
        Console.WriteLine($"Paid {amount} using Cash.");
    }
}
```

eSewa:

```csharp
class EsewaPayment : IPayment
{
    public void Pay(double amount)
    {
        Console.WriteLine($"Paid {amount} using eSewa.");
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

payment = new EsewaPayment();
payment.Pay(1000);
```

Output:

```text
Paid 1000 using Credit Card.
Paid 1000 using Cash.
Paid 1000 using eSewa.
```

The application only needs to understand:

```text
IPayment
```

It doesn't need to know the implementation details.

---

# 8. Polymorphism with Collections

Polymorphism becomes even more powerful when combined with collections.

```csharp
List<IPayment> payments = new List<IPayment>
{
    new CreditCardPayment(),
    new CashPayment(),
    new EsewaPayment()
};
```

Now:

```csharp
foreach (IPayment payment in payments)
{
    payment.Pay(1000);
}
```

Output:

```text
Paid 1000 using Credit Card.
Paid 1000 using Cash.
Paid 1000 using eSewa.
```

The loop doesn't care which payment implementation it receives.

---

# 9. What is Code Extensibility?

**Code extensibility** means designing software so that new functionality can be added with minimal changes to existing code.

For example, suppose your application initially supports:

```text
Cash
Credit Card
```

Later you want to add:

```text
eSewa
Khalti
Bank Transfer
```

A good design allows you to add these payment methods without rewriting the entire payment system.

---

# 10. Poorly Extensible Code

Imagine:

```csharp
class PaymentService
{
    public void Pay(string type, double amount)
    {
        if (type == "cash")
        {
            Console.WriteLine("Cash payment.");
        }
        else if (type == "card")
        {
            Console.WriteLine("Card payment.");
        }
        else if (type == "esewa")
        {
            Console.WriteLine("eSewa payment.");
        }
    }
}
```

This works.

But what happens when you add:

```text
Khalti
Bank
PayPal
```

You must keep modifying this class.

```text
PaymentService
     ↓
Modify existing code
     ↓
Add another if
     ↓
Modify again
     ↓
Add another if
```

As the application grows, this becomes difficult to maintain.

---

# 11. Extensible Design with Interfaces

Instead, create an interface:

```csharp
interface IPayment
{
    void Pay(double amount);
}
```

Then create implementations.

```csharp
class CashPayment : IPayment
{
    public void Pay(double amount)
    {
        Console.WriteLine($"Cash payment: {amount}");
    }
}
```

```csharp
class CardPayment : IPayment
{
    public void Pay(double amount)
    {
        Console.WriteLine($"Card payment: {amount}");
    }
}
```

```csharp
class EsewaPayment : IPayment
{
    public void Pay(double amount)
    {
        Console.WriteLine($"eSewa payment: {amount}");
    }
}
```

Now the application can work with:

```csharp
IPayment payment
```

instead of a specific payment class.

---

# 12. Adding a New Payment Method

Suppose you now want to add Khalti.

Create:

```csharp
class KhaltiPayment : IPayment
{
    public void Pay(double amount)
    {
        Console.WriteLine($"Khalti payment: {amount}");
    }
}
```

That's it.

The existing classes don't need to be changed.

Now:

```csharp
List<IPayment> payments = new List<IPayment>
{
    new CashPayment(),
    new CardPayment(),
    new EsewaPayment(),
    new KhaltiPayment()
};
```

The same code works.

This is **extensibility**.

---

# 13. Open/Closed Principle

This idea is related to the **Open/Closed Principle (OCP)** from SOLID.

The principle says:

> Software entities should be open for extension but closed for modification.

In simple words:

```text
Add new behavior
       ↓
Create new implementation
       ↓
Avoid changing stable existing code
```

For example:

```text
IPayment
   │
   ├── CashPayment
   ├── CardPayment
   ├── EsewaPayment
   └── KhaltiPayment
```

Adding `KhaltiPayment` doesn't require changing `CashPayment` or `CardPayment`.

---

# 14. Polymorphism and Extensibility Together

These two concepts work together:

```text
Interface
    ↓
Common contract
    ↓
Polymorphism
    ↓
Different implementations
    ↓
Extensibility
    ↓
New implementations can be added
```

Example:

```text
IPayment
   │
   ├── Cash
   ├── Card
   ├── eSewa
   └── Khalti
```

The application uses:

```text
IPayment
```

rather than:

```text
CashPayment
CardPayment
EsewaPayment
KhaltiPayment
```

---

# 15. Real-World Hotel Example

Imagine your Hotel Management System supports payments.

You could define:

```csharp
public interface IPaymentService
{
    void ProcessPayment(double amount);
}
```

Cash:

```csharp
public class CashPaymentService : IPaymentService
{
    public void ProcessPayment(double amount)
    {
        Console.WriteLine($"Cash payment processed: {amount}");
    }
}
```

Card:

```csharp
public class CardPaymentService : IPaymentService
{
    public void ProcessPayment(double amount)
    {
        Console.WriteLine($"Card payment processed: {amount}");
    }
}
```

eSewa:

```csharp
public class EsewaPaymentService : IPaymentService
{
    public void ProcessPayment(double amount)
    {
        Console.WriteLine($"eSewa payment processed: {amount}");
    }
}
```

A checkout service can use the interface:

```csharp
public class CheckoutService
{
    private readonly IPaymentService _paymentService;

    public CheckoutService(IPaymentService paymentService)
    {
        _paymentService = paymentService;
    }

    public void Checkout(double amount)
    {
        _paymentService.ProcessPayment(amount);
    }
}
```

Now:

```csharp
IPaymentService payment = new CashPaymentService();

CheckoutService checkout = new CheckoutService(payment);

checkout.Checkout(2500);
```

Output:

```text
Cash payment processed: 2500
```

Change the implementation:

```csharp
IPaymentService payment = new EsewaPaymentService();

CheckoutService checkout = new CheckoutService(payment);

checkout.Checkout(2500);
```

Output:

```text
eSewa payment processed: 2500
```

`CheckoutService` did not need to change.

---

# 16. Why This Design Is Better

Without polymorphism:

```text
CheckoutService
      │
      ├── Cash
      ├── Card
      ├── eSewa
      ├── Khalti
      └── Bank
```

The checkout service knows every payment type.

With polymorphism:

```text
CheckoutService
      │
      ↓
IPaymentService
      │
      ├── Cash
      ├── Card
      ├── eSewa
      ├── Khalti
      └── Bank
```

The checkout service only knows:

```text
IPaymentService
```

This reduces coupling.

---

# 17. Loose Coupling

**Loose coupling** means that classes have minimal dependency on specific implementations.

Tightly coupled:

```csharp
class OrderService
{
    private EsewaPaymentService _payment =
        new EsewaPaymentService();
}
```

`OrderService` is directly connected to `EsewaPaymentService`.

If you want to use Card, you need to modify the class.

Better:

```csharp
class OrderService
{
    private readonly IPaymentService _payment;

    public OrderService(IPaymentService payment)
    {
        _payment = payment;
    }
}
```

Now `OrderService` can work with:

```text
CashPaymentService
CardPaymentService
EsewaPaymentService
KhaltiPaymentService
```

as long as they implement:

```text
IPaymentService
```

---

# 18. Dependency Injection

The previous example demonstrates the basic idea behind **Dependency Injection (DI)**.

Instead of creating the dependency inside the class:

```csharp
_payment = new EsewaPaymentService();
```

we provide the dependency from outside:

```csharp
OrderService orderService =
    new OrderService(new EsewaPaymentService());
```

The class receives what it needs.

This makes the code:

* More flexible
* Easier to test
* Easier to maintain
* Easier to extend

ASP.NET Core heavily uses Dependency Injection.

---

# 19. Dependency Inversion

Another SOLID principle is the **Dependency Inversion Principle (DIP)**.

The basic idea is:

> High-level classes should depend on abstractions rather than concrete implementations.

Instead of:

```text
OrderService
     ↓
EsewaPaymentService
```

use:

```text
OrderService
     ↓
IPaymentService
     ↑
     ├── EsewaPaymentService
     ├── CardPaymentService
     └── CashPaymentService
```

The high-level service depends on:

```text
IPaymentService
```

not a specific payment provider.

---

# 20. Extensibility with Notifications

Consider a notification system.

Interface:

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

Push:

```csharp
class PushNotification : INotificationService
{
    public void Send(string message)
    {
        Console.WriteLine($"Push: {message}");
    }
}
```

A service can use:

```csharp
class OrderService
{
    private readonly INotificationService _notification;

    public OrderService(INotificationService notification)
    {
        _notification = notification;
    }

    public void OrderCompleted()
    {
        _notification.Send("Your order is completed.");
    }
}
```

Usage:

```csharp
INotificationService notification =
    new EmailNotification();

OrderService orderService =
    new OrderService(notification);

orderService.OrderCompleted();
```

Later you can switch to SMS:

```csharp
INotificationService notification =
    new SmsNotification();
```

without changing `OrderService`.

---

# 21. Polymorphism with Abstract Classes

Interfaces aren't the only way to achieve polymorphism.

Abstract classes can also provide runtime polymorphism.

```csharp
abstract class Employee
{
    public string Name { get; set; }

    public abstract void Work();
}
```

Developer:

```csharp
class Developer : Employee
{
    public override void Work()
    {
        Console.WriteLine("Developer is writing code.");
    }
}
```

Designer:

```csharp
class Designer : Employee
{
    public override void Work()
    {
        Console.WriteLine("Designer is designing UI.");
    }
}
```

Usage:

```csharp
List<Employee> employees = new List<Employee>
{
    new Developer(),
    new Designer()
};

foreach (Employee employee in employees)
{
    employee.Work();
}
```

Output:

```text
Developer is writing code.
Designer is designing UI.
```

---

# 22. Method Overriding

Method overriding allows a child class to provide its own implementation.

Base class:

```csharp
class Employee
{
    public virtual void Work()
    {
        Console.WriteLine("Employee is working.");
    }
}
```

Child:

```csharp
class Developer : Employee
{
    public override void Work()
    {
        Console.WriteLine("Developer is coding.");
    }
}
```

Usage:

```csharp
Employee employee = new Developer();

employee.Work();
```

Output:

```text
Developer is coding.
```

---

# 23. Method Hiding with `new`

C# also allows method hiding.

```csharp
class Animal
{
    public void MakeSound()
    {
        Console.WriteLine("Animal sound.");
    }
}
```

Child:

```csharp
class Dog : Animal
{
    public new void MakeSound()
    {
        Console.WriteLine("Woof.");
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
Woof.
```

However, method hiding is different from overriding.

With:

```csharp
new
```

you are hiding the parent member.

With:

```csharp
override
```

you are participating in runtime polymorphism.

Prefer `virtual` + `override` when you intentionally want polymorphic behavior.

---

# 24. Casting and Polymorphism

Suppose:

```csharp
Animal animal = new Dog();
```

You can access members defined in `Animal`.

To access members specific to `Dog`, you may need a cast.

Example:

```csharp
class Dog : Animal
{
    public void Bark()
    {
        Console.WriteLine("Woof");
    }
}
```

Then:

```csharp
Animal animal = new Dog();

Dog dog = (Dog)animal;

dog.Bark();
```

A safer approach is pattern matching:

```csharp
if (animal is Dog dog)
{
    dog.Bark();
}
```

This avoids an invalid cast if the object is not actually a `Dog`.

---

# 25. Pattern Matching

Modern C# provides useful pattern matching syntax.

Example:

```csharp
Animal animal = new Dog();

if (animal is Dog dog)
{
    dog.Bark();
}
```

You can also use:

```csharp
switch (animal)
{
    case Dog dog:
        dog.Bark();
        break;

    case Cat cat:
        cat.MakeSound();
        break;
}
```

Pattern matching is useful when behavior depends on the runtime type.

However, if you find yourself writing many type checks, it may be a sign that your design could benefit from better polymorphism.

---

# 26. Extensibility Example

Suppose you are building an order system.

Initially:

```text
Order
 └── Email notification
```

Later:

```text
Order
 ├── Email
 ├── SMS
 ├── Push
 └── WhatsApp
```

With an interface:

```csharp
interface INotification
{
    void Send(string message);
}
```

you can add:

```csharp
class WhatsAppNotification : INotification
{
    public void Send(string message)
    {
        Console.WriteLine($"WhatsApp: {message}");
    }
}
```

Existing notification implementations don't need to change.

That's the benefit of extensible design.

---

# 27. Polymorphism and Testing

Suppose:

```csharp
interface IPaymentService
{
    void Pay(double amount);
}
```

Production implementation:

```csharp
class RealPaymentService : IPaymentService
{
    public void Pay(double amount)
    {
        Console.WriteLine("Real payment.");
    }
}
```

Testing implementation:

```csharp
class FakePaymentService : IPaymentService
{
    public void Pay(double amount)
    {
        Console.WriteLine("Fake payment.");
    }
}
```

Your service can receive either:

```csharp
IPaymentService payment
```

This makes testing much easier.

In real applications, frameworks such as mocking libraries can create test doubles for interfaces.

---

# 28. Polymorphism in ASP.NET Core

You will frequently see this pattern in ASP.NET Core:

```csharp
public interface IUserService
{
    User GetUser(int id);
}
```

Implementation:

```csharp
public class UserService : IUserService
{
    public User GetUser(int id)
    {
        // Business logic
        return new User();
    }
}
```

Controller:

```csharp
public class UserController
{
    private readonly IUserService _userService;

    public UserController(IUserService userService)
    {
        _userService = userService;
    }
}
```

The controller depends on:

```text
IUserService
```

rather than:

```text
UserService
```

ASP.NET Core's Dependency Injection container can provide the implementation.

This is one of the most important practical uses of interfaces and polymorphism in .NET.

---

# 29. Polymorphism in Entity Framework Core

You may also encounter abstraction around repositories or services.

For example:

```csharp
public interface IUserRepository
{
    Task<User?> GetByIdAsync(int id);
}
```

Implementation:

```csharp
public class UserRepository : IUserRepository
{
    public Task<User?> GetByIdAsync(int id)
    {
        // Database operation
    }
}
```

Business logic can depend on:

```text
IUserRepository
```

instead of directly depending on the concrete repository.

This helps separate:

```text
Controller
    ↓
Service
    ↓
Repository Interface
    ↓
Repository Implementation
    ↓
Database
```

---

# 30. Benefits of Polymorphism

Polymorphism provides:

### Flexibility

The same code can work with different implementations.

### Extensibility

New implementations can be added easily.

### Loose Coupling

Classes depend on abstractions rather than concrete classes.

### Maintainability

Changes are isolated.

### Testability

Fake implementations can be injected.

### Reusability

Common code can work with many object types.

---

# 31. When Should You Use Polymorphism?

Use polymorphism when:

* Multiple classes share a common behavior.
* Different implementations are possible.
* You expect new implementations later.
* You want to reduce dependencies.
* You want to make code easier to test.
* You want to follow SOLID principles.

Example:

```text
Payment
 ├── Cash
 ├── Card
 ├── eSewa
 └── Khalti
```

This is a good candidate for polymorphism.

---

# 32. When Not to Overuse Polymorphism

Polymorphism is powerful, but don't create interfaces for everything without a reason.

For a very simple class:

```csharp
class Student
{
    public string Name { get; set; }
}
```

you probably don't need:

```csharp
IStudent
Student
StudentImplementation
StudentService
StudentManager
```

Keep the design simple when there is no real benefit.

Good software design balances:

```text
Simplicity
     +
Flexibility
     +
Maintainability
```

---

# 33. Complete Example

Here is a complete extensible payment system:

```csharp
public interface IPaymentService
{
    void Pay(double amount);
}
```

Cash:

```csharp
public class CashPayment : IPaymentService
{
    public void Pay(double amount)
    {
        Console.WriteLine($"Cash payment: {amount}");
    }
}
```

Card:

```csharp
public class CardPayment : IPaymentService
{
    public void Pay(double amount)
    {
        Console.WriteLine($"Card payment: {amount}");
    }
}
```

eSewa:

```csharp
public class EsewaPayment : IPaymentService
{
    public void Pay(double amount)
    {
        Console.WriteLine($"eSewa payment: {amount}");
    }
}
```

Checkout service:

```csharp
public class CheckoutService
{
    private readonly IPaymentService _paymentService;

    public CheckoutService(IPaymentService paymentService)
    {
        _paymentService = paymentService;
    }

    public void Checkout(double amount)
    {
        _paymentService.Pay(amount);
    }
}
```

Usage:

```csharp
IPaymentService payment = new EsewaPayment();

CheckoutService checkout =
    new CheckoutService(payment);

checkout.Checkout(2500);
```

Output:

```text
eSewa payment: 2500
```

Later, add:

```csharp
public class KhaltiPayment : IPaymentService
{
    public void Pay(double amount)
    {
        Console.WriteLine($"Khalti payment: {amount}");
    }
}
```

No changes are required to `CheckoutService`.

This is the core idea of extensible design.

---

# 34. Polymorphism Mental Model

Remember this:

```text
                   Abstraction
                       │
                       ↓
               IPaymentService
                       │
          ┌────────────┼────────────┐
          ↓            ↓            ↓
        Cash          Card         eSewa
          │            │            │
          └────────────┼────────────┘
                       ↓
                  Pay(amount)
                       ↓
              Different behavior
```

The application depends on the abstraction:

```text
IPaymentService
```

rather than the implementation:

```text
CashPayment
CardPayment
EsewaPayment
```

---
# Summary

Polymorphism allows different objects to be treated through a common abstraction.

The basic idea is:

```text
Common abstraction
       ↓
Different implementations
       ↓
Same operation
       ↓
Different behavior
```

For example:

```text
IPaymentService
       │
       ├── CashPayment
       ├── CardPayment
       ├── EsewaPayment
       └── KhaltiPayment
```

This gives you:

```text
Polymorphism
     ↓
Loose Coupling
     ↓
Extensibility
     ↓
Maintainability
     ↓
Testability
```

The most important design principle to remember is:

> **Depend on abstractions, not concrete implementations.**

This concept becomes especially important when working with **interfaces, dependency injection, ASP.NET Core, Web APIs, Entity Framework Core, and SOLID principles**.
