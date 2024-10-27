---

mindmap-plugin: basic

---

# Java I/O

## Java File Manipulation Basics : The **File** Class
- Key Methods of **File Class :**
    - File And Directory Creation :
        - createNewFile() → creates new file, empty file → returns true ( file created successfully ) , false if it already exists
        - mkdir() and mkdirs() → create directories. mkdir() creates a single directory, while mkdirs() creates parent directories if they don’t exist
    - Path and File Information :
        - getName(), getPath(), and getAbsolutePath() → Provides file name, path, and absolute path
        - getParent() → returns the parent directory
        - length() → returns file size in bytes
    - File Existence and Deletion :
        - exists() → checks if file or directory exists
        - delete() → Deletes the file, return true if successful
        - isDirectory(), isFile() → Check if the File instance is a directory or a regular file
    - Listing files and directories :
        - list() → lists all file names in the directory
        - listFiles() → lists all files in the directory as File objects
    - Basic Example :
        - Example Code :

## Reading and Writing Text Files :
- Classic approach with **FileReader** and **FileWriter** :
    - FileReader and FileWriter are basic character stream classes used for reading and writing character data to/from files
    - These classes are straightforward but not good for large file manipulation since they lack buffering
        - Methods in **FileReader** and **FileWriter**
            - read() → reads a single character or an array of characters
            - write() → writes a single character, character array, or string
            - close() → closes the stream and releases resources
                - Example :
- Modern approach with BufferedReader and BufferedWriter :
    - BufferedReader and BufferedWriter wrap around FileReader and FileWriter to add buffering, significantly improving performance for large files
    - BufferedReader provides efficient reading methods like **readLine() which reads an entire line instead of one character at a time**
    - Best Practice → always use BufferedReader and BufferedWriter for reading/writing large text files for efficiency
        - Example :
- Best Practices for File I/O in Java :
    - Use try-with-resources → ensures that resources are closed automatically, even if exceptions occur
        - Example :
    - Prefer Buffered Streams for Large Files :
        - Buffered streams like **BufferedReader** and **BufferedWriter offer better performance due to internal buffering**
        - Avoid mixing readers and writers with binary data :
            - FileReader and FileWriter are designed for character data
            - For binary data, use FileInputStream and FileOutputStream
        - Specify encoding when necessary : Use OutputStreamWriter with a specified charset for writing text with a particular encoding such as UTF-8
            - Example :
        - Error Handling : Always handle IOException and ensure proper resource release ( achieved automatically with try-with-resources )

## Modern techniques with JAVA NIO ( Non-blocking I/O ) :
- Key Classes and Methods in Java NIO :
    - Path Class → Represents file and directory paths ( more powerful and flexible than **File** )
        -
            - Create a Path instance
        -
            - Joins two paths
    - Files Class → Provides utility methods for file operations, like reading, writing, copying, and moving files
        - Files.readAllLines(Path) → Reads all lines from a file as a list of strings
        - Files.write(Path, Iterable) → Writes lines to a file
        - Files.copy(Path, Path) → Copies a file from one location to another
        - Files.move(Path, Path) → Moves or renames a file
        - Example of NIO For File Operations :
    - Avantages of Java NIO over Classic I/O :
        - Better error handling → NIO uses more descriptive exceptions
        - Path Manipulation → The Path class is more intuitive and flexible for path manipulations
        - Enhanced File Operations → The Files class offers more efficient file copying, moving, and attribute handling