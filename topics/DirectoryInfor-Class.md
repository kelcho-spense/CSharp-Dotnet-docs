# DirectoryInfo Class

Used for managing directories (folders) — `creation`, `deletion`, `listing files`, and `subdirectories`.

```C#
using System;
using System.IO;

namespace FileHandlingDemo
{
    class DirectoryInfoExample
    {
        static void Main()
        {
            string folderPath = "TestFolder";

            // Create directory if not exists
            DirectoryInfo dir = new DirectoryInfo(folderPath);
            if (!dir.Exists)
            {
                dir.Create();
                Console.WriteLine("Directory created successfully.");
            }

            // Create a file in the directory
            File.WriteAllText(Path.Combine(folderPath, "file1.txt"), "Hello from file1!");
            File.WriteAllText(Path.Combine(folderPath, "file2.txt"), "Hello from file2!");

            // Get files
            FileInfo[] files = dir.GetFiles();
            Console.WriteLine("\nFiles in Directory:");
            foreach (FileInfo file in files)
            {
                Console.WriteLine($"{file.Name} ({file.Length} bytes)");
            }

            // Get subdirectories (if any)
            DirectoryInfo[] subDirs = dir.GetDirectories();
            Console.WriteLine("\nSubdirectories:");
            foreach (DirectoryInfo sub in subDirs)
            {
                Console.WriteLine(sub.Name);
            }

            // dir.Delete(true); // Deletes directory and all contents
        }
    }
}

```

* `dir.Create()` → Creates a new directory.
* `GetFiles()` → Returns all files in the directory.
* `GetDirectories()` → Lists subfolders.
* `Delete(true)` → Deletes directory and its contents.