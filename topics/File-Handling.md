# File Handling

## What is a File?
A file is a collection of data stored on a disk with a specific name, extension, and directory path. When you open File using C# for reading and writing purposes it becomes Stream.

## What is Stream?
`A stream is a sequence of bytes travelling from a source to a destination over a communication path`. There are two main streams: the Input Stream and the Output Stream. The input stream is used for reading data from the file (read operation) and the output stream is used for writing into the file (write operation).

* **Input Stream**: This Stream is used to read data from a file, which is known as a read operation.
* **Output Stream**: This Stream is used to write data into a file, which is known as a write operation.

## Why do I need to Learn File Handling in C#?
As a C# Programmer, we need to save information on a disk. We will not get a database everywhere to save the information and our project may require saving information in a TEXT file, DOC file, XLS file, PDF file, or any other file type. So, we must know the concept of saving data in a file i.e. How to Handle Files in C#.

Generally, we mostly perform three basic operations on a file: reading data from a file, writing data to a file and appending data to a file.

- Reading
- Writing
- Appending

## System.IO Namespace in C#
In C#, The System.IO namespace contains the required classes used to handle the input and output streams and provide information about file and directory structure

![image_178.png](image_178.png)

> The **FileInfo**, **DirectoryInfo**, and **DriveInfo** classes have instance methods. **File**, **Directory**, and `Path classes have static methods`. The following table describes commonly used classes in the System.IO namespace.

![image_179.png](image_179.png)