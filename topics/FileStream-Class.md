# FileStream Class

The FileStream class is used for reading and writing bytes directly to a file. It’s the lowest-level file I/O operation and works with raw byte data.

```C#
using System;
using System.IO;

namespace FileHandlingDemo
{
    class FileStreamExample
    {
        static void Main()
        {
            string path = "example1.txt";

            // Writing bytes to file
            using (FileStream fs = new FileStream(path, FileMode.Create))
            {
                byte[] data = System.Text.Encoding.UTF8.GetBytes("Hello, FileStream!");
                fs.Write(data, 0, data.Length);
            }

            // Reading bytes from file
            using (FileStream fs = new FileStream(path, FileMode.Open, FileAccess.Read))
            {
                byte[] buffer = new byte[fs.Length];
                fs.Read(buffer, 0, buffer.Length);
                string content = System.Text.Encoding.UTF8.GetString(buffer);
                Console.WriteLine($"FileStream Read: {content}");
            }
        }
    }
}
```

* `FileMode.Create` → Creates a new file or overwrites if it exists.
* `fs.Write() →` Writes byte array to the file.
* `fs.Read() →` Reads byte data from the file.
* `Encoding.UTF8` converts text ↔ bytes.