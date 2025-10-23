# Integers Data types (Numbers without Decimal)

> Integers are Numbers without Decimal

In this category, the .NET Framework provides three kinds of integer data types. They are as follows:

1. **16-Bit Signed Numeric**: Example: `Int16`
2. **32-Bit Signed Numeric**: Example: `Int32`
3. **64-Bit Signed Numeric**: Example: `Int64`

As the above data types are signed, they can store both positive and negative numbers. The size of the number they can hold varies based on the data type.

---

## 16-Bit Signed Numeric (Int16)

The `Int16` data type is a 16-bit signed integer. It can store 65,536 values. Since it is signed, half of these values are negative, and half are positive.

* **Positive Range**: 0 to 32,767
* **Negative Range**: -1 to -32,768

```C#
const Int16 MaxInt16Value = 32767;
const Int16 MinInt16Value = -32768;

Console.WriteLine(MaxInt16Value);
Console.WriteLine(MinInt16Value);
Console.WriteLine($"the type of Int16: {typeof(Int16)}");
```

**Output:**

```Plain Text
32767
-32768
the type of Int16 : System.Int16
```

---

## 32-Bit Signed Numeric (Int32)

The `Int32` data type is a 32-bit signed integer, which allows it to store a wider range of values than `Int16`. It can represent both positive and negative numbers.

* **Positive Range**: 0 to 2,147,483,647
* **Negative Range**: -1 to -2,147,483,648

```C#
const Int32 MaxInt32Value = 2147483647;
const Int32 MinInt32Value = -2147483648;

Console.WriteLine(MaxInt32Value);
Console.WriteLine(MinInt32Value);
Console.WriteLine($"the type of Int32: {typeof(Int32)}");
```

**Output:**

```Plain Text
2147483647
-2147483648
the type of Int32 : System.Int32
```

---

## 64-Bit Signed Numeric (Int64)

The `Int64` data type is a 64-bit signed integer. This data type has an even larger range, making it suitable for situations where a large number must be stored.

* **Positive Range**: 0 to 9,223,372,036,854,775,807
* **Negative Range**: -1 to -9,223,372,036,854,775,808

```C#
const Int64 MaxInt64Value = 9223372036854775807;
const Int64 MinInt64Value = -9223372036854775808;

Console.WriteLine(MaxInt64Value);
Console.WriteLine(MinInt64Value);
Console.WriteLine($"the type of Int64: {typeof(Int64)}");
```

**Output:**

```Plain Text
9223372036854775807
-9223372036854775808
the type of Int64 : System.Int64
```

---

## Summary of Integer Data Types in C\#

| Data Type | Size   | Range (Positive)               | Range (Negative)                 |
| --------- | ------ | ------------------------------ | -------------------------------- |
| **Int16** | 16-bit | 0 to 32,767                    | -1 to -32,768                    |
| **Int32** | 32-bit | 0 to 2,147,483,647             | -1 to -2,147,483,648             |
| **Int64** | 64-bit | 0 to 9,223,372,036,854,775,807 | -1 to -9,223,372,036,854,775,808 |

### Notes:

* **Choosing the Right Data Type**: When working with integers in C#, choose the appropriate data type based on the range of numbers you need to store. For most applications, `Int32` or `Int64` will suffice, but for very large values, `Int64` is typically used.
* **Signed vs. Unsigned**: All of these integer types are signed, meaning they can store both positive and negative values. If you only need to store positive values, you can use unsigned versions like `UInt16`, `UInt32`, and `UInt64`.

---

This is the updated documentation focusing on the three main integer types in C#. Let me know if you'd like further details!
