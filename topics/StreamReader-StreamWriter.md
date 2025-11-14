# StreamReader &amp; StreamWriter

Used to **read/write text data** from and to files. These are higher-level classes built on top of streams.

```C#
using System;
using System.IO;

namespace FileHandlingDemo
{
    class StreamReaderWriterExample
    {
        static void Main()
        {
            string path = "example2.txt";

            // Writing text using StreamWriter
            using (StreamWriter writer = new StreamWriter(path))
            {
                writer.WriteLine("This is StreamWriter.");
                writer.WriteLine("It writes text easily!");
            }

            // Reading text using StreamReader
            using (StreamReader reader = new StreamReader(path))
            {
                string content = reader.ReadToEnd();
                Console.WriteLine("StreamReader Read:\n" + content);
            }
        }
    }
}
```

* `StreamWriter.WriteLine()` → Writes text line by line.* 
* `StreamReader.ReadToEnd()` → Reads all the text from a file.* 
* Automatically handles text encoding.