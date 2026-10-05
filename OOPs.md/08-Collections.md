# C# Collections

Collections are used to **store and manage multiple values** in a single object.

For example, in a hotel management system, we may need to store:

* Multiple food items
* Multiple customers
* Multiple orders
* Table information
* User roles
* Pending orders

C# provides different collection types depending on how we want to store, search, sort, or process data.

---

## 1. Why Collections?

An array has a fixed size:

```csharp
string[] foods = new string[3];

foods[0] = "Pizza";
foods[1] = "Burger";
foods[2] = "Momo";
```

If we later need 10 foods instead of 3, the array cannot automatically grow.

Collections are more flexible:

```csharp
List<string> foods = new List<string>();

foods.Add("Pizza");
foods.Add("Burger");
foods.Add("Momo");
foods.Add("Chowmein");
```

The `List<T>` can grow dynamically.

---

# 2. Main C# Collections

The most commonly used collections are:

| Collection                      | Purpose                     |
| ------------------------------- | --------------------------- |
| `List<T>`                       | Ordered collection of items |
| `Dictionary<TKey,TValue>`       | Key-value data              |
| `HashSet<T>`                    | Unique values               |
| `Queue<T>`                      | FIFO                        |
| `Stack<T>`                      | LIFO                        |
| `LinkedList<T>`                 | Node-based collection       |
| `SortedSet<T>`                  | Unique sorted values        |
| `SortedDictionary<TKey,TValue>` | Sorted key-value data       |

Most generic collections are available through:

```csharp
using System.Collections.Generic;
```

---

# 3. List<T>

`List<T>` is one of the most commonly used collections in C#.

It stores items in an ordered collection and allows duplicate values.

## Create a List

```csharp
List<string> foods = new List<string>();
```

Or:

```csharp
var foods = new List<string>();
```

Add items:

```csharp
foods.Add("Pizza");
foods.Add("Burger");
foods.Add("Momo");
```

---

## 3.1 List Initialization

You can directly initialize a list:

```csharp
List<string> foods = new List<string>
{
    "Pizza",
    "Burger",
    "Momo"
};
```

Modern C# also supports collection expressions:

```csharp
List<string> foods = ["Pizza", "Burger", "Momo"];
```

> Collection expressions such as `[]` are available in modern C# versions, including C# 12+.

---

## 3.2 Add()

Adds one item.

```csharp
List<string> foods = new();

foods.Add("Pizza");
foods.Add("Burger");

Console.WriteLine(foods[0]);
```

Output:

```text
Pizza
```

---

## 3.3 AddRange()

Adds multiple items.

```csharp
List<string> foods = new()
{
    "Pizza",
    "Burger"
};

foods.AddRange(["Momo", "Chowmein", "Pasta"]);
```

---

## 3.4 Insert()

Adds an item at a specific index.

```csharp
List<string> foods = ["Pizza", "Burger", "Momo"];

foods.Insert(1, "Pasta");
```

Result:

```text
Pizza
Pasta
Burger
Momo
```

---

## 3.5 Remove()

Removes the first matching value.

```csharp
foods.Remove("Burger");
```

---

## 3.6 RemoveAt()

Removes an item using its index.

```csharp
foods.RemoveAt(0);
```

---

## 3.7 Count

Gets the number of items.

```csharp
Console.WriteLine(foods.Count);
```

---

## 3.8 Contains()

Checks whether an item exists.

```csharp
if (foods.Contains("Pizza"))
{
    Console.WriteLine("Pizza is available.");
}
```

---

## 3.9 Clear()

Removes all items.

```csharp
foods.Clear();
```

---

## 3.10 Loop Through List

### `foreach`

```csharp
foreach (string food in foods)
{
    Console.WriteLine(food);
}
```

### `for`

```csharp
for (int i = 0; i < foods.Count; i++)
{
    Console.WriteLine(foods[i]);
}
```

---

# 4. Dictionary<TKey, TValue>

A `Dictionary` stores data as:

```text
Key → Value
```

For example:

```text
101 → Sandip
102 → Ram
103 → Hari
```

Create:

```csharp
Dictionary<int, string> customers = new();
```

Add values:

```csharp
customers.Add(101, "Sandip");
customers.Add(102, "Ram");
customers.Add(103, "Hari");
```

---

## 4.1 Access Value

```csharp
Console.WriteLine(customers[101]);
```

Output:

```text
Sandip
```

---

## 4.2 Dictionary Initializer

```csharp
Dictionary<int, string> customers = new()
{
    { 101, "Sandip" },
    { 102, "Ram" },
    { 103, "Hari" }
};
```

Modern syntax:

```csharp
Dictionary<int, string> customers = new()
{
    [101] = "Sandip",
    [102] = "Ram",
    [103] = "Hari"
};
```

---

## 4.3 ContainsKey()

Checks whether a key exists.

```csharp
if (customers.ContainsKey(101))
{
    Console.WriteLine("Customer exists.");
}
```

---

## 4.4 TryGetValue()

A safer way to retrieve a value:

```csharp
if (customers.TryGetValue(101, out string? customer))
{
    Console.WriteLine(customer);
}
```

This avoids an exception when the key does not exist.

---

## 4.5 Remove()

```csharp
customers.Remove(102);
```

---

## 4.6 Dictionary Count

```csharp
Console.WriteLine(customers.Count);
```

---

## 4.7 Loop Through Dictionary

```csharp
foreach (var customer in customers)
{
    Console.WriteLine($"{customer.Key} - {customer.Value}");
}
```

Output:

```text
101 - Sandip
102 - Ram
103 - Hari
```

---

# 5. HashSet<T>

`HashSet<T>` stores **unique values**.

Duplicate values are automatically ignored.

```csharp
HashSet<string> roles = new();

roles.Add("Admin");
roles.Add("Waiter");
roles.Add("Kitchen");
roles.Add("Admin");
```

The result contains:

```text
Admin
Waiter
Kitchen
```

`Admin` is stored only once.

---

## 5.1 Contains()

```csharp
if (roles.Contains("Admin"))
{
    Console.WriteLine("Admin exists.");
}
```

---

## 5.2 Remove()

```csharp
roles.Remove("Kitchen");
```

---

## 5.3 Set Operations

### Union

Combines unique values.

```csharp
HashSet<int> first = [1, 2, 3];
HashSet<int> second = [3, 4, 5];

first.UnionWith(second);
```

Result:

```text
1, 2, 3, 4, 5
```

### Intersection

Keeps common values.

```csharp
HashSet<int> first = [1, 2, 3];
HashSet<int> second = [2, 3, 4];

first.IntersectWith(second);
```

Result:

```text
2, 3
```

### Difference

```csharp
HashSet<int> first = [1, 2, 3];
HashSet<int> second = [2, 3];

first.ExceptWith(second);
```

Result:

```text
1
```

---

# 6. Queue<T>

A queue follows:

```text
FIFO
First In, First Out
```

The first item added is the first item removed.

Real-world example:

```text
Customer 1
Customer 2
Customer 3

Customer 1 is served first.
```

Create:

```csharp
Queue<string> orders = new();
```

Add:

```csharp
orders.Enqueue("Order 101");
orders.Enqueue("Order 102");
orders.Enqueue("Order 103");
```

---

## 6.1 Dequeue()

Removes the first item.

```csharp
string order = orders.Dequeue();

Console.WriteLine(order);
```

Output:

```text
Order 101
```

---

## 6.2 Peek()

Views the first item without removing it.

```csharp
Console.WriteLine(orders.Peek());
```

---

## 6.3 TryDequeue()

Safer when the queue may be empty:

```csharp
if (orders.TryDequeue(out string? order))
{
    Console.WriteLine(order);
}
```

---

## 6.4 Count

```csharp
Console.WriteLine(orders.Count);
```

---

# 7. Stack<T>

A stack follows:

```text
LIFO
Last In, First Out
```

The last item added is the first item removed.

Real-world example:

```text
Plate 1
Plate 2
Plate 3

Plate 3 is removed first.
```

Create:

```csharp
Stack<string> pages = new();
```

Add:

```csharp
pages.Push("Home");
pages.Push("Menu");
pages.Push("Order");
```

---

## 7.1 Pop()

Removes the top item.

```csharp
string page = pages.Pop();

Console.WriteLine(page);
```

Output:

```text
Order
```

---

## 7.2 Peek()

Views the top item.

```csharp
Console.WriteLine(pages.Peek());
```

---

## 7.3 TryPop()

Safer when the stack might be empty:

```csharp
if (pages.TryPop(out string? page))
{
    Console.WriteLine(page);
}
```

---

# 8. LinkedList<T>

`LinkedList<T>` stores elements as nodes.

Each node is connected to the next and previous node.

Conceptually:

```text
Node → Node → Node → Node
```

Create:

```csharp
LinkedList<string> foods = new();

foods.AddLast("Pizza");
foods.AddLast("Burger");
foods.AddLast("Momo");
```

---

## 8.1 AddFirst()

```csharp
foods.AddFirst("Salad");
```

---

## 8.2 AddLast()

```csharp
foods.AddLast("Pasta");
```

---

## 8.3 AddBefore()

```csharp
LinkedListNode<string>? node = foods.Find("Burger");

if (node != null)
{
    foods.AddBefore(node, "Momo");
}
```

---

## 8.4 AddAfter()

```csharp
LinkedListNode<string>? node = foods.Find("Burger");

if (node != null)
{
    foods.AddAfter(node, "Pasta");
}
```

---

## 8.5 Remove()

```csharp
foods.Remove("Pizza");
```

---

### When should you use LinkedList?

Use it when you frequently need insertion/removal around known nodes.

For most normal application code, `List<T>` is usually the better default.

---

# 9. SortedSet<T>

`SortedSet<T>` stores:

* Unique values
* In sorted order

Example:

```csharp
SortedSet<int> numbers = [5, 1, 3, 2, 4];
```

Output order:

```text
1
2
3
4
5
```

Duplicates are removed automatically.

---

# 10. SortedDictionary<TKey, TValue>

`SortedDictionary` stores key-value pairs sorted by key.

```csharp
SortedDictionary<int, string> students = new()
{
    [103] = "Hari",
    [101] = "Sandip",
    [102] = "Ram"
};
```

The keys are maintained in sorted order:

```text
101 - Sandip
102 - Ram
103 - Hari
```

---

# 11. Collection Interfaces

C# provides interfaces that describe collection behavior.

Important interfaces include:

| Interface                          | Purpose                     |
| ---------------------------------- | --------------------------- |
| `IEnumerable<T>`                   | Can be iterated             |
| `ICollection<T>`                   | Basic collection operations |
| `IList<T>`                         | List-style collection       |
| `IDictionary<TKey,TValue>`         | Key-value collection        |
| `ISet<T>`                          | Set operations              |
| `IReadOnlyList<T>`                 | Read-only list              |
| `IReadOnlyDictionary<TKey,TValue>` | Read-only dictionary        |

Example:

```csharp
IEnumerable<string> foods = new List<string>
{
    "Pizza",
    "Burger",
    "Momo"
};
```

You can iterate:

```csharp
foreach (string food in foods)
{
    Console.WriteLine(food);
}
```

---

# 12. Why Use Interfaces?

Instead of:

```csharp
void PrintFoods(List<string> foods)
{
    // ...
}
```

You can sometimes use:

```csharp
void PrintFoods(IEnumerable<string> foods)
{
    foreach (string food in foods)
    {
        Console.WriteLine(food);
    }
}
```

Now the method can work with many enumerable collections.

```csharp
List<string> foods = ["Pizza", "Burger"];

PrintFoods(foods);
```

This supports **flexibility and loose coupling**.

---

# 13. Generic Collections vs Non-Generic Collections

Modern C# applications generally prefer generic collections.

### Generic

```csharp
List<string> names = new();
```

The list accepts only strings.

```csharp
names.Add("Sandip");
```

This would be invalid:

```csharp
names.Add(100);
```

### Older non-generic collection

```csharp
ArrayList values = new();

values.Add("Sandip");
values.Add(22);
```

Non-generic collections can store different types but often require casting and boxing/unboxing.

Common older collections include:

```text
ArrayList
Hashtable
```

Prefer:

```text
List<T>
Dictionary<TKey,TValue>
HashSet<T>
```

for modern C# applications.

---

# 14. Searching Collections

For a `List<T>`:

```csharp
List<int> numbers = [10, 20, 30, 40, 50];
```

### Contains

```csharp
bool exists = numbers.Contains(30);
```

### IndexOf

```csharp
int index = numbers.IndexOf(30);
```

### Find

```csharp
int result = numbers.Find(x => x > 25);
```

### FindAll

```csharp
List<int> results = numbers.FindAll(x => x > 25);
```

### Exists

```csharp
bool exists = numbers.Exists(x => x > 40);
```

These methods use lambda expressions, which become especially important when learning LINQ.

---

# 15. Sorting a List

```csharp
List<int> numbers = [5, 2, 8, 1, 3];

numbers.Sort();
```

Result:

```text
1
2
3
5
8
```

Reverse:

```csharp
numbers.Reverse();
```

---

## Sorting Objects

Example:

```csharp
class Food
{
    public string Name { get; set; } = "";
    public decimal Price { get; set; }
}
```

Create:

```csharp
List<Food> foods =
[
    new Food { Name = "Pizza", Price = 500 },
    new Food { Name = "Momo", Price = 250 },
    new Food { Name = "Burger", Price = 350 }
];
```

Sort by price:

```csharp
foods.Sort((a, b) => a.Price.CompareTo(b.Price));
```

---

# 16. Read-Only Collections

Sometimes you want other code to read a collection but not modify it.

You can use:

```csharp
IReadOnlyList<string> foods =
[
    "Pizza",
    "Burger",
    "Momo"
];
```

Another option:

```csharp
List<string> foods = ["Pizza", "Burger"];

IReadOnlyList<string> readOnlyFoods = foods;
```

This is useful when designing APIs and application layers.

---

# 17. Concurrent Collections

For multithreaded applications, .NET provides thread-safe collections.

Namespace:

```csharp
using System.Collections.Concurrent;
```

Examples:

```text
ConcurrentDictionary<TKey,TValue>
ConcurrentQueue<T>
ConcurrentStack<T>
ConcurrentBag<T>
```

Example:

```csharp
ConcurrentDictionary<int, string> users = new();

users.TryAdd(1, "Sandip");
```

These are useful when multiple threads/tasks access a collection concurrently.

---

# 18. Hotel Management Examples

Collections are heavily used in real applications.

### Food List

```csharp
List<string> foods =
[
    "Pizza",
    "Burger",
    "Momo",
    "Chowmein"
];
```

---

### Table Dictionary

```csharp
Dictionary<int, string> tables = new()
{
    [1] = "Available",
    [2] = "Occupied",
    [3] = "Available",
    [4] = "Cleaning"
};
```

Access:

```csharp
Console.WriteLine(tables[2]);
```

Output:

```text
Occupied
```

---

### Unique Roles

```csharp
HashSet<string> roles =
[
    "Admin",
    "Waiter",
    "Kitchen"
];
```

---

### Pending Orders

A queue is suitable for orders waiting to be processed:

```csharp
Queue<string> pendingOrders = new();

pendingOrders.Enqueue("Order #101");
pendingOrders.Enqueue("Order #102");
pendingOrders.Enqueue("Order #103");
```

Kitchen processes:

```csharp
if (pendingOrders.TryDequeue(out string? order))
{
    Console.WriteLine($"Processing {order}");
}
```

---

### Undo Actions

A stack can store recent actions:

```csharp
Stack<string> actions = new();

actions.Push("Added food");
actions.Push("Updated price");
actions.Push("Deleted category");
```

Undo the latest action:

```csharp
if (actions.TryPop(out string? action))
{
    Console.WriteLine($"Undo: {action}");
}
```

---

# 19. Complete Example

```csharp
using System;
using System.Collections.Generic;

class Order
{
    public int Id { get; set; }
    public string Food { get; set; }
    public decimal Price { get; set; }

    public Order(int id, string food, decimal price)
    {
        Id = id;
        Food = food;
        Price = price;
    }
}

class Program
{
    static void Main()
    {
        List<Order> orders =
        [
            new Order(101, "Pizza", 500),
            new Order(102, "Momo", 250),
            new Order(103, "Burger", 350)
        ];

        Console.WriteLine("All Orders:");

        foreach (Order order in orders)
        {
            Console.WriteLine(
                $"{order.Id} - {order.Food} - Rs. {order.Price}"
            );
        }

        Dictionary<int, string> tableStatus = new()
        {
            [1] = "Available",
            [2] = "Occupied",
            [3] = "Cleaning"
        };

        Console.WriteLine(
            $"\nTable 2: {tableStatus[2]}"
        );

        Queue<Order> pendingOrders = new();

        foreach (Order order in orders)
        {
            pendingOrders.Enqueue(order);
        }

        Console.WriteLine("\nProcessing Orders:");

        while (pendingOrders.TryDequeue(out Order? orderToProcess))
        {
            Console.WriteLine(
                $"Processing {orderToProcess.Food}"
            );
        }
    }
}
```

Output:

```text
All Orders:
101 - Pizza - Rs. 500
102 - Momo - Rs. 250
103 - Burger - Rs. 350

Table 2: Occupied

Processing Orders:
Processing Pizza
Processing Momo
Processing Burger
```

---

# 20. Common Mistakes

## Accessing an Invalid List Index

```csharp
List<string> foods = ["Pizza", "Burger"];

Console.WriteLine(foods[5]);
```

This causes:

```text
ArgumentOutOfRangeException
```

Always make sure the index is valid.

---

## Accessing a Missing Dictionary Key

This can throw an exception:

```csharp
Console.WriteLine(customers[999]);
```

Prefer:

```csharp
if (customers.TryGetValue(999, out string? customer))
{
    Console.WriteLine(customer);
}
```

---

## Duplicate Dictionary Key

This causes an exception:

```csharp
customers.Add(101, "Sandip");
customers.Add(101, "Ram");
```

A dictionary cannot have duplicate keys.

You can update using:

```csharp
customers[101] = "Ram";
```

---

## Dequeue from an Empty Queue

Avoid:

```csharp
Queue<string> orders = new();

orders.Dequeue();
```

Prefer:

```csharp
if (orders.TryDequeue(out string? order))
{
    Console.WriteLine(order);
}
```

---

## Pop from an Empty Stack

Prefer:

```csharp
if (stack.TryPop(out string? item))
{
    Console.WriteLine(item);
}
```

---

# 21. Collection Selection Guide

| Requirement                  | Recommended Collection              |
| ---------------------------- | ----------------------------------- |
| Fixed-size data              | Array                               |
| General ordered data         | `List<T>`                           |
| Key-value lookup             | `Dictionary<TKey,TValue>`           |
| Unique values                | `HashSet<T>`                        |
| FIFO processing              | `Queue<T>`                          |
| LIFO processing              | `Stack<T>`                          |
| Node-based insertion/removal | `LinkedList<T>`                     |
| Unique sorted values         | `SortedSet<T>`                      |
| Sorted key-value data        | `SortedDictionary<TKey,TValue>`     |
| Read-only list               | `IReadOnlyList<T>`                  |
| Thread-safe key-value data   | `ConcurrentDictionary<TKey,TValue>` |

---

# 22. List vs Dictionary vs HashSet

### List

Use when:

```text
Order matters
Duplicates are allowed
Index-based access is useful
```

Example:

```csharp
List<string> foods = ["Pizza", "Pizza", "Burger"];
```

---

### Dictionary

Use when:

```text
You need Key → Value lookup
```

Example:

```csharp
Dictionary<int, string> users = new()
{
    [1] = "Sandip",
    [2] = "Ram"
};
```

---

### HashSet

Use when:

```text
Only unique values are needed
```

Example:

```csharp
HashSet<string> roles =
[
    "Admin",
    "Waiter",
    "Kitchen"
];
```

---

# 23. Important Collection Properties and Methods

### List

```csharp
Add()
AddRange()
Insert()
Remove()
RemoveAt()
Clear()
Contains()
IndexOf()
Find()
FindAll()
Exists()
Sort()
Reverse()
Count
```

### Dictionary

```csharp
Add()
Remove()
ContainsKey()
ContainsValue()
TryGetValue()
Clear()
Count
```

### HashSet

```csharp
Add()
Remove()
Contains()
UnionWith()
IntersectWith()
ExceptWith()
```

### Queue

```csharp
Enqueue()
Dequeue()
Peek()
TryDequeue()
TryPeek()
Count
```

### Stack

```csharp
Push()
Pop()
Peek()
TryPop()
TryPeek()
Count
```

---

# 24. Best Practices

### 1. Prefer generic collections

Use:

```csharp
List<string>
```

instead of:

```csharp
ArrayList
```

---

### 2. Choose the collection based on the requirement

Don't automatically use `List<T>` for everything.

For example:

```text
Key-value → Dictionary
Unique → HashSet
FIFO → Queue
LIFO → Stack
```

---

### 3. Use `TryGetValue()`

Instead of:

```csharp
users[id]
```

when the key may not exist:

```csharp
users.TryGetValue(id, out string? user);
```

---

### 4. Use interfaces where appropriate

Instead of tightly coupling a method to `List<T>`:

```csharp
void Print(IEnumerable<string> items)
{
    foreach (string item in items)
    {
        Console.WriteLine(item);
    }
}
```

---

### 5. Don't use LinkedList unnecessarily

For most normal application scenarios:

```csharp
List<T>
```

is a better default.

---

# 25. Collections Mental Model

Remember collections like this:

```text
C# Collections
│
├── Ordered
│   ├── Array
│   ├── List<T>
│   └── LinkedList<T>
│
├── Key → Value
│   ├── Dictionary<TKey,TValue>
│   └── SortedDictionary<TKey,TValue>
│
├── Unique
│   ├── HashSet<T>
│   └── SortedSet<T>
│
├── Processing
│   ├── Queue<T>  → FIFO
│   └── Stack<T>  → LIFO
│
└── Read-only
    ├── IReadOnlyList<T>
    └── IReadOnlyDictionary<TKey,TValue>
```

---

# 26. Quick Revision

### What is a collection?

A collection stores multiple values in a single object.

### What is `List<T>`?

A dynamic ordered collection that allows duplicates.

### What is `Dictionary<TKey,TValue>`?

A collection of key-value pairs.

### What is `HashSet<T>`?

A collection that stores unique values.

### What is `Queue<T>`?

A FIFO collection.

```text
First In → First Out
```

### What is `Stack<T>`?

A LIFO collection.

```text
Last In → First Out
```

### What is `LinkedList<T>`?

A collection made of linked nodes.

### What is `IEnumerable<T>`?

An interface representing something that can be iterated.

### Which collection should you use for unique values?

```csharp
HashSet<T>
```

### Which collection should you use for key-value data?

```csharp
Dictionary<TKey,TValue>
```

### Which collection should you use for FIFO?

```csharp
Queue<T>
```

### Which collection should you use for LIFO?

```csharp
Stack<T>
```

---

## Summary

The most important collections to remember are:

```text
List<T>       → General ordered data
Dictionary    → Key → Value
HashSet<T>    → Unique data
Queue<T>      → FIFO
Stack<T>      → LIFO
LinkedList<T> → Linked nodes
```

A good C# developer doesn't just know how to use collections—they know **which collection is appropriate for the problem**.
