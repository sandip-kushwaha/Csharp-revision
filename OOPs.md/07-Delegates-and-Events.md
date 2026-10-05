# C# Delegates and Events

Delegates and events are important features of C# used for **callbacks, notifications, event-driven programming, and loose coupling**.

The main concepts covered in this chapter are:

* Delegates
* Delegate syntax
* Passing methods to methods
* Delegate invocation
* Multicast delegates
* `Action`
* `Func`
* `Predicate`
* Anonymous methods
* Lambda expressions
* Callbacks
* Events
* Event handlers
* Custom events
* Delegates vs events
* Practical examples

---

# 1. What Is a Delegate?

A **delegate** is a type-safe reference to a method.

In simple words:

> A delegate allows a method to be stored inside a variable and passed around like data.

Normally we call a method directly:

```csharp
PrintMessage();
```

With a delegate, we can reference the method:

```csharp
MessageDelegate message = PrintMessage;

message();
```

The delegate points to the method.

---

# 2. Why Use Delegates?

Delegates are useful when:

* You want to pass a method as an argument.
* You want callbacks.
* You want to execute different methods dynamically.
* You want loose coupling.
* You want event-driven programming.
* You want reusable methods that accept custom behavior.

---

# 3. Basic Delegate Syntax

The syntax is:

```csharp
delegate returnType DelegateName(parameters);
```

Example:

```csharp
delegate void MessageDelegate(string message);
```

This delegate can reference any method that has:

```text
Return type → void
Parameter → string
```

For example:

```csharp
static void PrintMessage(string message)
{
    Console.WriteLine(message);
}
```

The method matches the delegate signature.

---

# 4. Creating a Delegate

Complete example:

```csharp
using System;

delegate void MessageDelegate(string message);

class Program
{
    static void PrintMessage(string message)
    {
        Console.WriteLine(message);
    }

    static void Main()
    {
        MessageDelegate message =
            new MessageDelegate(PrintMessage);

        message("Hello, C#!");
    }
}
```

Output:

```text
Hello, C#!
```

---

# 5. Simplified Delegate Syntax

Modern C# allows shorter syntax.

Instead of:

```csharp
MessageDelegate message =
    new MessageDelegate(PrintMessage);
```

you can write:

```csharp
MessageDelegate message = PrintMessage;
```

Then:

```csharp
message("Hello, C#!");
```

---

# 6. Delegate Signature

A delegate's signature must match the method.

Suppose:

```csharp
delegate int Calculator(int a, int b);
```

The method must return `int` and accept two `int` parameters.

Valid:

```csharp
static int Add(int a, int b)
{
    return a + b;
}
```

Valid:

```csharp
static int Multiply(int a, int b)
{
    return a * b;
}
```

Both can be assigned:

```csharp
Calculator calculator = Add;

Console.WriteLine(calculator(10, 20));
```

---

# 7. Delegate as a Method Parameter

One of the most useful features of delegates is passing a method to another method.

```csharp
delegate int Operation(int a, int b);
```

Create a method:

```csharp
static int Calculate(
    int a,
    int b,
    Operation operation)
{
    return operation(a, b);
}
```

Now:

```csharp
int result = Calculate(10, 5, Add);

Console.WriteLine(result);
```

Output:

```text
15
```

We can also pass another method:

```csharp
int result = Calculate(10, 5, Multiply);
```

Output:

```text
50
```

---

# 8. Callback

A **callback** is a method passed to another method so that it can be called later.

Example:

```csharp
delegate void Callback(string message);

static void Process(Callback callback)
{
    Console.WriteLine("Processing...");

    callback("Processing completed.");
}
```

Usage:

```csharp
Process(message =>
{
    Console.WriteLine(message);
});
```

Output:

```text
Processing...
Processing completed.
```

The delegate allows the caller to provide custom behavior.

---

# 9. Delegate Invocation

If a delegate references a method:

```csharp
MessageDelegate message = PrintMessage;
```

You can invoke it:

```csharp
message("Hello");
```

You can also use:

```csharp
message.Invoke("Hello");
```

Both call the referenced method.

Usually:

```csharp
message("Hello");
```

is cleaner.

---

# 10. Null Delegates

A delegate variable can be `null`.

```csharp
MessageDelegate message = null;
```

Calling it directly can cause:

```text
NullReferenceException
```

Use null checking:

```csharp
if (message != null)
{
    message("Hello");
}
```

Modern C# commonly uses:

```csharp
message?.Invoke("Hello");
```

This invokes the delegate only if it is not `null`.

---

# 11. Multicast Delegates

A delegate can reference multiple methods.

This is called a **multicast delegate**.

Example:

```csharp
delegate void MessageDelegate(string message);

static void First(string message)
{
    Console.WriteLine($"First: {message}");
}

static void Second(string message)
{
    Console.WriteLine($"Second: {message}");
}
```

Add both methods:

```csharp
MessageDelegate message = First;

message += Second;

message("Hello");
```

Output:

```text
First: Hello
Second: Hello
```

---

# 12. Removing a Method from a Multicast Delegate

Use `-=`:

```csharp
message -= Second;
```

Now:

```csharp
message("Hello");
```

only calls:

```text
First: Hello
```

Example:

```csharp
MessageDelegate message = First;

message += Second;
message += Third;

message -= Second;
```

The invocation list becomes:

```text
First
Third
```

---

# 13. Delegate Invocation List

A multicast delegate contains an invocation list.

Conceptually:

```text
Delegate
   │
   ├── First()
   ├── Second()
   └── Third()
```

Calling:

```csharp
message("Hello");
```

executes each method in the invocation list.

You can inspect the list:

```csharp
foreach (Delegate item in message.GetInvocationList())
{
    Console.WriteLine(item.Method.Name);
}
```

---

# 14. Delegate Return Values

Consider:

```csharp
delegate int Operation(int a, int b);
```

A delegate can return a value.

```csharp
static int Add(int a, int b)
{
    return a + b;
}
```

Usage:

```csharp
Operation operation = Add;

int result = operation(10, 20);

Console.WriteLine(result);
```

Output:

```text
30
```

For a multicast delegate with a return value, the return value from the **last invoked method** is normally the value obtained by the caller.

Therefore, multicast delegates are most commonly useful with `void` methods.

---

# 15. Anonymous Methods

An anonymous method is a method without a separate method name.

Example:

```csharp
delegate void MessageDelegate(string message);

MessageDelegate message = delegate(string text)
{
    Console.WriteLine(text);
};

message("Hello!");
```

Output:

```text
Hello!
```

Anonymous methods are created using:

```csharp
delegate
```

---

# 16. Lambda Expressions

Modern C# commonly uses lambda expressions instead of anonymous methods.

Example:

```csharp
MessageDelegate message =
    (text) =>
    {
        Console.WriteLine(text);
    };
```

Shorter:

```csharp
MessageDelegate message =
    text => Console.WriteLine(text);
```

Usage:

```csharp
message("Hello from lambda!");
```

---

# 17. Lambda Syntax

Basic lambda syntax:

```csharp
(parameters) => expression
```

Example:

```csharp
x => x * 2
```

Multiple parameters:

```csharp
(a, b) => a + b
```

Multiple statements:

```csharp
(a, b) =>
{
    int result = a + b;
    return result;
}
```

---

# 18. `Action`

C# provides built-in delegate types.

`Action` represents a method that:

* Returns `void`
* Can have zero or more parameters

Example:

```csharp
Action message = () =>
{
    Console.WriteLine("Hello!");
};

message();
```

---

# 19. `Action<T>`

`Action<T>` accepts a parameter.

```csharp
Action<string> print =
    message => Console.WriteLine(message);

print("Hello C#!");
```

Multiple parameters:

```csharp
Action<string, int> display =
    (name, age) =>
    {
        Console.WriteLine($"Name: {name}");
        Console.WriteLine($"Age: {age}");
    };

display("Sandip", 22);
```

---

# 20. `Action` Variations

Examples:

```csharp
Action
```

No parameters.

```csharp
Action<int>
```

One parameter.

```csharp
Action<int, int>
```

Two parameters.

```csharp
Action<string, int, bool>
```

Three parameters.

`Action` always returns:

```text
void
```

---

# 21. `Func`

`Func` represents a delegate that returns a value.

The **last generic parameter is the return type**.

Example:

```csharp
Func<int, int> square =
    number => number * number;

Console.WriteLine(square(5));
```

Output:

```text
25
```

---

# 22. `Func` with Multiple Parameters

```csharp
Func<int, int, int> add =
    (a, b) => a + b;

Console.WriteLine(add(10, 20));
```

Output:

```text
30
```

Here:

```text
int → first parameter
int → second parameter
int → return type
```

---

# 23. `Func` Syntax

Examples:

```csharp
Func<int>
```

No input, returns `int`.

```csharp
Func<int, int>
```

Takes `int`, returns `int`.

```csharp
Func<int, int, int>
```

Takes two `int` values, returns `int`.

```csharp
Func<string, bool>
```

Takes a `string`, returns `bool`.

---

# 24. `Predicate<T>`

`Predicate<T>` represents a method that:

* Accepts one parameter
* Returns `bool`

Example:

```csharp
Predicate<int> isEven =
    number => number % 2 == 0;

Console.WriteLine(isEven(10));
```

Output:

```text
True
```

Another example:

```csharp
Predicate<string> isLongName =
    name => name.Length > 5;

Console.WriteLine(isLongName("Sandip"));
```

Output:

```text
True
```

---

# 25. Action vs Func vs Predicate

| Type               | Parameters | Return    |
| ------------------ | ---------- | --------- |
| `Action`           | 0+         | `void`    |
| `Action<T>`        | 1+         | `void`    |
| `Func<TResult>`    | 0          | `TResult` |
| `Func<T, TResult>` | 1+         | `TResult` |
| `Predicate<T>`     | 1          | `bool`    |

### Easy memory trick

```text
Action    → Does something
Func      → Returns something
Predicate → Answers true/false
```

---

# 26. Delegate vs Lambda

A delegate is a **type** that represents a method signature.

```csharp
delegate int Calculator(int a, int b);
```

A lambda is a convenient way to provide the implementation:

```csharp
Calculator add = (a, b) => a + b;
```

So:

```text
Delegate
   ↓
Defines what method shape is allowed

Lambda
   ↓
Provides behavior
```

---

# 27. Delegates and LINQ

Delegates are heavily used by LINQ.

For example:

```csharp
List<int> numbers =
    new() { 1, 2, 3, 4, 5, 6 };

var evenNumbers =
    numbers.Where(number => number % 2 == 0);
```

The lambda:

```csharp
number => number % 2 == 0
```

is passed as a delegate-like expression to LINQ.

This is one reason understanding delegates helps you understand LINQ.

---

# 28. Events

An **event** is a mechanism that allows an object to notify other objects when something happens.

Examples:

* Button clicked
* Order created
* Payment completed
* User registered
* File downloaded
* Temperature changed
* Message received

The object that raises the event does not need to know exactly who is listening.

---

# 29. Real-World Event Example

Imagine a hotel order system.

When an order is ready:

```text
Kitchen
   ↓
Order Ready Event
   ↓
Waiter
   ↓
Notification
```

The kitchen does not need to directly call a waiter method.

It raises an event.

---

# 30. Basic Event Syntax

An event commonly uses:

```csharp
public event EventHandler SomethingHappened;
```

Example:

```csharp
class Order
{
    public event EventHandler OrderReady;

    public void Complete()
    {
        Console.WriteLine("Order completed.");

        OrderReady?.Invoke(this, EventArgs.Empty);
    }
}
```

---

# 31. Subscribing to an Event

Create an event handler:

```csharp
static void HandleOrderReady(
    object? sender,
    EventArgs e)
{
    Console.WriteLine(
        "Waiter notified: Order is ready."
    );
}
```

Subscribe:

```csharp
Order order = new Order();

order.OrderReady += HandleOrderReady;
```

Trigger the event:

```csharp
order.Complete();
```

Output:

```text
Order completed.
Waiter notified: Order is ready.
```

---

# 32. Understanding `+=` and `-=`

Subscribe:

```csharp
order.OrderReady += HandleOrderReady;
```

Unsubscribe:

```csharp
order.OrderReady -= HandleOrderReady;
```

Conceptually:

```text
+=
 ↓
Start listening

-=
 ↓
Stop listening
```

---

# 33. Why Events Use Delegates

Events are built on delegates.

Conceptually:

```text
Delegate
   ↓
Provides method references
   ↓
Event
   ↓
Provides controlled notification
```

The event uses a delegate type to define what handlers can subscribe.

---

# 34. Delegate vs Event

A delegate can generally be invoked by code that has access to it.

For example:

```csharp
public Action OnCompleted;
```

External code could potentially do:

```csharp
object.OnCompleted?.Invoke();
```

An event provides more controlled access:

```csharp
public event Action OnCompleted;
```

External code can subscribe:

```csharp
object.OnCompleted += Handler;
```

but cannot directly raise the event from outside the declaring type.

This is one of the key reasons to use events.

---

# 35. Event Handler Pattern

The common .NET event pattern is:

```csharp
void Handler(object? sender, EventArgs e)
{
}
```

For example:

```csharp
static void OnOrderReady(
    object? sender,
    EventArgs e)
{
    Console.WriteLine("Order ready.");
}
```

The event can then be:

```csharp
public event EventHandler? OrderReady;
```

---

# 36. Custom Event Arguments

Sometimes an event needs to send information.

For example:

```text
Order Ready
Order ID = 101
Table = T-03
```

Create custom event arguments:

```csharp
class OrderReadyEventArgs : EventArgs
{
    public int OrderId { get; }

    public int TableNumber { get; }

    public OrderReadyEventArgs(
        int orderId,
        int tableNumber)
    {
        OrderId = orderId;
        TableNumber = tableNumber;
    }
}
```

---

# 37. Event with Custom Event Arguments

Create the event:

```csharp
class Order
{
    public event EventHandler<OrderReadyEventArgs>?
        OrderReady;

    public void Complete(
        int orderId,
        int tableNumber)
    {
        Console.WriteLine("Order completed.");

        OrderReady?.Invoke(
            this,
            new OrderReadyEventArgs(
                orderId,
                tableNumber
            )
        );
    }
}
```

---

# 38. Handling Custom Event Data

```csharp
static void HandleOrderReady(
    object? sender,
    OrderReadyEventArgs e)
{
    Console.WriteLine(
        $"Order {e.OrderId} is ready."
    );

    Console.WriteLine(
        $"Table: {e.TableNumber}"
    );
}
```

Subscribe:

```csharp
Order order = new Order();

order.OrderReady += HandleOrderReady;
```

Trigger:

```csharp
order.Complete(101, 3);
```

Output:

```text
Order completed.
Order 101 is ready.
Table: 3
```

---

# 39. Event Flow

The event system can be understood as:

```text
Publisher
    │
    │ raises event
    ↓
Event
    │
    ├───────────────┐
    ↓               ↓
Subscriber 1    Subscriber 2
    ↓               ↓
Handler         Handler
```

For a hotel system:

```text
Kitchen
   │
   │ OrderReady
   ↓
Event
   │
   ├── Waiter notification
   ├── Customer notification
   └── Dashboard update
```

---

# 40. Publisher and Subscriber

### Publisher

The object that raises the event.

Example:

```csharp
class Order
{
    public event EventHandler? OrderReady;
}
```

### Subscriber

The object that listens to the event.

```csharp
order.OrderReady += HandleOrderReady;
```

Easy way to remember:

```text
Publisher → Announces
Subscriber → Listens
```

---

# 41. Lambda Event Handler

Instead of creating a separate method:

```csharp
order.OrderReady += HandleOrderReady;
```

you can use a lambda:

```csharp
order.OrderReady += (sender, e) =>
{
    Console.WriteLine("Order is ready.");
};
```

This is useful for short event handlers.

---

# 42. Unsubscribing Lambda Handlers

Be careful with anonymous lambdas.

For example:

```csharp
order.OrderReady += (sender, e) =>
{
    Console.WriteLine("Ready");
};
```

You cannot easily unsubscribe later using a newly created identical lambda because it is a different delegate instance.

If you need to unsubscribe, keep a reference to the handler:

```csharp
EventHandler handler = (sender, e) =>
{
    Console.WriteLine("Ready");
};

order.OrderReady += handler;

order.OrderReady -= handler;
```

---

# 43. Event Access Restrictions

Suppose:

```csharp
class Order
{
    public event EventHandler? OrderReady;

    public void Complete()
    {
        OrderReady?.Invoke(
            this,
            EventArgs.Empty
        );
    }
}
```

Inside the `Order` class, we can raise:

```csharp
OrderReady?.Invoke(...);
```

Outside the class, subscribers can do:

```csharp
order.OrderReady += Handler;
```

but they cannot directly invoke:

```csharp
order.OrderReady?.Invoke(...);
```

This provides encapsulation.

---

# 44. Custom Event Accessors

C# also allows custom event accessors:

```csharp
private EventHandler? handlers;

public event EventHandler SomethingHappened
{
    add
    {
        handlers += value;
    }

    remove
    {
        handlers -= value;
    }
}
```

The `add` accessor controls subscription.

The `remove` accessor controls unsubscription.

Most applications don't need custom event accessors, but they are useful when implementing advanced event behavior.

---

# 45. Event Example: Button Click

A simple event-driven model:

```csharp
class Button
{
    public event EventHandler? Click;

    public void Press()
    {
        Console.WriteLine("Button pressed.");

        Click?.Invoke(
            this,
            EventArgs.Empty
        );
    }
}
```

Usage:

```csharp
Button button = new Button();

button.Click += (sender, e) =>
{
    Console.WriteLine("Button clicked.");
};

button.Press();
```

Output:

```text
Button pressed.
Button clicked.
```

---

# 46. Event Example: Temperature

```csharp
class TemperatureSensor
{
    public event EventHandler? TemperatureChanged;

    public void ChangeTemperature()
    {
        Console.WriteLine("Temperature changed.");

        TemperatureChanged?.Invoke(
            this,
            EventArgs.Empty
        );
    }
}
```

Subscriber:

```csharp
TemperatureSensor sensor =
    new TemperatureSensor();

sensor.TemperatureChanged += (sender, e) =>
{
    Console.WriteLine(
        "Temperature notification received."
    );
};

sensor.ChangeTemperature();
```

---

# 47. Hotel Management Event Example

Imagine your hotel system has:

```text
Kitchen
Waiter
Customer
Dashboard
```

When the kitchen marks an order as ready:

```text
Kitchen
   ↓
OrderReady event
   ↓
┌──────────────┬──────────────┬──────────────┐
│              │              │
Waiter       Customer      Dashboard
```

Example:

```csharp
class KitchenOrder
{
    public event EventHandler<OrderReadyEventArgs>?
        OrderReady;

    public void MarkReady(
        int orderId,
        int tableNumber)
    {
        Console.WriteLine(
            $"Order {orderId} is ready."
        );

        OrderReady?.Invoke(
            this,
            new OrderReadyEventArgs(
                orderId,
                tableNumber
            )
        );
    }
}
```

Multiple subscribers:

```csharp
KitchenOrder order = new KitchenOrder();

order.OrderReady += (sender, e) =>
{
    Console.WriteLine(
        $"Waiter notified for table {e.TableNumber}."
    );
};

order.OrderReady += (sender, e) =>
{
    Console.WriteLine(
        $"Dashboard updated for order {e.OrderId}."
    );
};
```

Trigger:

```csharp
order.MarkReady(101, 3);
```

Output:

```text
Order 101 is ready.
Waiter notified for table 3.
Dashboard updated for order 101.
```

---

# 48. Delegates for Dependency Injection

Delegates can also be used to inject behavior.

Example:

```csharp
class Calculator
{
    public int Calculate(
        int a,
        int b,
        Func<int, int, int> operation)
    {
        return operation(a, b);
    }
}
```

Usage:

```csharp
Calculator calculator = new Calculator();

int sum = calculator.Calculate(
    10,
    20,
    (a, b) => a + b
);

int product = calculator.Calculate(
    10,
    20,
    (a, b) => a * b
);
```

The calculator does not need separate methods for every possible operation.

---

# 49. Delegates in Asynchronous Code

Delegates are also related to callback patterns.

For example:

```csharp
Action<string> callback =
    message => Console.WriteLine(message);
```

A method can receive it:

```csharp
static async Task ProcessAsync(
    Action<string> callback)
{
    await Task.Delay(1000);

    callback("Processing completed.");
}
```

Usage:

```csharp
await ProcessAsync(
    message => Console.WriteLine(message)
);
```

Modern C# often prefers `Task`, `async`, and `await` for asynchronous workflows, but delegates still appear in callback APIs and framework code.

---

# 50. Delegate vs Interface

Both can provide flexible behavior, but they solve different problems.

### Delegate

Best when you need:

```text
One piece of behavior
```

Example:

```csharp
Func<int, int> operation
```

### Interface

Best when you need:

```text
A contract containing multiple members
```

Example:

```csharp
interface IPaymentService
{
    void Pay();
    void Refund();
}
```

### Simple rule

```text
One behavior
    ↓
Delegate

Multiple related behaviors
    ↓
Interface
```

---

# 51. Delegate vs Event

| Feature                  | Delegate                       | Event                    |
| ------------------------ | ------------------------------ | ------------------------ |
| Represents methods       | Yes                            | Yes                      |
| Pass method as parameter | Yes                            | Not its main purpose     |
| Callback                 | Yes                            | Can support notification |
| Multiple subscribers     | Yes                            | Yes                      |
| External invocation      | Possible depending on exposure | Restricted               |
| Main purpose             | Behavior/callback              | Notification             |
| Uses `+=` / `-=`         | Yes                            | Yes                      |

---

# 52. Common Mistakes

## Mistake 1: Wrong delegate signature

```csharp
delegate int Calculator(int a, int b);
```

This method doesn't match:

```csharp
static void Add(int a, int b)
{
}
```

The return type is different.

---

## Mistake 2: Forgetting null checks

Potentially unsafe:

```csharp
handler();
```

Safer:

```csharp
handler?.Invoke();
```

---

## Mistake 3: Raising events from outside the class

If:

```csharp
public event EventHandler? Completed;
```

external code should subscribe:

```csharp
object.Completed += Handler;
```

The owning class should normally raise the event.

---

## Mistake 4: Forgetting to unsubscribe

Long-lived objects can keep references to event subscribers.

When appropriate:

```csharp
publisher.Event -= handler;
```

This can be important for avoiding unintended object retention.

---

# 53. Best Practices

### 1. Use meaningful delegate names

```csharp
delegate void OrderHandler(int orderId);
```

is clearer than:

```csharp
delegate void MyDelegate(int x);
```

---

### 2. Prefer built-in delegates when appropriate

Use:

```csharp
Action
Func
Predicate
```

when a custom delegate name adds no value.

---

### 3. Use events for notifications

If an object is announcing that something happened:

```csharp
public event EventHandler? Completed;
```

is usually more appropriate than exposing a public delegate field.

---

### 4. Use `?.Invoke()`

```csharp
Completed?.Invoke(
    this,
    EventArgs.Empty
);
```

This safely handles the case where there are no subscribers.

---

### 5. Unsubscribe when necessary

```csharp
publisher.Event -= handler;
```

Especially for long-lived publishers/subscribers.

---

### 6. Keep event handlers lightweight

If significant work must happen, consider moving that work into a service or asynchronous workflow.

---

# 54. Complete Delegate Example

```csharp
using System;

class Program
{
    static int Add(int a, int b)
    {
        return a + b;
    }

    static int Multiply(int a, int b)
    {
        return a * b;
    }

    static int Calculate(
        int a,
        int b,
        Func<int, int, int> operation)
    {
        return operation(a, b);
    }

    static void Main()
    {
        int sum = Calculate(10, 20, Add);

        int product = Calculate(
            10,
            20,
            Multiply
        );

        int difference = Calculate(
            20,
            10,
            (a, b) => a - b
        );

        Console.WriteLine($"Sum: {sum}");
        Console.WriteLine($"Product: {product}");
        Console.WriteLine($"Difference: {difference}");
    }
}
```

Output:

```text
Sum: 30
Product: 200
Difference: 10
```

---

# 55. Complete Event Example

```csharp
using System;

class OrderReadyEventArgs : EventArgs
{
    public int OrderId { get; }

    public int TableNumber { get; }

    public OrderReadyEventArgs(
        int orderId,
        int tableNumber)
    {
        OrderId = orderId;
        TableNumber = tableNumber;
    }
}

class KitchenOrder
{
    public event EventHandler<OrderReadyEventArgs>?
        OrderReady;

    public void MarkReady(
        int orderId,
        int tableNumber)
    {
        Console.WriteLine(
            $"Kitchen: Order {orderId} is ready."
        );

        OrderReady?.Invoke(
            this,
            new OrderReadyEventArgs(
                orderId,
                tableNumber
            )
        );
    }
}

class Program
{
    static void Main()
    {
        KitchenOrder order = new KitchenOrder();

        order.OrderReady += OnOrderReady;

        order.OrderReady += (sender, e) =>
        {
            Console.WriteLine(
                $"Dashboard updated for order {e.OrderId}."
            );
        };

        order.MarkReady(101, 3);

        order.OrderReady -= OnOrderReady;
    }

    static void OnOrderReady(
        object? sender,
        OrderReadyEventArgs e)
    {
        Console.WriteLine(
            $"Waiter notified for table {e.TableNumber}."
        );
    }
}
```

Output:

```text
Kitchen: Order 101 is ready.
Waiter notified for table 3.
Dashboard updated for order 101.
```

---

# 56. Delegate and Event Mental Model

Think about a delegate as a **function holder**:

```text
Delegate
   │
   ├── Method A
   ├── Method B
   └── Method C
```

Think about an event as a **notification system**:

```text
Publisher
    │
    │ raises event
    ↓
  EVENT
    │
    ├── Subscriber A
    ├── Subscriber B
    └── Subscriber C
```

### Simple mental model

```text
Delegate → "Which method should I call?"

Event   → "Something happened; who wants to know?"
```

---

# 57. Quick Revision

### Custom delegate

```csharp
delegate void MessageHandler(string message);
```

### Assign method

```csharp
MessageHandler handler = PrintMessage;
```

### Invoke

```csharp
handler("Hello");
```

### Add method

```csharp
handler += AnotherMethod;
```

### Remove method

```csharp
handler -= AnotherMethod;
```

### `Action`

```csharp
Action<string> print =
    message => Console.WriteLine(message);
```

### `Func`

```csharp
Func<int, int, int> add =
    (a, b) => a + b;
```

### `Predicate`

```csharp
Predicate<int> isEven =
    number => number % 2 == 0;
```

### Event

```csharp
public event EventHandler? Completed;
```

### Raise event

```csharp
Completed?.Invoke(
    this,
    EventArgs.Empty
);
```

### Subscribe

```csharp
object.Completed += Handler;
```

### Unsubscribe

```csharp
object.Completed -= Handler;
```

---

# Summary

A **delegate** is a type-safe reference to a method.

```csharp
Func<int, int, int> add =
    (a, b) => a + b;
```

An **event** provides a controlled notification mechanism:

```csharp
public event EventHandler? Completed;
```

The relationship is:

```text
Delegate
   ↓
References methods

Event
   ↓
Uses delegates for notification
```

The easiest way to remember the difference:

> **Delegate = pass or store behavior.**

> **Event = notify subscribers that something happened.**

These concepts are especially useful for callbacks, LINQ, event-driven applications, UI programming, and loosely coupled .NET applications.
