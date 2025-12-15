# User Defined Data Type

![image_31.png](image_31.png)

These types are defined by the programmer. Two common user-defined value data types are:

* **Enumeration** (enum)
* **Structure** (struct)

## **Enums in C#**

### **What is an Enum?**

An **Enum** (short for "enumeration") is a special value type in C# that allows you to define a set of named constants. These constants are typically used to represent a collection of related values in a more readable and maintainable manner.

Enums are ideal for use cases where you need to define a collection of options or states that are represented by a numeric value, but you want to use descriptive names instead of raw numbers.

### **Declaration of Enum**

An enum is declared using the `enum` keyword followed by the name of the enum and the list of constant values it will hold. By default, the underlying type of the enum is `int`, but you can specify other integral types such as `byte`, `short`, `long`, etc.

### **Syntax:** {id="syntax_1"}

```C#
enum EnumName : EnumType
{
    Value1,
    Value2,
    Value3,
    ...
}
```

* `EnumName`: The name of the enumeration.
* `EnumType`: The underlying data type (optional; defaults to `int`).
* `Value1, Value2, Value3, ...`: The named constants within the enum.

### **Example of Enum:**

```C#
// Defining an Enum for Days of the Week
enum Day { Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday }

class Program
{
    static void Main(string[] args)
    {
        // Assigning an Enum value
        Day today = Day.Monday;

        // Using Enum in a switch case
        switch (today)
        {
            case Day.Monday:
                Console.WriteLine("Start of the work week.");
                break;
            case Day.Friday:
                Console.WriteLine("End of the work week.");
                break;
            default:
                Console.WriteLine("It's a regular day.");
                break;
        }

        // Getting the numeric value of the Enum
        Console.WriteLine($"Numeric value of {today}: {(int)today}");
    }
}
```

**Output:**

```plaintext
Start of the work week.
Numeric value of Monday: 0
```

### **Enum Values and Numeric Representation**

By default, the first value in an enum is assigned `0`, and each subsequent value is incremented by 1. However, you can explicitly assign values to the enum members.

### **Example with Explicit Values:**

```C#
enum Day { Monday = 1, Tuesday = 2, Wednesday = 3, Thursday = 4, Friday = 5 }

class Program
{
    static void Main(string[] args)
    {
        Day today = Day.Wednesday;
        Console.WriteLine($"Numeric value of {today}: {(int)today}");
    }
}
```

**Output:**

```plaintext
Numeric value of Wednesday: 3
```

### **Why Use Enums?**

* **Improves Code Readability**: Enums allow you to use descriptive names instead of arbitrary numbers.
* **Prevents Invalid Values**: Enums limit the possible values to a predefined set of constants, preventing the use of invalid data.

---

## **Structs in C#**

### **What is a Struct?**

A **Struct** in C# is a value type that allows you to define a complex data type. Structs are similar to classes but have a few key differences. They are often used when you need to group related data together, and you want that data to be passed around by value (copying the entire struct).

Unlike classes, which are reference types, structs are **value types**, meaning they are stored on the stack and passed by value. Structs can contain fields, methods, properties, and constructors.

### **Declaration of Struct**

A struct is defined using the `struct` keyword, and it can contain members such as fields, methods, properties, and constructors.

### **Syntax:**

```C#
struct StructName
{
    public DataType Field1;
    public DataType Field2;
    ...
    
    // Constructor
    public StructName(DataType value1, DataType value2)
    {
        Field1 = value1;
        Field2 = value2;
    }
}
```

* `StructName`: The name of the struct.
* `Field1`, `Field2`, etc.: The fields (data members) of the struct.
* The constructor (optional) initializes the fields when a new instance of the struct is created.

### **Example of Struct:**

```C#
// Defining a Struct to represent a Point in 2D space
struct Point
{
    public int X;
    public int Y;

    // Constructor for initialization
    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }

    // Method to display point coordinates
    public void DisplayPoint()
    {
        Console.WriteLine($"Point coordinates: ({X}, {Y})");
    }
}

class Program
{
    static void Main(string[] args)
    {
        // Creating an instance of the struct
        Point p = new Point(10, 20);
        p.DisplayPoint();  // Output: Point coordinates: (10, 20)
    }
}
```

**Output:**

```plaintext
Point coordinates: (10, 20)
```

### **Struct vs Class**

* **Value Type**: Structs are value types, meaning when you assign a struct variable to another, a copy of the data is made.
* **Reference Type**: Classes are reference types, meaning they store references to the data and not the actual data itself.

### **Why Use Structs?**

* **Performance**: Structs are more memory-efficient when dealing with small, simple data types that don't require inheritance or complex functionality.
* **No Inheritance**: Unlike classes, structs cannot be inherited or derive from another struct or class.

---

## **Differences Between Enums and Structs**

| Feature          | **Enum**                                  | **Struct**                                  |
| ---------------- | ----------------------------------------- | ------------------------------------------- |
| **Type**         | Special data type for named constants     | User-defined value type                     |
| **Purpose**      | To define a set of related constants      | To group data into a single entity          |
| **Memory**       | Stores the numeric value of each constant | Stores values for each member (fields)      |
| **Default Type** | `int` (default underlying type)           | Can contain multiple fields of any type     |
| **Inheritance**  | Cannot inherit from other enums or types  | Cannot inherit from another struct or class |
| **Use Case**     | Days of the week, months of the year      | Point, Rectangle, Customer information      |

---

> * **Enums** are used to define a set of related constants with readable names instead of numbers, improving code clarity and maintainability.
> * **Structs** are lightweight, value types used to group related data together, often used for simple data structures like points, rectangles, or any data that requires passing by value.

```C#
// Enumeration (User-defined value type)
enum Day { Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday }

// Structure (User-defined value type)
struct Point
{
    public int X;
    public int Y;
}

class Program
{
    static void Main(string[] args)
    {
        // Using Enum
        Day today = Day.Monday;
        Console.WriteLine("Today is: " + today);

        // Using Structure
        Point p = new Point();
        p.X = 10;
        p.Y = 20;
        Console.WriteLine($"Point: ({p.X}, {p.Y})");
    }
}

```
Output

```Plain Text
Today is: Monday
Point: (10, 20)

```