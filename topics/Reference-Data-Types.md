# Reference Data Types

![image_31.png](image_31.png)

### **Reference Data Types in C#**

In C#, **reference data types** are types that store references (or pointers) to the actual data rather than the data itself. When you assign a reference type variable to another variable, they both point to the same memory location, meaning they share the same data.

Reference types are stored in the **heap** memory, and variables of reference types hold the memory address (reference) of where the data is stored, not the actual data. This is different from **value types** (like `int`, `float`, etc.), which store the actual data in memory.

### **Types of Reference Data Types in C#**

1. **Objects**
2. **Strings**
3. **Classes**
4. **Interfaces**
5. **Arrays**

---

### **1. Objects in C#**

* **Object** is the base type for all data types in C#. Every type, whether it’s a built-in type or user-defined class, is derived from `System.Object`.
* **Object** can store any data type, including value types and reference types.

#### **Example:**

```C#
using System;

class Program
{
    static void Main()
    {
        // Storing a value type in an object
        int number = 10;
        object obj = number;  // Boxing: value type stored in object

        // Storing a reference type in an object
        string message = "Hello, C#!";
        obj = message;  // obj now holds a reference to the string

        Console.WriteLine(obj);  // Output: Hello, C#!
    }
}
```

**Explanation:**

* The `obj` variable can hold any type of data (here, an `int` and a `string`). It’s a generic reference type that can store any object, but it requires casting to work with the original type.

---

### **2. Strings in C#**

Strings in C# are reference types. They are immutable, which means once a string is created, it cannot be changed. Any operation that modifies a string will result in the creation of a new string.

#### **Example:**

```C#
using System;

class Program
{
    static void Main()
    {
        string str1 = "Hello";
        string str2 = str1;

        str1 = "Goodbye";  // Reassigning str1

        Console.WriteLine(str1);  // Output: Goodbye
        Console.WriteLine(str2);  // Output: Hello
    }
}
```

**Explanation:**

* Even though `str2` was assigned `str1`, changing `str1` does not affect `str2`. This is because strings are immutable, and `str2` holds a reference to the original string `"Hello"`, while `str1` now points to a new string `"Goodbye"`.

In the previous Chapter, we discussed the `char data` type where we are storing a single character in it. Now, if I try to add multiple characters to a char data type, then I will get a compile time error as shown in the below image.

![image_33.png](image_33.png)

As you can see, here we are getting the error Too many characters in character literal. This means you cannot store multiple characters in the character literal. If you want to store multiple characters, then you need to use the string data type in C# as shown in the below example,

```C#
string str = "ABC";

Console.WriteLine(str);

Console.WriteLine($"The type of str is : {typeof(string)}");

Console.ReadKey();
```

output

```Plain Text
ABC
The type of str is : System.String
```
---

### **3. Classes in C#**

A **class** is a reference type that defines the structure of objects. When you create an instance of a class, the object is stored in the heap, and the variable holds a reference to that object.

#### **Example:**

```C#
using System;

class Person
{
    public string Name;
    public int Age;
}

class Program
{
    static void Main()
    {
        // Creating an instance of the Person class
        Person person1 = new Person();
        person1.Name = "Alice";
        person1.Age = 25;

        // Assigning reference of person1 to person2
        Person person2 = person1;

        // Modifying the object through person2
        person2.Name = "Bob";

        // Both person1 and person2 refer to the same object
        Console.WriteLine(person1.Name);  // Output: Bob
        Console.WriteLine(person2.Name);  // Output: Bob
    }
}
```

**Explanation:**

* `person1` and `person2` both refer to the same object in memory. Modifying the object through `person2` also affects `person1`, as they both point to the same memory location.

---

### **4. Interfaces in C#**

An **interface** defines a contract that classes can implement. It is a reference type that specifies a set of methods or properties that a class must implement.

#### **Example:**

```C#
using System;

interface IDriveable
{
    void Drive();
}

class Car : IDriveable
{
    public void Drive()
    {
        Console.WriteLine("The car is driving.");
    }
}

class Program
{
    static void Main()
    {
        IDriveable myCar = new Car();
        myCar.Drive();  // Output: The car is driving.
    }
}
```

**Explanation:**

* The `Car` class implements the `IDriveable` interface, meaning it must provide an implementation for the `Drive` method. The `myCar` variable holds a reference to a `Car` object, and calling `Drive` on it triggers the `Drive` method of the `Car` class.

---

### **5. Arrays in C#**

Arrays in C# are also reference types. They store multiple elements of the same type and are allocated on the heap. When you assign an array to another variable, both variables point to the same array in memory.

#### **Example:**

```C#
using System;

class Program
{
    static void Main()
    {
        int[] arr1 = { 1, 2, 3, 4 };
        int[] arr2 = arr1;  // arr2 points to the same array as arr1

        arr2[0] = 10;  // Modify the first element of the array

        Console.WriteLine(arr1[0]);  // Output: 10 (arr1 and arr2 are pointing to the same array)
        Console.WriteLine(arr2[0]);  // Output: 10
    }
}
```

**Explanation:**

* Both `arr1` and `arr2` reference the same array. Changing an element in `arr2` also changes it in `arr1` because they both point to the same memory location.

---

### **Summary of Reference Data Types**

| Type          | Memory Location | Example Usage                                         |
| ------------- | --------------- | ----------------------------------------------------- |
| **Object**    | Heap            | Can store any data type (e.g., `int`, `string`, etc.) |
| **String**    | Heap            | Represents a sequence of characters, immutable        |
| **Class**     | Heap            | Defines objects with data and behavior                |
| **Interface** | Heap            | Defines a contract that classes can implement         |
| **Array**     | Heap            | A collection of elements of the same type             |

### **Key Points:**

> * **Heap Memory**: Reference types are stored in heap memory, and variables hold a reference to the memory location.
> * **Shared Data**: Assigning one reference type variable to another makes them both reference the same object in memory.
> * **Object Types**: Classes, interfaces, and arrays are examples of reference types in C#.
> * **Mutability**: Reference types allow the data they point to be modified, and changes are reflected across all references to that object.

---


