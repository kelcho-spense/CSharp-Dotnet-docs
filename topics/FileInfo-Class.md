# FileInfo Class

The FileInfo class gives detailed information about a file and provides instance-based methods (unlike File, which is static).

```C#
using System;
using System.IO;

namespace FileHandlingDemo
{
    class FileInfoExample
    {
        static void Main()
        {
            string path = "example4.txt";
            File.WriteAllText(path, "FileInfo class demo text!");

            FileInfo fileInfo = new FileInfo(path);

            Console.WriteLine($"File Name: {fileInfo.Name}");
            Console.WriteLine($"Full Path: {fileInfo.FullName}");
            Console.WriteLine($"Extension: {fileInfo.Extension}");
            Console.WriteLine($"Size (bytes): {fileInfo.Length}");
            Console.WriteLine($"Created On: {fileInfo.CreationTime}");

            // Copy file
            fileInfo.CopyTo("copy_example4.txt", true);

            // Delete file
            // fileInfo.Delete();
        }
    }
}

```

* `FileInfo.Length` → Size of the file in bytes.
* `FileInfo.CreationTime` → Date/time created.
* `CopyTo()` and `Delete()` → Manipulate the file physically.