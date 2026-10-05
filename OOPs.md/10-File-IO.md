# C# File I/O

File I/O means **Input/Output with files and directories**.

It is used to:

* Create files
* Read files
* Write files
* Append data
* Copy/move files
* Delete files
* Create directories

```csharp
using System.IO;
```

---

## 1. Write to a File

```csharp
File.WriteAllText("notes.txt", "Hello C#");
```

> Replaces existing content.

---

## 2. Read a File

```csharp
string text = File.ReadAllText("notes.txt");

Console.WriteLine(text);
```

---

## 3. Append to a File

```csharp
File.AppendAllText(
    "notes.txt",
    "\nNew line"
);
```

Adds data without removing existing content.

---

## 4. Write Multiple Lines

```csharp
string[] foods =
[
    "Pizza",
    "Burger",
    "Momo"
];

File.WriteAllLines("foods.txt", foods);
```

Read:

```csharp
string[] foods = File.ReadAllLines("foods.txt");

foreach (string food in foods)
{
    Console.WriteLine(food);
}
```

---

## 5. Check File Exists

```csharp
if (File.Exists("notes.txt"))
{
    Console.WriteLine("File exists");
}
```

---

## 6. Copy, Move, Delete

### Copy

```csharp
File.Copy("notes.txt", "backup.txt");
```

### Move

```csharp
File.Move("notes.txt", "backup/notes.txt");
```

### Delete

```csharp
File.Delete("notes.txt");
```

---

## 7. Directory

Create:

```csharp
Directory.CreateDirectory("data");
```

Check:

```csharp
Directory.Exists("data");
```

Get files:

```csharp
string[] files = Directory.GetFiles("data");
```

Delete:

```csharp
Directory.Delete("data", true);
```

---

## 8. Path

Use `Path.Combine()` instead of manually joining paths.

```csharp
string path = Path.Combine(
    "data",
    "notes.txt"
);
```

Useful methods:

```csharp
Path.GetFileName(path);
Path.GetExtension(path);
Path.GetDirectoryName(path);
Path.GetFullPath(path);
```

---

## 9. FileInfo

`FileInfo` provides information about a file.

```csharp
FileInfo file = new("notes.txt");

Console.WriteLine(file.Name);
Console.WriteLine(file.Length);
Console.WriteLine(file.Extension);
```

Common properties:

```text
Name
Length
FullName
Extension
CreationTime
LastWriteTime
```

---

## 10. StreamReader

Used to read text.

```csharp
using StreamReader reader = new("notes.txt");

string text = reader.ReadToEnd();

Console.WriteLine(text);
```

Read line by line:

```csharp
using StreamReader reader = new("notes.txt");

string? line;

while ((line = reader.ReadLine()) != null)
{
    Console.WriteLine(line);
}
```

---

## 11. StreamWriter

Used to write text.

```csharp
using StreamWriter writer = new("notes.txt");

writer.WriteLine("C#");
writer.WriteLine("ASP.NET Core");
```

Append:

```csharp
using StreamWriter writer =
    new("notes.txt", append: true);

writer.WriteLine("New line");
```

---

## 12. FileStream

Used for lower-level file operations and binary data.

```csharp
using FileStream stream =
    new("data.txt", FileMode.OpenOrCreate);
```

Common modes:

```text
FileMode.Create
FileMode.Open
FileMode.OpenOrCreate
FileMode.Append
FileMode.Truncate
```

---

## 13. JSON File I/O

```csharp
using System.Text.Json;

class User
{
    public string Name { get; set; } = "";
    public int Age { get; set; }
}
```

Write JSON:

```csharp
User user = new()
{
    Name = "Sandip",
    Age = 22
};

string json = JsonSerializer.Serialize(user);

File.WriteAllText("user.json", json);
```

Read JSON:

```csharp
string json = File.ReadAllText("user.json");

User? user =
    JsonSerializer.Deserialize<User>(json);
```

---

## 14. Async File I/O

Read asynchronously:

```csharp
string text =
    await File.ReadAllTextAsync("notes.txt");
```

Write asynchronously:

```csharp
await File.WriteAllTextAsync(
    "notes.txt",
    "Hello C#"
);
```

---

## 15. Exception Handling

File operations can fail.

```csharp
try
{
    string text =
        File.ReadAllText("notes.txt");
}
catch (FileNotFoundException)
{
    Console.WriteLine("File not found.");
}
catch (UnauthorizedAccessException)
{
    Console.WriteLine("Access denied.");
}
catch (IOException ex)
{
    Console.WriteLine(ex.Message);
}
```

---

## 16. Important Classes

| Class           | Purpose                  |
| --------------- | ------------------------ |
| `File`          | Simple file operations   |
| `FileInfo`      | File information         |
| `Directory`     | Directory operations     |
| `DirectoryInfo` | Directory information    |
| `Path`          | File path handling       |
| `FileStream`    | Stream/binary operations |
| `StreamReader`  | Read text                |
| `StreamWriter`  | Write text               |

---

## 17. Quick Revision

```text
File.ReadAllText()     → Read text
File.WriteAllText()    → Write text
File.ReadAllLines()    → Read lines
File.WriteAllLines()   → Write lines
File.AppendAllText()   → Append
File.Exists()          → Check file
File.Copy()            → Copy
File.Move()            → Move
File.Delete()          → Delete

Directory.CreateDirectory() → Create folder
Directory.GetFiles()         → Get files

Path.Combine()          → Build path

StreamReader            → Read stream
StreamWriter            → Write stream
FileStream              → Low-level stream
```

### Mental Model

```text
File
├── Read
├── Write
├── Append
├── Copy
├── Move
└── Delete

Directory
├── Create
├── Get files
└── Delete

Path
└── Manage paths

Streams
├── FileStream
├── StreamReader
└── StreamWriter
```


