# C# Generics

**Generics** allow you to write reusable code that works with different data types while maintaining **type safety**.

Generic example:

```csharp id="zj4q1k"
List<int> numbers = new();
List<string> names = new();
```

---

## 1. Why Generics?

Without generics:

```csharp id="2v2q0y"
object value = 100;
int number = (int)value;
```

With generics:

```csharp id="z8a5qv"
List<int> numbers = new();
```

Benefits:

* Type safety
* Code reusability
* Less casting
* Better readability

---

# 2. Generic Class

```csharp id="z19v5r"
class Box<T>
{
    public T Value { get; set; }
}
```

Usage:

```csharp id="7p4h9v"
Box<int> numberBox = new()
{
    Value = 100
};

Box<string> textBox = new()
{
    Value = "Hello"
};
```

`T` represents a type.

---

# 3. Generic Method

```csharp id="d4t8wr"
static void Print<T>(T value)
{
    Console.WriteLine(value);
}
```

Usage:

```csharp id="oqc5h5"
Print<int>(100);
Print<string>("Sandip");
```

C# can infer the type:

```csharp id="y9x3ag"
Print(100);
Print("Sandip");
```

---

# 4. Multiple Type Parameters

```csharp id="grv6os"
class Pair<TKey, TValue>
{
    public TKey Key { get; set; }
    public TValue Value { get; set; }
}
```

Usage:

```csharp id="2pkcbb"
Pair<int, string> user = new()
{
    Key = 1,
    Value = "Sandip"
};
```

---

# 5. Generic Interface

```csharp id="5b5ksj"
interface IRepository<T>
{
    void Add(T item);
    T? GetById(int id);
}
```

Implementation:

```csharp id="6c0x7r"
class FoodRepository : IRepository<Food>
{
    public void Add(Food item)
    {
        // Add food
    }

    public Food? GetById(int id)
    {
        return null;
    }
}
```

---

# 6. Generic Constraints

Constraints control which types can be used.

### `where T : class`

```csharp id="g1x5zq"
class Repository<T> where T : class
{
}
```

Only reference types.

### `where T : struct`

```csharp id="34o9pp"
class Storage<T> where T : struct
{
}
```

Only value types.

### `where T : new()`

Requires a public parameterless constructor.

```csharp id="c4f5l6"
class Factory<T> where T : new()
{
    public T Create()
    {
        return new T();
    }
}
```

### Interface constraint

```csharp id="q8j8ai"
class Service<T> where T : IDisposable
{
}
```

---

# 7. Multiple Constraints

```csharp id="6qzqv1"
class Repository<T>
    where T : class, IEntity, new()
{
}
```

Meaning:

```text id="pp2aqw"
T must be:
✓ Reference type
✓ IEntity
✓ Have parameterless constructor
```

---

# 8. Generic Collections

The most common generics are collections:

```csharp id="z5l0cs"
List<int> numbers = new();

Dictionary<int, string> users = new();

HashSet<string> names = new();

Queue<Order> orders = new();
```

---

# 9. Generic Delegate

### Action

```csharp id="q1s8k4"
Action<string> print =
    message => Console.WriteLine(message);
```

### Func

```csharp id="1v6gcf"
Func<int, int, int> add =
    (a, b) => a + b;
```

### Predicate

```csharp id="s4f5nw"
Predicate<int> isEven =
    x => x % 2 == 0;
```

---

# 10. Generic Extension Method

```csharp id="4c7c2x"
static class Extensions
{
    public static bool IsNull<T>(this T value)
    {
        return value == null;
    }
}
```

Usage:

```csharp id="kw0dyt"
string? name = null;

Console.WriteLine(name.IsNull());
```

---

# 11. Generic Repository Pattern

Common in backend applications:

```csharp id="c4n8t5"
interface IRepository<T>
{
    Task<List<T>> GetAllAsync();
    Task<T?> GetByIdAsync(int id);
    Task AddAsync(T entity);
}
```

Then:

```csharp id="n7f4di"
IRepository<Food> foodRepository;
IRepository<Order> orderRepository;
IRepository<User> userRepository;
```

The same interface structure can work with different entity types.

---

# 12. `default(T)`

Returns the default value of a type.

```csharp id="l1r6kf"
static T GetDefault<T>()
{
    return default!;
}
```

Examples:

```text id="x7osbq"
int       → 0
bool      → false
string    → null
reference → null
```

---

# 13. Generics vs Object

Without generics:

```csharp id="uj1fwy"
object value = 100;

int number = (int)value;
```

With generics:

```csharp id="zq5x2y"
List<int> numbers = new();
```

Generics provide compile-time type safety.

---

# 14. Hotel Example

Generic response model:

```csharp id="5emh3v"
class ApiResponse<T>
{
    public bool Success { get; set; }
    public T? Data { get; set; }
    public string Message { get; set; } = "";
}
```

Food response:

```csharp id="j6n0vo"
ApiResponse<List<Food>> response = new()
{
    Success = true,
    Data = foods,
    Message = "Foods loaded"
};
```

Order response:

```csharp id="v7h3gp"
ApiResponse<Order> response = new()
{
    Success = true,
    Data = order,
    Message = "Order created"
};
```

Same generic class, different types.

---

# 15. Generic Method with Constraint

```csharp id="x5l7uj"
static void Display<T>(T item)
    where T : class
{
    Console.WriteLine(item);
}
```

Only reference types can be passed.

---

# 16. Generic Inheritance

```csharp id="q8x1k6"
class Animal
{
}

class Repository<T>
{
}

class AnimalRepository : Repository<Animal>
{
}
```

---

# 17. Naming Conventions

Common generic type names:

```text id="j6k7e1"
T      → Type
TKey   → Key type
TValue → Value type
TItem  → Item type
TEntity → Entity type
```

Example:

```csharp id="5t0g8f"
class Repository<TEntity>
{
}
```

---

# 18. Quick Revision

| Concept           | Meaning                             |
| ----------------- | ----------------------------------- |
| `T`               | Generic type                        |
| Generic Class     | Class working with different types  |
| Generic Method    | Method working with different types |
| Generic Interface | Reusable typed interface            |
| Constraint        | Restricts allowed types             |
| `class`           | Reference type constraint           |
| `struct`          | Value type constraint               |
| `new()`           | Requires parameterless constructor  |
| `default(T)`      | Default value of T                  |
| `List<T>`         | Generic collection                  |

---

# 19. Mental Model

```text id="k4y6pf"
Generics
   ↓
Reusable Code
   ↓
Different Types
   ↓
Type Safety
   ↓
Less Casting
```

Example:

```csharp id="7j4g6x"
List<int>
List<string>
List<Food>
List<Order>
```

Same `List<T>` concept, different types.

---

# Final Checklist

* [ ] Generic classes
* [ ] Generic methods
* [ ] Generic interfaces
* [ ] Multiple type parameters
* [ ] Generic collections
* [ ] Generic constraints
* [ ] `class` constraint
* [ ] `struct` constraint
* [ ] `new()` constraint
* [ ] Interface constraints
* [ ] `Action`
* [ ] `Func`
* [ ] `Predicate`
* [ ] `default(T)`
* [ ] Generic repository
* [ ] Generic API response
* [ ] Generic naming conventions

**Remember:**

> **Generics = Write once, use with different types, while keeping type safety.**
