# C# Asynchronous Programming

Asynchronous programming allows a program to **start a time-consuming operation without blocking the current thread**.

It is commonly used for:

* API requests
* Database operations
* File operations
* Network operations
* HTTP requests
* Background tasks

Main keywords:

```text
async
await
Task
Task<T>
```

---

# 1. Synchronous vs Asynchronous

### Synchronous

Tasks execute one after another.

```text
Task A → Task B → Task C
```

If Task A takes 5 seconds, Task B waits.

### Asynchronous

A program can start an operation and continue other work while waiting.

```text
Start Task A
     ↓
await
     ↓
Other work can continue
     ↓
Task A completes
```

---

# 2. Task

`Task` represents an asynchronous operation.

```csharp
Task task = DoSomethingAsync();
```

For a result:

```csharp
Task<int> task = CalculateAsync();
```

---

# 3. async Keyword

The `async` keyword allows a method to use `await`.

```csharp
async Task DoSomethingAsync()
{
    await Task.Delay(1000);

    Console.WriteLine("Done");
}
```

---

# 4. await Keyword

`await` waits for an asynchronous operation to complete **without blocking the thread in the usual async flow**.

```csharp
async Task ExampleAsync()
{
    Console.WriteLine("Start");

    await Task.Delay(2000);

    Console.WriteLine("End");
}
```

Output:

```text
Start
End
```

The method pauses at `await` until the task completes.

---

# 5. async Method Return Types

Common return types:

```csharp
async Task MethodAsync()
{
}
```

With a result:

```csharp
async Task<int> GetNumberAsync()
{
    return 100;
}
```

Application-level event handlers may use:

```csharp
async void Button_Click(object sender, EventArgs e)
{
}
```

Generally, prefer `Task` over `async void`.

---

# 6. Task.Delay()

`Task.Delay()` is useful for demonstrating asynchronous waiting.

```csharp
async Task WaitAsync()
{
    await Task.Delay(3000);

    Console.WriteLine("3 seconds completed");
}
```

It does **not** block the thread like `Thread.Sleep()` does.

---

# 7. Thread.Sleep() vs Task.Delay()

### Thread.Sleep()

```csharp
Thread.Sleep(3000);
```

Blocks the current thread.

### Task.Delay()

```csharp
await Task.Delay(3000);
```

Asynchronously waits.

For asynchronous applications, prefer:

```csharp
await Task.Delay(...);
```

when an asynchronous delay is what you need.

---

# 8. Returning a Value

```csharp
async Task<int> GetPriceAsync()
{
    await Task.Delay(1000);

    return 500;
}
```

Call it:

```csharp
int price = await GetPriceAsync();

Console.WriteLine(price);
```

---

# 9. Exception Handling

Use normal `try-catch` with async methods.

```csharp
async Task GetDataAsync()
{
    try
    {
        await Task.Delay(1000);

        throw new Exception("Something went wrong");
    }
    catch (Exception ex)
    {
        Console.WriteLine(ex.Message);
    }
}
```

---

# 10. Multiple Async Operations

Suppose two independent operations are required.

Instead of:

```csharp
var users = await GetUsersAsync();
var foods = await GetFoodsAsync();
```

you can start both first:

```csharp
Task<User[]> usersTask = GetUsersAsync();
Task<Food[]> foodsTask = GetFoodsAsync();

var users = await usersTask;
var foods = await foodsTask;
```

Or commonly:

```csharp
var usersTask = GetUsersAsync();
var foodsTask = GetFoodsAsync();

await Task.WhenAll(usersTask, foodsTask);
```

`Task.WhenAll()` waits for multiple tasks.

---

# 11. Task.WhenAll()

```csharp
Task task1 = SendEmailAsync();
Task task2 = SaveDataAsync();
Task task3 = LogActivityAsync();

await Task.WhenAll(task1, task2, task3);
```

Useful when operations are independent.

Concept:

```text
Task 1 ────────┐
Task 2 ────────┼──→ WhenAll → Continue
Task 3 ────────┘
```

---

# 12. Task.WhenAny()

`Task.WhenAny()` completes when **any one** of the supplied tasks completes.

```csharp
Task task1 = DownloadFileAsync();
Task task2 = DownloadImageAsync();

Task completed = await Task.WhenAny(task1, task2);
```

Useful when you only need the first completed operation.

---

# 13. Async HTTP Request

In ASP.NET Core or other .NET applications:

```csharp
using HttpClient client = new();

string result =
    await client.GetStringAsync("https://example.com");
```

The HTTP operation is asynchronous.

---

# 14. Async File I/O

```csharp
string content =
    await File.ReadAllTextAsync("data.txt");

Console.WriteLine(content);
```

Writing:

```csharp
await File.WriteAllTextAsync(
    "data.txt",
    "Hello Sandip"
);
```

---

# 15. Async Database Operations

With Entity Framework Core:

```csharp
var foods =
    await dbContext.Foods
        .Where(x => x.IsAvailable)
        .ToListAsync();
```

Other common methods:

```text
ToListAsync()
FirstOrDefaultAsync()
SingleOrDefaultAsync()
CountAsync()
AnyAsync()
SaveChangesAsync()
```

---

# 16. ASP.NET Core Example

Controller/service method:

```csharp
public async Task<List<Food>> GetFoodsAsync()
{
    return await dbContext.Foods
        .Where(x => x.IsAvailable)
        .ToListAsync();
}
```

Controller:

```csharp
[HttpGet]
public async Task<IActionResult> GetFoods()
{
    var foods = await foodService.GetFoodsAsync();

    return Ok(foods);
}
```

Flow:

```text
HTTP Request
     ↓
Controller
     ↓
Service
     ↓
EF Core
     ↓
Database
     ↓
await
     ↓
Response
```

---

# 17. CancellationToken

`CancellationToken` allows an asynchronous operation to be cancelled.

```csharp
async Task DownloadAsync(CancellationToken token)
{
    await Task.Delay(5000, token);
}
```

Call:

```csharp
CancellationTokenSource cts = new();

await DownloadAsync(cts.Token);
```

Cancel:

```csharp
cts.Cancel();
```

This is useful for:

* HTTP requests
* Long-running operations
* Background services
* File processing

---

# 18. ConfigureAwait

You may see:

```csharp
await SomeOperationAsync()
    .ConfigureAwait(false);
```

It controls synchronization-context behavior.

In modern ASP.NET Core applications, you usually don't need to add `ConfigureAwait(false)` everywhere.

For basic C# learning, focus on:

```text
async
await
Task
Task<T>
Task.WhenAll()
CancellationToken
```

---

# 19. async Does Not Automatically Mean Parallel

Important:

```csharp
async
```

does **not** mean the code automatically runs on another thread.

Asynchronous programming is mainly about **not blocking while waiting for asynchronous operations**.

Parallel programming is about **doing multiple pieces of work at the same time**.

They are related but different concepts.

---

# 20. Task.Run()

`Task.Run()` can run CPU-bound work on a thread-pool thread.

Example:

```csharp
int result = await Task.Run(() =>
{
    return CalculateSomething();
});
```

Don't use `Task.Run()` simply to wrap every asynchronous I/O operation.

For example, prefer:

```csharp
await httpClient.GetAsync(url);
```

instead of:

```csharp
await Task.Run(() =>
    httpClient.GetAsync(url)
);
```

---

# 21. Async Naming Convention

Asynchronous methods commonly end with `Async`.

Good:

```csharp
GetUsersAsync()
SaveFoodAsync()
DeleteOrderAsync()
SendEmailAsync()
```

Example:

```csharp
public async Task<List<User>> GetUsersAsync()
{
    return await dbContext.Users.ToListAsync();
}
```

---

# 22. Common Mistake: .Result

Avoid:

```csharp
var result = GetDataAsync().Result;
```

Prefer:

```csharp
var result = await GetDataAsync();
```

Similarly, avoid unnecessary:

```csharp
GetDataAsync().Wait();
```

Prefer:

```csharp
await GetDataAsync();
```

Blocking async code can cause performance problems and, in some environments, deadlocks.

---

# 23. Common Mistake: async void

Avoid:

```csharp
async void GetDataAsync()
{
}
```

Prefer:

```csharp
async Task GetDataAsync()
{
}
```

`async void` is mainly appropriate for event handlers.

---

# 24. Hotel Management Example

Suppose the hotel system needs to load foods from the database:

```csharp
public async Task<List<Food>> GetAvailableFoodsAsync()
{
    return await dbContext.Foods
        .Where(x => x.IsAvailable && x.IsActive)
        .OrderBy(x => x.Name)
        .ToListAsync();
}
```

Controller:

```csharp
[HttpGet("available")]
public async Task<IActionResult> GetAvailableFoods()
{
    var foods =
        await foodService.GetAvailableFoodsAsync();

    return Ok(foods);
}
```

The request doesn't need to block a thread while the database is processing the query.

---

# 25. Complete Example

```csharp
using System;
using System.Threading.Tasks;

class Program
{
    static async Task Main()
    {
        Console.WriteLine("Starting...");

        int result = await CalculateAsync();

        Console.WriteLine($"Result: {result}");
        Console.WriteLine("Finished.");
    }

    static async Task<int> CalculateAsync()
    {
        await Task.Delay(1000);

        return 100 + 200;
    }
}
```

Output:

```text
Starting...
Result: 300
Finished.
```

---

# 26. Async Mental Model

Remember:

```text
async
  ↓
Method can await asynchronous work
  ↓
await
  ↓
Wait without blocking the async flow
  ↓
Task completes
  ↓
Continue
```

Common pattern:

```csharp
public async Task<T> MethodAsync()
{
    T result = await SomeOperationAsync();

    return result;
}
```

---

# 27. Quick Revision

| Concept               | Meaning                                    |
| --------------------- | ------------------------------------------ |
| `async`               | Marks an asynchronous method               |
| `await`               | Asynchronously waits for a task            |
| `Task`                | Represents async work                      |
| `Task<T>`             | Async work returning `T`                   |
| `Task.Delay()`        | Asynchronous delay                         |
| `Task.WhenAll()`      | Wait for all tasks                         |
| `Task.WhenAny()`      | Wait for any task                          |
| `CancellationToken`   | Cancel async work                          |
| `Task.Run()`          | Run suitable CPU-bound work on thread pool |
| `async void`          | Mainly for event handlers                  |
| `.Result` / `.Wait()` | Avoid unnecessary blocking                 |

---

# 28. Best Practices

* Prefer `async`/`await` for asynchronous I/O.
* Return `Task` or `Task<T>`.
* Use `Async` suffix for async methods.
* Avoid `.Result` and `.Wait()`.
* Avoid unnecessary `Task.Run()`.
* Use `Task.WhenAll()` for independent operations.
* Use `CancellationToken` for cancellable long-running work.
* Handle exceptions with `try-catch` where appropriate.
* Use async database methods such as `ToListAsync()` and `SaveChangesAsync()`.
* Keep the async flow asynchronous instead of mixing blocking and non-blocking code.

---

## Summary

The core idea is:

```text
async → await → Task
```

For modern C# backend development:

```csharp
public async Task<List<Food>> GetFoodsAsync()
{
    return await dbContext.Foods
        .Where(x => x.IsAvailable)
        .ToListAsync();
}
```

**Async programming helps applications remain responsive and efficiently handle operations that spend time waiting, especially I/O such as databases, files, and network requests.**
