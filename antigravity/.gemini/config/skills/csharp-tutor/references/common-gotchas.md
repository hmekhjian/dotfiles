# Common C# Gotchas for Python Developers

Here are the most frequent pitfalls and differences Python developers encounter when writing or reading C#.

---

### 1. Integer Division Truncation 🔢
- **Python**: `5 / 2` produces `2.5` (float). To get integer division in Python, you explicitly use `5 // 2`.
- **C#**: `5 / 2` produces `2`! If both operands are integers, C# performs integer division and truncates the decimal part.
- **The Fix**: Make at least one number a floating-point literal: `5.0 / 2` or `(double)5 / 2`.

---

### 2. Single Quotes vs. Double Quotes 🔤
- **Python**: `'hello'` and `"hello"` are identical strings.
- **C#**:
  - Double quotes `"hello"` = `string` (text).
  - Single quotes `'a'` = `char` (a single 16-bit character).
  - Writing `'hello'` in C# will fail to compile with an error.

---

### 3. `var` is NOT `dynamic` 🔒
- **Python**: Variables are dynamically typed. You can assign an integer to `x`, then later assign a string to `x`.
- **C#**: `var x = 10;` does **not** mean dynamic. It is compile-time **type inference**. The C# compiler figures out that `x` is an `int`, and `x` stays an `int` forever.
- Doing `x = "hello";` later will cause a compiler error.

---

### 4. Naming Conventions (PascalCase vs snake_case) 🏷️
- **Python**: Functions, methods, and variables use `snake_case` (`calculate_total`, `user_id`). Classes use `PascalCase`.
- **C#**:
  - Methods, Properties, Classes, Namespaces use **PascalCase** (`CalculateTotal()`, `UserId`, `CustomerOrder`).
  - Local variables and method parameters use **camelCase** (`calculateTotal`, `userId`).
  - Private backing fields often start with an underscore (`_userName`).

---

### 5. Modifying Collections During Iteration 💥
- **Python**: Mutating a list during a `for x in my_list:` loop is bad practice and can cause skipped elements.
- **C#**: Doing this in a `foreach (var x in myList)` loop immediately throws an **`InvalidOperationException: Collection was modified`**.
- **The Fix**: Iterate over a snapshot/copy, or use LINQ:
  ```csharp
  foreach (var item in items.ToList()) { /* safe to mutate original items */ }
  ```

---

### 6. Dealing with `null` (NullReferenceException) 🚫
- **Python**: Trying to access an attribute on `None` throws `AttributeError: 'NoneType' object has no attribute '...'`.
- **C#**: Trying to access a member on `null` throws `NullReferenceException` (the famous NRE).
- **Modern C# Safety Tools**:
  - **Nullable type marker (`?`)**: `string? name = null;` tells the compiler this can be null.
  - **Null-conditional operator (`?.`)**: `user?.Address?.City` returns `null` safely if `user` or `Address` is null (like `getattr` chains).
  - **Null-coalescing operator (`??`)**: `string displayName = user.Name ?? "Anonymous";` (like `user.name or "Anonymous"` in Python).

---

### 7. Methods Need Explicit Return Types (`void` if nothing returned) ↩️
- **Python**: A function without a `return` statement silently returns `None`.
- **C#**: Every method must declare its return type up front. If it returns nothing, you must specify `void`:
  ```csharp
  public void LogMessage(string message)
  {
      Console.WriteLine(message);
  }
  ```

---

### 8. Value Types vs. Reference Types (Simple Intuition) 📦
You do **not** need to worry about memory layout or CPU caches, but understand this basic behavior:
- **Value Types** (`int`, `double`, `bool`, `DateTime`, `struct`): When assigned to a new variable or passed to a function, they copy the **value itself** (like copying text on a notepad). Changing one copy does not affect the other.
- **Reference Types** (`class`, `string`, `List<T>`, arrays): Variables hold a reference to the object (just like Python variables). If two variables refer to the same instance, modifying the object through one is reflected in the other.
