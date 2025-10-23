# Type Casting in C#

Here is the updated **Type Casting in C#** documentation with the requested code blocks removed:

---

### **Type Casting in C#**

In simple terms, **Type Casting** or **Type Conversion** in C# is the process of converting a value from one data type to another. Type conversion is possible only when the source and target data types are compatible with each other; otherwise, it results in a **compile-time error** saying `cannot implicitly convert one type to another`.

Let us better understand this concept with an example. When we declare an integer variable, we cannot implicitly assign a string value to it. For instance:

```csharp
int a;
a = "Hello";  // Error CS0029: cannot implicitly convert type string to int
```

In this case, the compiler throws an error because a string cannot be automatically converted to an integer.

---

### **Types of Type Casting in C#**

Type casting in C# is of two types:

1. **Implicit Type Casting** (also called **Automatic Type Conversion**)
2. **Explicit Type Casting**

---

#### **1. Implicit Type Casting (Automatic Type Conversion)**

**Implicit Type Casting** is automatically done by the C# compiler when converting from a smaller data type to a larger one. This type of conversion is safe and doesn’t lose any data. The compiler performs the conversion without requiring the developer’s intervention.

In implicit conversion, the conversion is performed **automatically** because the target data type has enough capacity to store the source data type's value.

**Example:**

```csharp
int num = 10;  // int is 32-bit
double result = num;  // Implicit conversion from int to double

Console.WriteLine("Value of result: " + result);  // Output: 10.0
```

**Explanation:**

* In the above example, the `int` value `num` is automatically converted to `double` without any explicit cast.
* The **int** type (32-bit) can be safely converted to **double** (64-bit) because `double` has more capacity, so there is no data loss.

---

#### **2. Explicit Type Casting (Manual Type Conversion)**

**Explicit Type Casting** occurs when we manually convert one data type to another using casting operators. This is necessary when converting from a larger data type to a smaller one, as data loss or truncation may occur. Since this type of casting could potentially lose data, the compiler does not allow it implicitly and expects the developer to handle the conversion explicitly.

**Syntax for Explicit Casting:**

```csharp
(targetType) value;
```

**Example:**

```csharp
double num = 10.75;  // double is 64-bit
int result = (int)num;  // Explicit conversion from double to int

Console.WriteLine("Value of result: " + result);  // Output: 10
```

**Explanation:**

* The `double` value `num` is explicitly converted to `int` using the cast `(int)`. This operation discards the decimal portion, resulting in `10`.

---

### **Difference Between Implicit and Explicit Type Casting**

| Aspect                   | **Implicit Type Casting**                | **Explicit Type Casting**                                             |
| ------------------------ | ---------------------------------------- | --------------------------------------------------------------------- |
| **When it occurs**       | Automatically performed by the compiler. | Performed manually by the developer.                                  |
| **Conversion direction** | Smaller data types to larger data types. | Larger data types to smaller data types.                              |
| **Risk of data loss**    | No risk of data loss or truncation.      | May result in data loss or truncation (e.g., from `double` to `int`). |
| **Syntax**               | No special syntax required.              | Requires a cast operator (e.g., `(int)`, `(float)`, etc.).            |
| **Example**              | `int num = 10; double result = num;`     | `double num = 10.75; int result = (int)num;`                          |

---

### **Type Casting with Different Data Types**

#### **1. Implicit Conversion Example (int to long)**

```csharp
int a = 10;
long b = a;  // Implicit conversion from int to long

Console.WriteLine("Value of b: " + b);  // Output: 10
```

**Explanation:**

* Converting from `int` to `long` is an **implicit** conversion because `long` (64-bit) has more capacity than `int` (32-bit).

---

#### **2. Explicit Conversion Example (double to int)**

```csharp
double a = 9.99;
int b = (int)a;  // Explicit conversion from double to int

Console.WriteLine("Value of b: " + b);  // Output: 9
```

**Explanation:**

* The `double` value `a` is explicitly converted to `int` using the cast `(int)`. This operation discards the decimal portion, resulting in `9`.

---

#### **3. Using `Convert` Class for Explicit Type Casting**

Another way to explicitly convert types is by using the `Convert` class, which handles many conversions.

```csharp
string str = "123";
int num = Convert.ToInt32(str);  // Convert string to int

Console.WriteLine("Converted number: " + num);  // Output: 123
```

**Explanation:**

* The `Convert.ToInt32()` method is used to convert a string value to an integer. If the string is not a valid number, this method will throw an exception.

---

### **Common Errors in Type Casting**

1. **Invalid Type Casting**: Trying to convert incompatible types (e.g., `string` to `int` directly).

```csharp
string text = "Hello";
int number = (int)text;  // Error: Cannot convert string to int directly.
```

2. **Data Loss**: Converting a larger data type to a smaller one without explicitly handling the potential loss of data.

```csharp
double largeValue = 9999999.99;
int smallValue = (int)largeValue;  // Data loss due to truncation.
Console.WriteLine(smallValue);  // Output: 9999999
```

---

### **Table of Compatible Data Types for Type Casting in C#**

| **Source Type** | **Target Type** | **Conversion Type** | **Description**                                               |
| --------------- | --------------- | ------------------- | ------------------------------------------------------------- |
| **int**         | long            | Implicit            | `int` can be implicitly converted to `long`                   |
| **int**         | double          | Implicit            | `int` can be implicitly converted to `double`                 |
| **int**         | float           | Implicit            | `int` can be implicitly converted to `float`                  |
| **long**        | float           | Explicit            | `long` needs explicit casting to `float`                      |
| **long**        | double          | Implicit            | `long` can be implicitly converted to `double`                |
| **float**       | double          | Implicit            | `float` can be implicitly converted to `double`               |
| **double**      | float           | Explicit            | `double` needs explicit casting to `float`                    |
| **double**      | long            | Explicit            | `double` needs explicit casting to `long`                     |
| **char**        | int             | Implicit            | `char` can be implicitly converted to `int`                   |
| **char**        | long            | Implicit            | `char` can be implicitly converted to `long`                  |
| **char**        | double          | Implicit            | `char` can be implicitly converted to `double`                |
| **byte**        | short           | Implicit            | `byte` can be implicitly converted to `short`                 |
| **byte**        | int             | Implicit            | `byte` can be implicitly converted to `int`                   |
| **byte**        | long            | Implicit            | `byte` can be implicitly converted to `long`                  |
| **byte**        | float           | Implicit            | `byte` can be implicitly converted to `float`                 |
| **byte**        | double          | Implicit            | `byte` can be implicitly converted to `double`                |
| **short**       | int             | Implicit            | `short` can be implicitly converted to `int`                  |
| **short**       | long            | Implicit            | `short` can be implicitly converted to `long`                 |
| **short**       | float           | Implicit            | `short` can be implicitly converted to `float`                |
| **short**       | double          | Implicit            | `short` can be implicitly converted to `double`               |
| **decimal**     | double          | Explicit            | `decimal` needs explicit casting to `double`                  |
| **decimal**     | float           | Explicit            | `decimal` needs explicit casting to `float`                   |
| **bool**        | (other types)   | Not allowed         | `bool` cannot be implicitly or explicitly cast to other types |

---


### **Conclusion**

* **Implicit Type Casting**: Done automatically by the compiler when converting from a smaller data type to a larger one. It is safe and causes no data loss.
* **Explicit Type Casting**: Done manually by the developer when converting from a larger data type to a smaller one. This type of casting can lead to data loss or truncation if not handled properly.

Understanding the difference between implicit and explicit type casting is crucial in C# to avoid errors and ensure proper data manipulation.
