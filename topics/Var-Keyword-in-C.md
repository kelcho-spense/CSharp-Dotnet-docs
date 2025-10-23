# Var Keyword in C#

* **Type Inference**: `var` is used when you want the compiler to automatically _**infer the type of a variable based on the value assigned to it**_.
* **Cannot Be Changed**: Once a variable is declared using var, its type is fixed and cannot be changed later.
* **Must Be Initialized**: When using var, the variable must be initialized at the time of declaration, as the compiler infers the type from the initialization.

Examples:

```C#
var age = 25;  // Inferred as int
var name = "John";  // Inferred as string
var list = new List<int>();  // Inferred as List<int>
```

> `var` is not a data type: It’s a keyword that tells the compiler to infer the type.

> `Type Safety`: The type is still statically determined at compile-time, so var is type-safe.