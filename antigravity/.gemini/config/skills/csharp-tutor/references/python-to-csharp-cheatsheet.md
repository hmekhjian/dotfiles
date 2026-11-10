# Python-to-C# Cheatsheet for Learners

Use this cheatsheet to quickly map Python mental models to C# syntax and conventions.

---

## 1. Basics & Output

| Feature | Python | C# | Notes |
| :--- | :--- | :--- | :--- |
| **Print to Console** | `print("Hello World")` | `Console.WriteLine("Hello World");` | Note the uppercase `Console`, semicolon `;`. |
| **String Interpolation** | `f"Hello, {name}!"` | `$"Hello, {name}!";` | Uses `$` instead of `f` prefix. |
| **Comments** | `# Comment`<br>`""" Multi-line """` | `// Comment`<br>`/* Multi-line */` | C# uses C-style comments. |
| **Entry Point** | `if __name__ == "__main__":` | Top-level statements (in modern C#) or `static void Main()` | In modern C# (.NET 6+), you can write code directly at the top of `Program.cs`. |

---

## 2. Variables & Types

| Concept | Python | C# | Key Difference |
| :--- | :--- | :--- | :--- |
| **Explicit Types** | `x: int = 10` | `int x = 10;` | C# enforces types at compile time. |
| **Type Inference** | `x = 10` (dynamic) | `var x = 10;` | In C#, `var` is still **statically typed**! The compiler infers `int`. You cannot assign a `string` to `x` later. |
| **None vs Null** | `val = None` | `string? val = null;` | `null` represents the absence of a value for reference types. |
| **Booleans** | `True`, `False` | `true`, `false` | Lowercase in C#. |
| **Constants** | `MY_CONST = 42` (convention) | `const int MyConst = 42;` | C# strictly enforces immutability at compile time. |

---

## 3. Collections & Data Structures

| Concept | Python | C# | Details |
| :--- | :--- | :--- | :--- |
| **List / Array** | `items = [1, 2, 3]`<br>`items.append(4)` | `List<int> items = new() { 1, 2, 3 };`<br>`items.Add(4);` | C# lists are typed (`List<T>`). Arrays `int[]` are fixed in length once created. |
| **Dictionary** | `data = {"a": 1, "b": 2}`<br>`val = data["a"]` | `Dictionary<string, int> data = new() { {"a", 1}, {"b", 2} };`<br>`int val = data["a"];` | Keys and values have strict types. |
| **Set** | `unique = {1, 2, 3}` | `HashSet<int> unique = new() { 1, 2, 3 };` | Fast unique item lookups. |
| **Tuples** | `point = (10, 20)`<br>`x, y = point` | `var point = (X: 10, Y: 20);`<br>`var (x, y) = point;` | Modern C# tuples support named elements: `point.X`. |

---

## 4. LINQ vs. Comprehensions (The Pythonista's Dream)

C#'s **LINQ** (Language Integrated Query) is the equivalent of Python's list comprehensions, `map`, and `filter`:

### Filtering (`filter` / `if` in comprehension)
- **Python**: `evens = [x for x in numbers if x % 2 == 0]`
- **C#**: `var evens = numbers.Where(x => x % 2 == 0).ToList();`

### Transforming (`map` / expression in comprehension)
- **Python**: `squares = [x * x for x in numbers]`
- **C#**: `var squares = numbers.Select(x => x * x).ToList();`

### Chaining Filter & Transform
- **Python**: `[x * 2 for x in numbers if x > 5]`
- **C#**: `numbers.Where(x => x > 5).Select(x => x * 2).ToList()`

### First / Find
- **Python**: `next((x for x in users if x.id == target_id), None)`
- **C#**: `users.FirstOrDefault(x => x.Id == targetId);`

### Any / All
- **Python**: `any(x > 10 for x in nums)` / `all(x > 0 for x in nums)`
- **C#**: `nums.Any(x => x > 10)` / `nums.All(x => x > 0)`

---

## 5. Control Flow

| Structure | Python | C# |
| :--- | :--- | :--- |
| **If / Else** | ```python
if x > 0:
    print("positive")
elif x == 0:
    print("zero")
else:
    print("negative")
``` | ```csharp
if (x > 0) {
    Console.WriteLine("positive");
} else if (x == 0) {
    Console.WriteLine("zero");
} else {
    Console.WriteLine("negative");
}
``` |
| **For Loop (Iteration)** | ```python
for item in items:
    print(item)
``` | ```csharp
foreach (var item in items) {
    Console.WriteLine(item);
}
``` |
| **Range Loop** | ```python
for i in range(5):
    print(i)
``` | ```csharp
for (int i = 0; i < 5; i++) {
    Console.WriteLine(i);
}
``` |

---

## 6. Functions / Methods

### Python
```python
def greet(name: str, greeting: str = "Hello") -> str:
    return f"{greeting}, {name}!"
```

### C#
```csharp
public string Greet(string name, string greeting = "Hello")
{
    return $"{greeting}, {name}!";
}

// Or as an expression-bodied method (arrow syntax):
public string Greet(string name, string greeting = "Hello") => $"{greeting}, {name}!";
```

### Lambdas / Anonymous Functions
- **Python**: `lambda x: x * 2`
- **C#**: `x => x * 2`

---

## 7. Classes & OOP

### Python
```python
class Dog:
    def __init__(self, name: str, age: int):
        self.name = name
        self.age = age

    def bark(self) -> None:
        print(f"{self.name} says woof!")
```

### C#
```csharp
public class Dog
{
    // Properties with automatic getters/setters
    public string Name { get; set; }
    public int Age { get; set; }

    // Constructor (same name as class, no return type)
    public Dog(string name, int age)
    {
        Name = name;
        Age = age;
    }

    public void Bark()
    {
        Console.WriteLine($"{Name} says woof!");
    }
}

// Modern C# 12+ Primary Constructor:
public class Dog(string name, int age)
{
    public string Name { get; set; } = name;
    public int Age { get; set; } = age;

    public void Bark() => Console.WriteLine($"{Name} says woof!");
}
```

---

## 8. Error Handling

### Python
```python
try:
    risky_operation()
except ValueError as e:
    print(f"Bad value: {e}")
except Exception as e:
    print(f"Error: {e}")
finally:
    clean_up()
```

### C#
```csharp
try
{
    RiskyOperation();
}
catch (ArgumentException e)
{
    Console.WriteLine($"Bad value: {e.Message}");
}
catch (Exception e)
{
    Console.WriteLine($"Error: {e.Message}");
}
finally
{
    CleanUp();
}
```
