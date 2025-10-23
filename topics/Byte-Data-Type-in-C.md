# Byte Data Type

Is used to represent an 8-Bit unsigned integer.
> Unsigned means only positive values

```C#

const Byte MaxValue = 255;
const Byte MinValue = 0;

Console.WriteLine(MaxValue);
Console.WriteLine(MinValue);
```
Now, if you want to store both positive and negative values, then you need to use the signed byte data type i.e. SByte

## SByte Data Type in C#

SByte data type represents an 8-bit signed integer. Then what will be the maximum and minimum values? Remember when a data type is signed, then it can hold both positive and negative values. In that case, the maximum needs to be divided by two i.e. 256/2 which is 128. So, it will store 128 positive numbers and 128 negative numbers

```C#
const SByte MaxValue = 127;
const SByte MinValue = -128;

Console.WriteLine(MaxValue);
Console.WriteLine(MinValue);
```