# C# Collections

Collections are used to **store and manage multiple values/objects**.

Main collections:

```text
List<T>
Dictionary<TKey,TValue>
HashSet<T>
Queue<T>
Stack<T>
LinkedList<T>
```

Namespace:

```csharp
using System.Collections.Generic;
```

---

## 1. List<T>

Stores an ordered collection of items.

```csharp
List<string> foods =
[
    "Momo",
    "Pizza",
    "Burger"
];
```

Add:

```csharp
foods.Add("Pasta");
```

Remove:

```csharp
foods.Remove("Pizza");
```

Access:

```csharp
Console.WriteLine(foods[0]);
```

Count:

```csharp
Console.WriteLine(foods.Count);
```

Useful methods:

```text
Add()
AddRange()
Remove()
RemoveAt()
Contains()
Clear()
Sort()
```

---

## 2. Dictionary<TKey, TValue>

Stores **key-value pairs**.

```csharp
Dictionary<int, string> users = new()
{
    [1] = "Sandip",
    [2] = "Ram"
};
```

Access:

```csharp
Console.WriteLine(users[1]);
```

Add:

```csharp
users.Add(3, "Hari");
```

Check key:

```csharp
if (users.ContainsKey(1))
{
    Console.WriteLine(users[1]);
}
```

Safer lookup:

```csharp
users.TryGetValue(1, out string? name);
```

---

## 3. HashSet<T>

Stores **unique values**.

```csharp
HashSet<int> numbers =
[
    10,
    20,
    20,
    30
];
```

Result:

```text
10
20
30
```

Add:

```csharp
numbers.Add(40);
```

Check:

```csharp
numbers.Contains(20);
```

---

## 4. Queue<T>

Follows **FIFO**:

```text
First In → First Out
```

```csharp
Queue<string> queue = new();

queue.Enqueue("Order 1");
queue.Enqueue("Order 2");
queue.Enqueue("Order 3");
```

Remove:

```csharp
string order = queue.Dequeue();
```

View first:

```csharp
string first = queue.Peek();
```

Useful for:

* Order processing
* Print queues
* Task queues

---

## 5. Stack<T>

Follows **LIFO**:

```text
Last In → First Out
```

```csharp
Stack<string> stack = new();

stack.Push("Page 1");
stack.Push("Page 2");
stack.Push("Page 3");
```

Remove:

```csharp
string page = stack.Pop();
```

View top:

```csharp
string top = stack.Peek();
```

Useful for:

* Undo/Redo
* Browser history
* Backtracking

---

## 6. LinkedList<T>

Stores elements as linked nodes.

```csharp
LinkedList<string> list = new();

list.AddLast("A");
list.AddLast("B");
list.AddFirst("Start");
```

Useful when frequent insertion/removal at known nodes is required.

For most normal application lists, prefer `List<T>`.

---

# 7. Iterating Collections

### foreach

```csharp
foreach (string food in foods)
{
    Console.WriteLine(food);
}
```

### for

```csharp
for (int i = 0; i < foods.Count; i++)
{
    Console.WriteLine(foods[i]);
}
```

---

# 8. Collection Interfaces

Common interfaces:

```text
IEnumerable<T> → Can be enumerated
ICollection<T> → Collection operations
IList<T>       → List/index operations
IDictionary<K,V> → Key-value collection
ISet<T>        → Set operations
```

Example:

```csharp
IEnumerable<string> foods =
    new List<string>
    {
        "Momo",
        "Pizza"
    };
```

---

# 9. Generic Collections

Prefer:

```csharp
List<int> numbers = new();
```

instead of old non-generic:

```csharp
ArrayList numbers = new();
```

Generic collections provide:

* Type safety
* Better readability
* Less casting
* Better performance for value types

---

# 10. LINQ with Collections

Collections work with LINQ.

```csharp
List<int> numbers =
[
    10, 20, 30, 40, 50
];

var result = numbers
    .Where(x => x > 20)
    .OrderBy(x => x)
    .ToList();
```

---

# 11. Collection Expressions

Modern C# supports:

```csharp
List<int> numbers =
[
    10,
    20,
    30
];
```

This is shorter than:

```csharp
List<int> numbers = new()
{
    10,
    20,
    30
};
```

---

# 12. Hotel Example

### Food List

```csharp
List<string> foods =
[
    "Momo",
    "Pizza",
    "Burger"
];
```

### Table Dictionary

```csharp
Dictionary<string, int> tables = new()
{
    ["T-01"] = 4,
    ["T-02"] = 6
};
```

### Order Queue

```csharp
Queue<string> orders = new();

orders.Enqueue("Order #101");
orders.Enqueue("Order #102");
```

Kitchen processes:

```csharp
string order = orders.Dequeue();
```

---

# 13. Which Collection Should I Use?

| Requirement        | Collection        |
| ------------------ | ----------------- |
| Ordered items      | `List<T>`         |
| Key + value        | `Dictionary<K,V>` |
| Unique values      | `HashSet<T>`      |
| First-in-first-out | `Queue<T>`        |
| Last-in-first-out  | `Stack<T>`        |
| Linked nodes       | `LinkedList<T>`   |

---

# 14. Quick Revision

```text
List<T>
→ Ordered collection
→ Index based
```

```text
Dictionary<K,V>
→ Key + Value
→ Fast key lookup
```

```text
HashSet<T>
→ Unique values
```

```text
Queue<T>
→ FIFO
→ Enqueue / Dequeue
```

```text
Stack<T>
→ LIFO
→ Push / Pop
```

```text
LinkedList<T>
→ Linked nodes
```

---

# 15. Important Methods

### List

```text
Add()
Remove()
RemoveAt()
Contains()
Sort()
Clear()
```

### Dictionary

```text
Add()
Remove()
ContainsKey()
TryGetValue()
```

### HashSet

```text
Add()
Remove()
Contains()
UnionWith()
IntersectWith()
```

### Queue

```text
Enqueue()
Dequeue()
Peek()
```

### Stack

```text
Push()
Pop()
Peek()
```

---

# Final Checklist

* [ ] `List<T>`
* [ ] `Dictionary<TKey,TValue>`
* [ ] `HashSet<T>`
* [ ] `Queue<T>`
* [ ] `Stack<T>`
* [ ] `LinkedList<T>`
* [ ] Generic collections
* [ ] Collection interfaces
* [ ] `foreach`
* [ ] LINQ with collections
* [ ] Collection expressions
* [ ] Choose the correct collection

**Remember:**

```text
List       → Ordered
Dictionary → Key/Value
HashSet    → Unique
Queue      → FIFO
Stack      → LIFO
```
