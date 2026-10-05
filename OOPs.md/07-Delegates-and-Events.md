# C# Delegates and Events

## 1. Delegate

A **delegate** is a type-safe reference to a method.

It allows you to **pass a method as a parameter**.

```csharp
delegate void MessageHandler(string message);
```

Example:

```csharp
static void ShowMessage(string message)
{
    Console.WriteLine(message);
}

MessageHandler handler = ShowMessage;
handler("Hello Sandip");
```

Output:

```text
Hello Sandip
```

---

## 2. Delegate with Return Value

```csharp
delegate int Calculator(int a, int b);

static int Add(int a, int b)
{
    return a + b;
}

Calculator calculate = Add;

Console.WriteLine(calculate(10, 20));
```

Output:

```text
30
```

---

## 3. Passing Delegate to a Method

```csharp
static void Execute(Calculator calculator)
{
    Console.WriteLine(calculator(10, 20));
}
```

Call:

```csharp
Execute(Add);
```

---

## 4. Lambda with Delegate

Instead of creating a separate method:

```csharp
Calculator add = (a, b) => a + b;

Console.WriteLine(add(5, 10));
```

---

## 5. Multicast Delegate

A delegate can reference multiple methods.

```csharp
handler += Method1;
handler += Method2;
```

Remove:

```csharp
handler -= Method1;
```

Example:

```csharp
delegate void Notify();

static void Email()
{
    Console.WriteLine("Email sent");
}

static void SMS()
{
    Console.WriteLine("SMS sent");
}

Notify notify = Email;
notify += SMS;

notify();
```

---

# 6. Built-in Delegates

C# provides common generic delegates.

### Action

Returns nothing.

```csharp
Action<string> print =
    message => Console.WriteLine(message);

print("Hello");
```

### Func

Returns a value.

```csharp
Func<int, int, int> add =
    (a, b) => a + b;

Console.WriteLine(add(10, 20));
```

### Predicate

Returns `bool`.

```csharp
Predicate<int> isEven =
    number => number % 2 == 0;

Console.WriteLine(isEven(10));
```

---

# 7. Event

An **event** is used for communication between objects.

Common pattern:

```text
Publisher → Event → Subscriber
```

Example:

```csharp
class Order
{
    public event Action? OrderReady;

    public void Complete()
    {
        Console.WriteLine("Order completed");

        OrderReady?.Invoke();
    }
}
```

Subscribe:

```csharp
Order order = new();

order.OrderReady += () =>
{
    Console.WriteLine("Waiter notified");
};

order.Complete();
```

Output:

```text
Order completed
Waiter notified
```

---

# 8. EventHandler

Recommended standard event pattern:

```csharp
public event EventHandler? OrderReady;
```

Raise event:

```csharp
OrderReady?.Invoke(this, EventArgs.Empty);
```

Subscribe:

```csharp
order.OrderReady += OrderReadyHandler;
```

---

# 9. Custom EventArgs

For sending data with an event:

```csharp
class OrderEventArgs : EventArgs
{
    public int OrderId { get; set; }
}
```

Event:

```csharp
public event EventHandler<OrderEventArgs>? OrderReady;
```

Raise:

```csharp
OrderReady?.Invoke(
    this,
    new OrderEventArgs { OrderId = 101 }
);
```

---

# 10. Delegate vs Event

| Delegate                             | Event                                        |
| ------------------------------------ | -------------------------------------------- |
| References methods                   | Built around notifications                   |
| Can be called by its owner/reference | Usually can only be raised by declaring type |
| Useful for callbacks                 | Useful for publisher/subscriber              |
| Supports multicast                   | Supports subscription with `+=`              |

---

# 11. Hotel Example

```csharp
class Kitchen
{
    public event EventHandler? FoodReady;

    public void PrepareFood()
    {
        Console.WriteLine("Food prepared");

        FoodReady?.Invoke(this, EventArgs.Empty);
    }
}
```

Waiter subscribes:

```csharp
Kitchen kitchen = new();

kitchen.FoodReady += (sender, e) =>
{
    Console.WriteLine("Waiter: Serve the food");
};

kitchen.PrepareFood();
```

Output:

```text
Food prepared
Waiter: Serve the food
```

---

# 12. Important Syntax

```text
delegate → Reference to a method
Action   → No return value
Func     → Returns a value
Predicate → Returns bool
event    → Notification mechanism
+=       → Subscribe/add
-=       → Unsubscribe/remove
Invoke() → Execute/raise
```

---

# 13. Best Practices

* Use `Action`, `Func`, or `Predicate` when custom delegate types aren't needed.
* Use events for publisher/subscriber communication.
* Use `?.Invoke()` when raising nullable events.
* Unsubscribe when the subscriber's lifetime requires it.
* Prefer standard `EventHandler` patterns for public .NET-style events.

---

# Quick Revision

```text
Delegate
   ↓
Method reference
   ↓
Callback / Lambda
```

```text
Event
   ↓
Publisher
   ↓
Notification
   ↓
Subscriber
```

**Delegate = pass/hold methods.**

**Event = notify other objects when something happens.**
