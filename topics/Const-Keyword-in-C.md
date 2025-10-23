# Const Keyword in C#

* **Constant Value**: `const` is used to declare a constant value that cannot be changed after it is initialized.
* **Compile-time Constant**: The value of a `const` is determined at compile-time, and it must be assigned a value when declared.
* **Type**: A `const` can be of any data type (int, string, etc.), and its value must be assigned at the time of declaration.
* **Examples**:

  ```c#
  const double Pi = 3.14;
  const int MaxValue = 100;
  const string Greeting = "Hello, World!";
  ```

### **Key Points**:

* **`const` is implicitly static**: All constants are static by default, so you don’t need to explicitly specify `static` when declaring them.
* **Cannot Be Modified**: Once a `const` value is set, it cannot be changed throughout the program.
* **Use Case**: Useful for values that are known at compile-time and should not change, such as mathematical constants or configuration values.

---

### **Comparison**:

* **`var`**: Used for type inference, and the type is still determined at compile time. The value can change.
* **`const`**: Used to declare constants with a fixed value that cannot change after initialization.

Let me know if you'd like more details or examples!
