# Numeric Numbers with Decimal (Single, Double, Decimal)

In C#, numeric values with decimals are represented by three main data types:

1. **Single** (single-precision floating-point number)
2. **Double** (double-precision floating-point number)
3. **Decimal** (represents a decimal floating-point number)

These data types differ primarily in the amount of memory they consume and their precision:

* **Single**: Takes 4 bytes of memory
* **Double**: Takes 8 bytes of memory
* **Decimal**: Takes 16 bytes of memory

## Why Add `f` and `m` Suffixes?

In C#, when you write a decimal value, the compiler may default to interpreting it as a `double`. To avoid confusion and to specify the exact type, you can use suffixes:

* **`f`**: This suffix is used to indicate that the number is a `Single` (float) type. Without this suffix, the compiler will assume the value is a `double` by default.

* **`m`**: This suffix is used to indicate that the number is a `Decimal` type. Without this suffix, the compiler assumes the number is a `double`.

### Example Code:

```c#
Single a = 1.123f;  // Single (float) value with f suffix
Double b = 1.456;   // Double value (default, no suffix)
Decimal c = 1.789M; // Decimal value with m suffix
            
Console.WriteLine($"Single Size: {sizeof(Single)} Byte");
Console.WriteLine($"Single Min Value: {Single.MinValue} and Max Value: {Single.MaxValue}");
Console.WriteLine($"Double Size: {sizeof(Double)} Byte");
Console.WriteLine($"Double Min Value: {Double.MinValue} and Max Value: {Double.MaxValue}");
Console.WriteLine($"Decimal Size: {sizeof(Decimal)} Byte");
Console.WriteLine($"Decimal Min Value: {Decimal.MinValue} and Max Value: {Decimal.MaxValue}");
Console.ReadKey();
```

**Output:**

```Plain Text
Single Size: 4 Byte
Single Min Value: -3.402823E+38 and Max Value: 3.402823E+38
Double Size: 8 Byte
Double Min Value: -1.7976931348623157E+308 and Max Value: 1.7976931348623157E+308
Decimal Size: 16 Byte
Decimal Min Value: -79228162514264337593543950335 and Max Value: 79228162514264337593543950335
```

### Explanation of the Code:

* `Single a = 1.123f;`: The `f` suffix specifies that `a` is a `Single` (float).
* `Double b = 1.456;`: No suffix is used, so this is interpreted as a `Double` by default.
* `Decimal c = 1.789M;`: The `M` suffix specifies that `c` is a `Decimal`.

### Short-Hand Names:

Instead of using `Single`, `Double`, and `Decimal`, you can use the short-hand names for these types in C#:

* **`float`** for `Single`
* **`double`** for `Double`
* **`decimal`** for `Decimal`

Here's how you can use the shorthand names:

```C#
float a = 1.123f;  // float (Single) type
double b = 1.456;  // double (Double) type
decimal c = 1.789M; // decimal (Decimal) type
            
Console.WriteLine($"float Size: {sizeof(float)} Byte");
Console.WriteLine($"float Min Value: {float.MinValue} and Max Value: {float.MaxValue}");
Console.WriteLine($"double Size: {sizeof(double)} Byte");
Console.WriteLine($"double Min Value: {double.MinValue} and Max Value: {double.MaxValue}");
Console.WriteLine($"decimal Size: {sizeof(decimal)} Byte");
Console.WriteLine($"decimal Min Value: {decimal.MinValue} and Max Value: {decimal.MaxValue}");
Console.ReadKey();

```

**Output** will be the same as the earlier example, but using short-hand names for types.

---

## Summary:

| Data Type   | Size     | Range (Positive)                   | Range (Negative)                                     | Default Suffix |
| ----------- | -------- | ---------------------------------- | ---------------------------------------------------- | -------------- |
| **Single**  | 4 bytes  | 0 to 3.402823E+38                  | -3.402823E+38 to -1.401298E-45                       | `f`            |
| **Double**  | 8 bytes  | 0 to 1.7976931348623157E+308       | -1.7976931348623157E+308 to -4.9406564584124654E-324 | No suffix      |
| **Decimal** | 16 bytes | 0 to 79228162514264337593543950335 | -79228162514264337593543950335                       | `m`            |


> * **Use `f` for Single**: When specifying a `Single` (float) value.
> * **Use `m` for Decimal**: When specifying a `Decimal` value.
> * **No suffix means Double**: If no suffix is provided, the value defaults to `double`.
> * **Choosing the Right Type**: Use `Single` for less memory usage and moderate precision, `Double` for larger numbers and higher precision, and `Decimal` when you need very high precision (such as for financial calculations).

---
