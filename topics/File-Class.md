# File Class

The File class provides static helper methods to quickly read, write, and manage files without manually opening streams.

```C#
using System;
using System.IO;

namespace FileHandlingDemo
{
    class FileClassExample
    {
        static void Main()
        {
            string path = "example3.txt";

            // Write all text
            File.WriteAllText(path, "This text is written using File class!");

            // Read all text
            string content = File.ReadAllText(path);
            Console.WriteLine("File Content: " + content);

            // Append text
            File.AppendAllText(path, "\nAdding another line!");
            Console.WriteLine("\nAfter Append:");
            Console.WriteLine(File.ReadAllText(path));
        }
    }
}
```

* `File.WriteAllText()` → Creates or overwrites text files.
* `File.ReadAllText()` → Reads entire file content.
* `File.AppendAllText()` → Adds new text without overwriting