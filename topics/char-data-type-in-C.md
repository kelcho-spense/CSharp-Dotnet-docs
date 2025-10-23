# Char Data Type

Char is a 2-Byte length data type that can contain Unicode data. 
**What is Unicode?**

Unicode is a standard for character encoding and decoding for computers. We can use various Unicode encodings formats such as 
- UTF-8(8-bit)
- UTF-16(16-bit), 

and so on. As per the definition of char, it represents a character as a UTF-16 code unit. UTF-16 means 16-bit length which is nothing but 2-Bytes.

As char is 2-Byte length, so it will contain 216 numbers i.e. 65536. So, the minimum number is 0 and the maximum number is 65535. For a better understanding, please have a look at the below example.

```C#
char ch = 'B';
Console.WriteLine($"Char: {ch}");
Console.WriteLine($"Equivalent Number: {(byte)ch}");
Console.WriteLine($"Char Minimum: {(int)char.MinValue} and Maximum: {(int)char.MaxValue}");
Console.WriteLine($"Char Size: {sizeof(char)} Byte");
Console.ReadKey();
```
output will be 
```plain text
Char: B
Equivalent Number: 66
Char Minimum: 0 and Maximum: 65535
Char Size: 2 Byte
```