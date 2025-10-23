# Value Data Types in C#

Value data types directly hold their data in memory. When you assign a value to a variable of a value type, a copy of the actual data is stored in the variable. There are two subcategories of value data types:

1. Predefined Data Types: These are built-in types provided by C#:

    - Integer: Represents whole numbers (e.g., `int`, `long`).
    - Boolean: Represents true or false values (`bool`).    
    - Float: Represents floating-point numbers with single precision (`float`).    
    - Char: Represents a single character (`char`).

2. User-Defined Data Types: These types are defined by the programmer. Two common user-defined value data types are:
    - Enumeration (`enum`): Used to define a set of named integral constants. For example, representing the days of the week.
    - Structure (`struct`): A value type that can contain multiple variables of different data types. For example, creating a structure to represent a point with x and y coordinates.
## How Data is Represented in a Computer?

Please have a look at the below diagram

![image_32.png](image_32.png)

In essence, the computer stores and processes data using binary (0s and 1s), but we, as developers, often work with higher-level formats like decimal for easier understanding.
In C#, we typically work with decimal numbers instead of binary. So, we convert the binary number to its decimal equivalent. Internally, the computer then converts this decimal back into binary (byte) format to process and store the data.