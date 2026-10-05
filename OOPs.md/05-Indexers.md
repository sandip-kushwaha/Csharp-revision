# C# Indexers

An **indexer** allows an object to be accessed like an array using an index.

Normally, we access an array like:

```csharp
numbers[0]
```

With an indexer, we can make our own class work in a similar way:

```csharp
students[0]
```

The main keyword used in an indexer is:

```csharp
this
```

---

# 1. What Is an Indexer?

An indexer allows an object to use square-bracket syntax:

```text
object[index]
```

Instead of:

```csharp
object.GetItem(index);
```

we can write:

```csharp
object[index];
```

### Basic idea

```text
Array
   ↓
numbers[0]

Custom class
   ↓
students[0]
```

An indexer provides a convenient way to access data stored inside an object.

---

# 2. Why Use Indexers?

Suppose we have a class:

```csharp
class StudentCollection
{
    private string[] students =
    {
        "Sandip",
        "Ram",
        "Shyam"
    };
}
```

Without an indexer, we might create a method:

```csharp
public string GetStudent(int index)
{
    return students[index];
}
```

Then:

```csharp
StudentCollection collection = new StudentCollection();

Console.WriteLine(collection.GetStudent(0));
```

With an indexer, we can write:

```csharp
Console.WriteLine(collection[0]);
```

This is cleaner and more natural when the object represents a collection.

---

# 3. Basic Indexer Syntax

The basic syntax is:

```csharp
public dataType this[int index]
{
    get
    {
        return value;
    }

    set
    {
        value = value;
    }
}
```

A practical example:

```csharp
class StudentCollection
{
    private string[] students =
    {
        "Sandip",
        "Ram",
        "Shyam"
    };

    public string this[int index]
    {
        get
        {
            return students[index];
        }

        set
        {
            students[index] = value;
        }
    }
}
```

Usage:

```csharp
StudentCollection students = new StudentCollection();

Console.WriteLine(students[0]);

students[1] = "Hari";

Console.WriteLine(students[1]);
```

Output:

```text
Sandip
Hari
```

---

# 4. Understanding `this[int index]`

This line:

```csharp
public string this[int index]
```

means:

```text
public
   ↓
Accessible from outside

string
   ↓
The indexer returns a string

this
   ↓
Represents the current object

[int index]
   ↓
Accepts an integer index
```

So:

```csharp
students[0]
```

calls the indexer's `get`.

And:

```csharp
students[0] = "Hari";
```

calls the indexer's `set`.

---

# 5. `get` Accessor

The `get` accessor returns a value.

```csharp
public string this[int index]
{
    get
    {
        return students[index];
    }
}
```

Usage:

```csharp
Console.WriteLine(students[0]);
```

Internally:

```text
students[0]
    ↓
indexer
    ↓
get
    ↓
students[index]
```

---

# 6. `set` Accessor

The `set` accessor changes a value.

```csharp
public string this[int index]
{
    set
    {
        students[index] = value;
    }
}
```

Usage:

```csharp
students[0] = "Sandip";
```

Here:

```text
"Sandip"
   ↓
value
   ↓
students[index]
```

---

# 7. Read-Only Indexer

An indexer can have only `get`.

```csharp
class ProductCollection
{
    private string[] products =
    {
        "Laptop",
        "Mouse",
        "Keyboard"
    };

    public string this[int index]
    {
        get
        {
            return products[index];
        }
    }
}
```

Usage:

```csharp
ProductCollection products = new ProductCollection();

Console.WriteLine(products[0]);
```

But this is not allowed:

```csharp
products[0] = "Monitor";
```

because there is no `set`.

---

# 8. Write-Only Indexer

An indexer can technically have only `set`.

```csharp
class DataStore
{
    private string[] data = new string[5];

    public string this[int index]
    {
        set
        {
            data[index] = value;
        }
    }
}
```

Usage:

```csharp
DataStore store = new DataStore();

store[0] = "Hello";
```

However, write-only indexers are uncommon because users normally expect to be able to read the value as well.

---

# 9. Auto-Property-Like Indexer

Modern C# allows expression-bodied indexers.

Instead of:

```csharp
public string this[int index]
{
    get
    {
        return students[index];
    }

    set
    {
        students[index] = value;
    }
}
```

You can write:

```csharp
public string this[int index]
{
    get => students[index];
    set => students[index] = value;
}
```

This is shorter and commonly used in modern C#.

---

# 10. Expression-Bodied Indexer

A read-only indexer can be even shorter:

```csharp
public string this[int index] => students[index];
```

Example:

```csharp
class StudentCollection
{
    private string[] students =
    {
        "Sandip",
        "Ram",
        "Shyam"
    };

    public string this[int index] => students[index];
}
```

Usage:

```csharp
Console.WriteLine(students[0]);
```

---

# 11. Indexer with Validation

An indexer can contain validation logic.

```csharp
class StudentCollection
{
    private string[] students = new string[5];

    public string this[int index]
    {
        get
        {
            if (index < 0 || index >= students.Length)
            {
                throw new IndexOutOfRangeException(
                    "Invalid student index."
                );
            }

            return students[index];
        }

        set
        {
            if (index < 0 || index >= students.Length)
            {
                throw new IndexOutOfRangeException(
                    "Invalid student index."
                );
            }

            students[index] = value;
        }
    }
}
```

This prevents invalid indexes.

---

# 12. Indexers and Arrays

Arrays already support indexing:

```csharp
int[] numbers = { 10, 20, 30 };

Console.WriteLine(numbers[0]);
```

Output:

```text
10
```

A custom indexer allows your own class to provide similar behavior.

```csharp
MyCollection collection = new MyCollection();

Console.WriteLine(collection[0]);
```

This makes custom collection-like classes easier to use.

---

# 13. Multiple Indexers

A class can have multiple indexers as long as their parameter lists are different.

For example:

```csharp
class DataCollection
{
    private string[] names =
    {
        "Sandip",
        "Ram",
        "Shyam"
    };

    private Dictionary<string, string> data = new();

    public string this[int index]
    {
        get => names[index];
        set => names[index] = value;
    }

    public string this[string key]
    {
        get => data[key];
        set => data[key] = value;
    }
}
```

Usage:

```csharp
DataCollection collection = new DataCollection();

Console.WriteLine(collection[0]);

collection["country"] = "Nepal";

Console.WriteLine(collection["country"]);
```

Output:

```text
Sandip
Nepal
```

The compiler chooses the correct indexer based on the argument type.

---

# 14. Indexer with Multiple Parameters

An indexer does not have to accept only one parameter.

Example:

```csharp
class Matrix
{
    private int[,] values = new int[3, 3];

    public int this[int row, int column]
    {
        get
        {
            return values[row, column];
        }

        set
        {
            values[row, column] = value;
        }
    }
}
```

Usage:

```csharp
Matrix matrix = new Matrix();

matrix[0, 0] = 100;
matrix[1, 2] = 200;

Console.WriteLine(matrix[0, 0]);
Console.WriteLine(matrix[1, 2]);
```

Output:

```text
100
200
```

This is useful for:

* Matrices
* Tables
* Grids
* Two-dimensional data

---

# 15. Matrix Example

A more complete example:

```csharp
class Matrix
{
    private int[,] values;

    public Matrix(int rows, int columns)
    {
        values = new int[rows, columns];
    }

    public int this[int row, int column]
    {
        get => values[row, column];

        set => values[row, column] = value;
    }
}
```

Usage:

```csharp
Matrix matrix = new Matrix(2, 2);

matrix[0, 0] = 10;
matrix[0, 1] = 20;
matrix[1, 0] = 30;
matrix[1, 1] = 40;

Console.WriteLine(matrix[1, 1]);
```

Output:

```text
40
```

---

# 16. String Indexer

We can create a class that exposes characters through an indexer.

```csharp
class Word
{
    private string value;

    public Word(string value)
    {
        this.value = value;
    }

    public char this[int index]
    {
        get => value[index];
    }
}
```

Usage:

```csharp
Word word = new Word("Sandip");

Console.WriteLine(word[0]);
Console.WriteLine(word[1]);
```

Output:

```text
S
a
```

---

# 17. Indexer with a Different Index Type

The index does not have to be an integer.

For example, a dictionary-like class can use `string`.

```csharp
class UserCollection
{
    private Dictionary<string, string> users = new();

    public string this[string username]
    {
        get => users[username];
        set => users[username] = value;
    }
}
```

Usage:

```csharp
UserCollection users = new UserCollection();

users["sandip"] = "Sandip Prasad Kushwaha";
users["ram"] = "Ram Sharma";

Console.WriteLine(users["sandip"]);
```

Output:

```text
Sandip Prasad Kushwaha
```

---

# 18. Indexer vs Property

Both properties and indexers use `get` and `set`, but they have different purposes.

### Property

```csharp
public string Name
{
    get;
    set;
}
```

Access:

```csharp
student.Name
```

### Indexer

```csharp
public string this[int index]
{
    get => students[index];
    set => students[index] = value;
}
```

Access:

```csharp
student[0]
```

### Comparison

| Feature       | Property      | Indexer         |
| ------------- | ------------- | --------------- |
| Access syntax | `object.Name` | `object[index]` |
| Represents    | A named value | Indexed data    |
| Parameters    | No            | Yes             |
| `get`         | Yes           | Yes             |
| `set`         | Yes           | Yes             |
| Keyword       | Property name | `this`          |

---

# 19. Indexer vs Method

Suppose we have:

```csharp
public string GetStudent(int index)
{
    return students[index];
}
```

We call:

```csharp
collection.GetStudent(0);
```

With an indexer:

```csharp
public string this[int index]
{
    get => students[index];
}
```

We call:

```csharp
collection[0];
```

### Which is easier?

For collection-like objects:

```csharp
collection[0]
```

is usually more natural.

For operations that perform an action, methods are usually better.

---

# 20. Indexer with `List<T>`

Indexers are frequently used with .NET collections.

For example:

```csharp
List<string> names = new()
{
    "Sandip",
    "Ram",
    "Shyam"
};

Console.WriteLine(names[0]);
```

`List<T>` itself provides an indexer.

Conceptually, it behaves like:

```csharp
list[index]
```

This is one reason indexers are important when learning C# collections.

---

# 21. Indexer with a Custom Collection

```csharp
class StudentCollection
{
    private List<string> students = new();

    public void Add(string name)
    {
        students.Add(name);
    }

    public string this[int index]
    {
        get => students[index];
        set => students[index] = value;
    }

    public int Count => students.Count;
}
```

Usage:

```csharp
StudentCollection students = new StudentCollection();

students.Add("Sandip");
students.Add("Ram");
students.Add("Shyam");

Console.WriteLine(students[0]);
Console.WriteLine(students[1]);

students[2] = "Hari";

Console.WriteLine(students[2]);
```

Output:

```text
Sandip
Ram
Hari
```

---

# 22. Hotel Management Example

Imagine a hotel has rooms.

```csharp
class Hotel
{
    private string[] rooms = new string[5];

    public string this[int roomNumber]
    {
        get
        {
            return rooms[roomNumber];
        }

        set
        {
            rooms[roomNumber] = value;
        }
    }
}
```

Usage:

```csharp
Hotel hotel = new Hotel();

hotel[0] = "Available";
hotel[1] = "Occupied";
hotel[2] = "Cleaning";

Console.WriteLine(hotel[1]);
```

Output:

```text
Occupied
```

The indexer makes the hotel object behave like a collection of rooms.

---

# 23. Better Hotel Example

Instead of storing only strings, we can store objects.

```csharp
class Room
{
    public int Number { get; set; }

    public string Status { get; set; }

    public Room(int number, string status)
    {
        Number = number;
        Status = status;
    }
}
```

Hotel:

```csharp
class Hotel
{
    private Room[] rooms;

    public Hotel(Room[] rooms)
    {
        this.rooms = rooms;
    }

    public Room this[int index]
    {
        get => rooms[index];
        set => rooms[index] = value;
    }
}
```

Usage:

```csharp
Room[] rooms =
{
    new Room(101, "Available"),
    new Room(102, "Occupied"),
    new Room(103, "Cleaning")
};

Hotel hotel = new Hotel(rooms);

Console.WriteLine(hotel[0].Number);
Console.WriteLine(hotel[0].Status);
```

Output:

```text
101
Available
```

---

# 24. Indexers and Encapsulation

An indexer can help keep internal data private.

Instead of exposing:

```csharp
public string[] Students;
```

we can keep the array private:

```csharp
private string[] students;
```

and expose controlled access:

```csharp
public string this[int index]
{
    get => students[index];
    set => students[index] = value;
}
```

This follows the principle of **encapsulation**.

The class controls how its internal data is accessed.

---

# 25. Read-Only Collection Access

Sometimes we want users to read values but not modify them.

```csharp
class StudentCollection
{
    private readonly string[] students =
    {
        "Sandip",
        "Ram",
        "Shyam"
    };

    public string this[int index] => students[index];
}
```

Users can:

```csharp
Console.WriteLine(collection[0]);
```

But cannot:

```csharp
collection[0] = "Hari";
```

This protects the internal data from modification.

---

# 26. Interface Indexers

Interfaces can define indexers.

```csharp
interface IStudentCollection
{
    string this[int index]
    {
        get;
        set;
    }
}
```

A class can implement it:

```csharp
class StudentCollection : IStudentCollection
{
    private string[] students = new string[5];

    public string this[int index]
    {
        get => students[index];
        set => students[index] = value;
    }
}
```

Usage:

```csharp
IStudentCollection students =
    new StudentCollection();

students[0] = "Sandip";

Console.WriteLine(students[0]);
```

This allows an interface to define indexed access as part of its contract.

---

# 27. Static Indexers

C# indexers are associated with instances.

You cannot normally define a static indexer.

This:

```csharp
public static string this[int index]
```

is not a valid normal C# indexer design.

If you need static indexed access, consider using:

* Static methods
* Static properties
* Static collections

For example:

```csharp
public static string GetStudent(int index)
{
    return students[index];
}
```

---

# 28. Indexer with Validation

A professional indexer can validate both index and value.

```csharp
class ProductCollection
{
    private string[] products = new string[5];

    public string this[int index]
    {
        get
        {
            if (index < 0 || index >= products.Length)
            {
                throw new IndexOutOfRangeException(
                    "Invalid product index."
                );
            }

            return products[index];
        }

        set
        {
            if (index < 0 || index >= products.Length)
            {
                throw new IndexOutOfRangeException(
                    "Invalid product index."
                );
            }

            if (string.IsNullOrWhiteSpace(value))
            {
                throw new ArgumentException(
                    "Product name cannot be empty."
                );
            }

            products[index] = value;
        }
    }
}
```

Usage:

```csharp
ProductCollection products = new ProductCollection();

products[0] = "Laptop";

Console.WriteLine(products[0]);
```

---

# 29. Important Indexer Rules

Remember these rules:

### Rule 1

An indexer uses:

```csharp
this
```

Example:

```csharp
public string this[int index]
```

### Rule 2

An indexer must have at least one parameter.

### Rule 3

An indexer can have:

```csharp
get
```

and/or:

```csharp
set
```

### Rule 4

A class can have multiple indexers if their parameter signatures differ.

### Rule 5

An indexer does not have a normal name.

You access it using:

```csharp
object[index]
```

### Rule 6

Indexers are useful for collection-like classes.

---

# 30. Common Mistakes

### Mistake 1: Forgetting `this`

Incorrect:

```csharp
public string [int index]
```

Correct:

```csharp
public string this[int index]
```

---

### Mistake 2: Invalid index handling

```csharp
return students[index];
```

If the index is invalid, an exception may occur.

Validate the index when appropriate.

---

### Mistake 3: Exposing internal arrays unnecessarily

Avoid:

```csharp
public string[] Students { get; set; }
```

when unrestricted modification is not intended.

Consider controlled access through an indexer or read-only collection.

---

### Mistake 4: Using indexers for everything

Indexers are best when an object naturally represents indexed data.

Don't use an indexer just because it is available.

---

# 31. When Should You Use Indexers?

Use an indexer when:

* Your class represents a collection.
* Users naturally think in terms of indexes.
* You want `object[index]` syntax.
* You need dictionary-like access.
* You need grid or matrix access.
* You want controlled indexed access to internal data.

Examples:

```text
StudentCollection[0]
ProductCollection[2]
Hotel[101]
Dictionary["username"]
Matrix[1, 2]
```

---

# 32. When Should You Use a Method Instead?

Use a method when the operation is an action rather than simple indexed access.

For example:

```csharp
hotel.GetAvailableRooms();
```

is better than:

```csharp
hotel["available"];
```

Similarly:

```csharp
order.Cancel();
```

is better than:

```csharp
order["cancel"];
```

Use syntax that communicates your intention clearly.

---

# 33. Complete Example

```csharp
using System;
using System.Collections.Generic;

class StudentCollection
{
    private readonly List<string> students = new();

    public void Add(string name)
    {
        if (string.IsNullOrWhiteSpace(name))
        {
            throw new ArgumentException(
                "Student name cannot be empty."
            );
        }

        students.Add(name);
    }

    public string this[int index]
    {
        get
        {
            if (index < 0 || index >= students.Count)
            {
                throw new IndexOutOfRangeException(
                    "Invalid student index."
                );
            }

            return students[index];
        }

        set
        {
            if (index < 0 || index >= students.Count)
            {
                throw new IndexOutOfRangeException(
                    "Invalid student index."
                );
            }

            if (string.IsNullOrWhiteSpace(value))
            {
                throw new ArgumentException(
                    "Student name cannot be empty."
                );
            }

            students[index] = value;
        }
    }

    public int Count => students.Count;
}

class Program
{
    static void Main()
    {
        StudentCollection students =
            new StudentCollection();

        students.Add("Sandip");
        students.Add("Ram");
        students.Add("Shyam");

        Console.WriteLine(students[0]);

        students[1] = "Hari";

        Console.WriteLine(students[1]);
        Console.WriteLine($"Total: {students.Count}");
    }
}
```

Output:

```text
Sandip
Hari
Total: 3
```

---

# 34. Indexer Mental Model

Think of an indexer as a bridge between an object and indexed data:

```text
             Custom Class
                  │
                  │
            ┌─────┴─────┐
            │  Indexer  │
            └─────┬─────┘
                  │
            this[index]
                  │
                  ↓
          Internal Collection
                  │
        ┌─────────┼─────────┐
        ↓         ↓         ↓
       [0]       [1]       [2]
```

Instead of:

```csharp
collection.GetItem(0);
```

you can write:

```csharp
collection[0];
```

---

# 35. Quick Revision

### What is an indexer?

An indexer allows an object to be accessed using:

```csharp
object[index]
```

### Main syntax

```csharp
public string this[int index]
{
    get => data[index];
    set => data[index] = value;
}
```

### Read-only indexer

```csharp
public string this[int index] => data[index];
```

### Multiple parameters

```csharp
public int this[int row, int column]
{
    get => matrix[row, column];
    set => matrix[row, column] = value;
}
```

### String indexer

```csharp
public string this[string key]
{
    get => data[key];
    set => data[key] = value;
}
```

### Property vs indexer

```text
Property:
object.Name

Indexer:
object[index]
```

---

# Summary

An indexer allows a class to behave like a collection.

The key syntax is:

```csharp
public string this[int index]
{
    get => data[index];
    set => data[index] = value;
}
```

Then the object can be accessed naturally:

```csharp
collection[0]
```

The most important keyword to remember is:

```csharp
this
```

### Core idea

> **An indexer lets your custom object use array-like `[]` syntax to provide controlled access to indexed data.**
