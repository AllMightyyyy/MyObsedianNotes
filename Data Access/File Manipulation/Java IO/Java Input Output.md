---
tags:
  - FileManipulation
  - JavaIO
---

Created: October 25, 2024 11:30 PM
Class: Data Access
Goal: Understand and practice essential file operations in Java, including file creation, reading, writing, and deletion
Tasks and Exercises: File Navigation and Info / Basic Text File operations / Extension handling
Topics Covered: File Class, File Filtering, Text Files

# Java File Manipulation Basics : The **File** Class

The File class in Java provides methods for handling file paths, creating files/directories, and managing metadata.

Primarily used for file and directory navigation, rather than directly reading or writing content

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
        
        ```java
        import java.io.File;
        import java.io.IOException;
        
        public class FileDemo {
            public static void main(String[] args) {
                File file = new File("example.txt");
        
                try {
                    if (file.createNewFile()) {
                        System.out.println("File created: " + file.getName());
                    } else {
                        System.out.println("File already exists.");
                    }
        
                    System.out.println("Absolute Path: " + file.getAbsolutePath());
                    System.out.println("File size: " + file.length() + " bytes");
        
                    if (file.delete()) {
                        System.out.println("File deleted successfully");
                    }
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
        ```
        
    

# Reading and Writing Text Files :

Java offers several ways to read and write text files.

The **FileReader/FileWriter** and **BufferedReader/BufferedWriter** classes are the classic, widely-used choices.

### Classic approach with **FileReader** and **FileWriter** :

- FileReader and FileWriter are basic character stream classes used for reading and writing character data to/from files
- These classes are straightforward but not good for large file manipulation since they lack buffering
    - Methods in **FileReader** and **FileWriter**
        - read() → reads a single character or an array of characters
        - write() → writes a single character, character array, or string
        - close() → closes the stream and releases resources
            - Example :
            
            ```java
            import java.io.FileReader;
            import java.io.FileWriter;
            import java.io.IOException;
            
            public class FileReaderWriterDemo {
                public static void main(String[] args) {
                    try (FileWriter writer = new FileWriter("output.txt")) {
                        writer.write("Hello, World!\n");
                        writer.write("Writing to a file using FileWriter.\n");
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
            
                    try (FileReader reader = new FileReader("output.txt")) {
                        int character;
                        while ((character = reader.read()) != -1) {
                            System.out.print((char) character);
                        }
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
            }
            ```
            

### Modern approach with BufferedReader and BufferedWriter :

- BufferedReader and BufferedWriter wrap around FileReader and FileWriter to add buffering, significantly improving performance for large files
- BufferedReader provides efficient reading methods like **readLine() which reads an entire line instead of one character at a time**
- Best Practice → always use BufferedReader and BufferedWriter for reading/writing large text files for efficiency
    - Example :
    
    ```java
    import java.io.BufferedReader;
    import java.io.BufferedWriter;
    import java.io.FileReader;
    import java.io.FileWriter;
    import java.io.IOException;
    
    public class BufferedFileIO {
        public static void main(String[] args) {
            try (BufferedWriter writer = new BufferedWriter(new FileWriter("buffered_output.txt"))) {
                writer.write("This is a line of text.");
                writer.newLine();
                writer.write("Buffered writing is efficient for large files.");
            } catch (IOException e) {
                e.printStackTrace();
            }
    
            try (BufferedReader reader = new BufferedReader(new FileReader("buffered_output.txt"))) {
                String line;
                while ((line = reader.readLine()) != null) {
                    System.out.println(line);
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
    ```
    

### Best Practices for File I/O in Java :

- Use try-with-resources → ensures that resources are closed automatically, even if exceptions occur
    - Example :
    
    ```java
    try (BufferedReader reader = new BufferedReader(new FileReader("file.txt"))) {
        // Read from file
    } catch (IOException e) {
        e.printStackTrace();
    }
    ```
    
- Prefer Buffered Streams for Large Files :
    - Buffered streams like **BufferedReader** and **BufferedWriter offer better performance due to internal buffering**
    - Avoid mixing readers and writers with binary data :
        - FileReader and FileWriter are designed for character data
        - For binary data, use FileInputStream and FileOutputStream
    - Specify encoding when necessary : Use OutputStreamWriter with a specified charset for writing text with a particular encoding such as UTF-8
        - Example :
        
        ```java
        try (Writer writer = new OutputStreamWriter(new FileOutputStream("file.txt"), StandardCharsets.UTF_8)) {
            writer.write("Text with UTF-8 encoding");
        }
        ```
        
    - Error Handling : Always handle IOException and ensure proper resource release ( achieved automatically with try-with-resources )

# Modern techniques with JAVA NIO ( Non-blocking I/O ) :

Java’s NIO ( New I/O ) package, introduced in JAVA 7, offers a more flexible, efficient and modern way of handling files and directories. It’s based on **Path** and **Files** classes, which provides more advanced and flexible options than **File**

### Key Classes and Methods in Java NIO :

- Path Class → Represents file and directory paths ( more powerful and flexible than **File** )
    - Create a Path instance
    
    ```java
    Paths.get("filePath")
    ```
    
    - Joins two paths
    
    ```java
    resolve()
    ```
    
- Files Class → Provides utility methods for file operations, like reading, writing, copying, and moving files
    - Files.readAllLines(Path) → Reads all lines from a file as a list of strings
    - Files.write(Path, Iterable) → Writes lines to a file
    - Files.copy(Path, Path) → Copies a file from one location to another
    - Files.move(Path, Path) → Moves or renames a file
    - Example of NIO For File Operations :
    
    ```java
    import java.io.IOException;
    import java.nio.file.Files;
    import java.nio.file.Path;
    import java.nio.file.Paths;
    import java.util.List;
    
    public class NIOFileDemo {
        public static void main(String[] args) {
            Path path = Paths.get("nio_output.txt");
    
            try {
                // Writing to a file
                Files.write(path, List.of("Line 1", "Line 2", "Line 3"));
                
                // Reading from a file
                List<String> lines = Files.readAllLines(path);
                lines.forEach(System.out::println);
    
                // Copying a file
                Path targetPath = Paths.get("nio_output_copy.txt");
                Files.copy(path, targetPath);
    
                // Moving/Renaming a file
                Files.move(targetPath, Paths.get("nio_output_renamed.txt"));
    
                // Deleting a file
                Files.delete(Paths.get("nio_output_renamed.txt"));
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
    ```
    
- Avantages of Java NIO over Classic I/O :
    - Better error handling → NIO uses more descriptive exceptions
    - Path Manipulation → The Path class is more intuitive and flexible for path manipulations
    - Enhanced File Operations → The Files class offers more efficient file copying, moving, and attribute handling

# Exercises To Test Mastery :

### File Operations :

- Write a program that creates a directory structure for a project, such as "src", "bin", and "resources" folders. -> Solution : 

```java
import java.io.File;  
import java.util.Arrays;  
  
public class Main {  
    /*  
    Write a program that creates a directory structure for a project, such as "src", "bin", and "resources" folders.     */    public static void main(String[] args) {  
        File project = new File("project");  
        project.mkdir();  ``
        String[] subDirs = {"src", "bin", "resources"};  
        for (String subDir : subDirs) {  
            File subDirectory = new File(project, subDir);  
            subDirectory.mkdir();  
        }        System.out.println(Arrays.stream(project.list()).toList());  
    }  
}
```

- Write a file manager using `NIO` that lets users select a file, view its contents, copy it to another directory, and delete it -> Solution :
```java
import java.io.IOException;  
import java.nio.file.Files;  
import java.nio.file.Paths;  
  
public class Main {  
    public static void main(String[] args) {  
        String path = "C://Users//zakar//testCopy";  
        try {  
            System.out.println(Files.list(Paths.get(path)));  
            Files.copy(Paths.get(path), Paths.get("C://Users//zakar//FileOperations//FileManagerNIO//testPaste"));  
            Files.delete(Paths.get(path));  
            System.out.println("done");  
        } catch (IOException e) {  
            throw new RuntimeException(e);  
        }    }  
}
```

### Reading and writing text files :

- Create a CSV parser that reads a CSV file into memory and then writes it back after processing specific columns.
```java
import java.io.*;  
import java.util.Objects;  
import java.util.Scanner;  
  
public class Main {  
    public static void main(String[] args) {  
        Scanner sc = new Scanner(System.in);  
        System.out.println("Enter a month");  
        System.out.println("Options are : JAN , FEB , MAR , APR , MAY , JUN, JUL, AUG, SEP, OCT, NOV, DEC");  
        String month = sc.nextLine().toUpperCase();  // Ensure uppercase input for consistency  
        System.out.println("Enter year, Options are : 1958 , 1959 , 1960");  
        int year = sc.nextInt();  
  
        File csvFile = new File("data.csv");  
        if (!csvFile.exists()) {  
            System.out.println("CSV file not found!");  
            return;  
        }  
        try {  
            BufferedReader reader = new BufferedReader(new FileReader(csvFile));  
            String header = reader.readLine();  // Skip header line  
            System.out.println("Header: " + header);  // Debug: Print header  
  
            String line;  
            boolean found = false;  
            while ((line = reader.readLine()) != null) {  
                System.out.println("Reading line: " + line);  // Debug: Print each line read  
                String[] data = line.split(",");  
                if (data.length < 4) {  
                    System.out.println("Skipping malformed line: " + line);  // Debug: Line format check  
                    continue;  
                }  
  
                if ((data[0].trim()).contains(month)) {  
                    System.out.println("Match found for month: " + data[0].trim());  // Debug: Print matched month  
                    AirTravelStats airTravelStat = new AirTravelStats(  
                            data[0].trim(),  
                            Integer.parseInt(data[1].trim()),  
                            Integer.parseInt(data[2].trim()),  
                            Integer.parseInt(data[3].trim())  
                    );  
  
                    switch (year) {  
                        case 1958:  
                            System.out.println("Value for 1958: " + airTravelStat.getYear1());  
                            break;  
                        case 1959:  
                            System.out.println("Value for 1959: " + airTravelStat.getYear2());  
                            break;  
                        case 1960:  
                            System.out.println("Value for 1960: " + airTravelStat.getYear3());  
                            break;  
                        default:  
                            System.out.println("Year not available");  
                    }  
                    found = true;  
                    break;  
                }  
            }  
  
            if (!found) {  
                System.out.println("Month not found in data.");  
            }  
            reader.close();  
        } catch (IOException ex) {  
            throw new RuntimeException(ex);  
        }    }  
}

public class AirTravelStats {  
    private String monthAbv;  
    private int year1;  
    private int year2;  
    private int year3;  
  
    public AirTravelStats(String monthAbv, int year1, int year2, int year3) {  
        this.monthAbv = monthAbv;  
        this.year1 = year1;  
        this.year2 = year2;  
        this.year3 = year3;  
    }  
  
    public int getYear1() {  
        return year1;  
    }  
  
    public void setYear1(int year1) {  
        this.year1 = year1;  
    }  
  
    public String getMonthAbv() {  
        return monthAbv;  
    }  
  
    public void setMonthAbv(String monthAbv) {  
        this.monthAbv = monthAbv;  
    }  
  
    public int getYear2() {  
        return year2;  
    }  
  
    public void setYear2(int year2) {  
        this.year2 = year2;  
    }  
  
    public int getYear3() {  
        return year3;  
    }  
  
    public void setYear3(int year3) {  
        this.year3 = year3;  
    }  
}
```
### File information Extractor :

- **Objective**: Create a program that takes a file path as input and displays detailed information about the file.
- **Details**:
    - Use the `File` class to display the file’s name, path, absolute path, size (in bytes), and last modified date.
    - Check if the file is readable, writable, and executable.
- **Extra**: Implement this as a loop so the user can check multiple files without restarting the program.
```java
import java.io.File;  
import java.util.ArrayList;  
import java.util.List;  
import java.util.Scanner;  
  
public class Main {  
    public static void main(String[] args) {  
        /*  
        Program that takes a file path as input and displays detailed information about the file.        Display the file’s name, path, absolute path, size (in bytes), and last modified date.        Check if the file is readable, writable, and executable.        Implement this as a loop so the user can check multiple files without restarting the program.         */  
        boolean run = true;  
        while (run) {  
            Scanner sc = new Scanner(System.in);  
            System.out.println("Enter a file path to display information about, or type exit");  
            String path = sc.nextLine();  
            if (path.toLowerCase().equals("exit")) {  
                run = false;  
                break;  
            }  
            File file = new File(path);  
            String[] fileSubDirs = file.list();  
            System.out.println(file.getAbsolutePath() + " privilieges are : ");  
            String canExecute = file.canExecute() ? "can execute the file" : "can not execute the file";  
            String canRead = file.canRead() ? "can read the file" : "can not read the file";  
            String canWrite = file.canWrite() ? "can write the file" : "can not write the file";  
            System.out.println("the related privilieges are : " + canExecute + " and " + canRead + " and " + canWrite);  
            System.out.println("File name : " + file.getName());  
            System.out.println("File path : " + path);  
            System.out.println("File is directory : " + file.isDirectory());  
            System.out.println("File is a file : " + file.isFile());  
            System.out.println("File size ( in bytes ) : " + file.length());  
            System.out.println("File last modified date : " + file.lastModified());  
        }    }  
}
```
### **Directory Structure Creator**

- **Objective**: Write a program to create a predefined project directory structure.
- **Details**:
    - Prompt the user to enter the base directory path.
    - Create subdirectories like `/src`, `/bin`, `/resources`, and `/docs` inside the base directory.
    - Verify each directory’s creation and print a success or failure message.
- **Extra**: Allow the user to specify additional custom subdirectory names.
```java
	import java.io.File;  
import java.nio.file.Path;  
import java.nio.file.Paths;  
import java.util.ArrayList;  
import java.util.List;  
import java.util.Scanner;  
  
public class Main {  
    public static void main(String[] args) {  
        // List of essential directories  
        List<String> essentialDirs = new ArrayList<>();  
        essentialDirs.add("src");  
        essentialDirs.add("bin");  
        essentialDirs.add("resources");  
        essentialDirs.add("docs");  
  
        Scanner sc = new Scanner(System.in);  
  
        // Asking for additional custom subdirectories  
        System.out.println("Do you want to specify any additional subdirectories (in addition to src, bin, resources, and docs)?");  
        System.out.println("Enter the names separated by commas, or leave empty if no additional directories are needed:");  
        String response = sc.nextLine().trim();  
  
        // Adding additional directories if specified  
        if (!response.isEmpty()) {  
            String[] additionalDirs = response.split(",");  
            for (String dirToAdd : additionalDirs) {  
                dirToAdd = dirToAdd.trim();  
                if (!dirToAdd.isEmpty() && !essentialDirs.contains(dirToAdd)) {  
                    essentialDirs.add(dirToAdd);  
                }  
            }  
        }  
  
        // Asking for the base directory path  
        System.out.println("Enter the base directory path for your project (copy-paste the absolute path):");  
        String basePath = sc.nextLine().trim();  
  
        // Normalize the path  
        Path normalizedPath = Paths.get(basePath).toAbsolutePath().normalize();  
        File baseDir = normalizedPath.toFile();  
  
        // Create the base directory if it doesn't exist  
        if (!baseDir.exists()) {  
            if (baseDir.mkdirs()) {  
                System.out.println("Base directory created successfully: " + baseDir.getPath());  
            } else {  
                System.out.println("Failed to create base directory: " + baseDir.getPath());  
                sc.close();  
                return; // Exit the program if base directory creation fails  
            }  
        } else {  
            System.out.println("Base directory already exists: " + baseDir.getPath());  
        }  
        // Creating subdirectories  
        for (String dir : essentialDirs) {  
            File subDir = new File(baseDir, dir);  
            if (!subDir.exists()) {  
                if (subDir.mkdirs()) {  
                    System.out.println("Directory created successfully: " + subDir.getPath());  
                } else {  
                    System.out.println("Failed to create directory: " + subDir.getPath());  
                }  
            } else {  
                System.out.println("Directory already exists: " + subDir.getPath());  
            }  
        }  
  
        sc.close();  
        System.out.println("Directory structure setup completed.");  
    }  
}
```
### **Directory Content Lister**

- **Objective**: List all files and directories inside a specified directory.
- **Details**:
    - Print the name of each file and directory along with its size (in KB).
    - Implement filtering to show only files with specific extensions (e.g., `.txt`, `.java`).
- **Extra**: Implement recursive listing to show the entire directory tree.
```java
import java.io.File;  
import java.util.ArrayList;  
import java.util.List;  
import java.util.Scanner;  
  
public class Main {  
    public static void main(String[] args) {  
        /*  
        List all files and directories inside a specified directory.        Print the name of each file and directory along with its size (in KB).        Implement filtering to show only files with specific extensions (e.g., `.txt`, `.java`).        Implement recursive listing to show the entire directory tree.         */        Scanner sc = new Scanner(System.in);  
        System.out.println("Enter the directory to scan:");  
        String dir = sc.nextLine();  
        File file = new File(dir);  
  
        if (!file.isDirectory()) {  
            System.out.println("Not a valid directory.");  
        } else {  
            System.out.println("Enter file extensions to filter (comma-separated, e.g., .txt,.java), or leave empty to show all files:");  
            String filter = sc.nextLine().trim();  
            String[] extensions = filter.isEmpty() ? new String[0] : filter.split(",");  
            System.out.println("\nScanned directory:");  
            listAllSubDirectoriesAndFiles(file, extensions, 0);  
        }  
        sc.close();  
    }  
  
    static void listAllSubDirectoriesAndFiles(File file, String[] extensions, int level) {  
        if (file.isDirectory()) {  
            // Indent according to directory level  
            String indent = "  ".repeat(level);  
            System.out.println(indent + "[DIR] " + file.getName());  
  
            File[] subFiles = file.listFiles();  
            if (subFiles != null) {  
                for (File subFile : subFiles) {  
                    listAllSubDirectoriesAndFiles(subFile, extensions, level + 1);  
                }  
            }  
        } else {  
            // Check if the file matches the specified extensions  
            if (shouldDisplayFile(file, extensions)) {  
                String indent = "  ".repeat(level);  
                long fileSizeInKB = file.length() / 1024;  
                System.out.println(indent + "[FILE] " + file.getName() + " (" + fileSizeInKB + " KB)");  
            }  
        }  
    }  
  
    static boolean shouldDisplayFile(File file, String[] extensions) {  
        // If no extensions are specified, show all files  
        if (extensions.length == 0) {  
            return true;  
        }  
        // Check if the file ends with any of the specified extensions  
        String fileName = file.getName().toLowerCase();  
        for (String ext : extensions) {  
            if (fileName.endsWith(ext.toLowerCase().trim())) {  
                return true;  
            }  
        }  
        return false;  
    }  
}
```
### **Reading and Writing Text Files**

### **Line-by-Line File Reader**

- **Objective**: Create a program to read a text file line by line and display each line on the console.
- **Details**:
    - Use `BufferedReader` to read each line and display line numbers with each line of text.
- **Extra**: Implement a feature to stop reading after a certain line count (e.g., after 100 lines).
```java
import java.io.*;  
  
public class Main {  
    public static void main(String[] args) {  
        /*  
        Create a program to read a text file line by line and display each line on the console.        Use `BufferedReader` to read each line and display line numbers with each line of text.        Implement a feature to stop reading after a certain line count (e.g., after 100 lines).            */        try {  
            String filePath = "textToReader";  
            File file = new File(filePath);  
            BufferedReader br = new BufferedReader(new InputStreamReader(new FileInputStream(file)));  
            String line;  
            int i = 1;  
            while ((line = br.readLine()) != null) {  
                System.out.println(i + ": " + line);  
                i++;  
                if (i == 101) {  
                    break;  
                }  
            }  
            br.close();  
        } catch (IOException e) {  
            throw new RuntimeException(e);  
        }    }  
}
```
### **File Content Reverser**

- **Objective**: Write a program to reverse the lines of a text file.
- **Details**:
    - Read the entire file into memory and reverse the order of lines.
    - Save the reversed content to a new file.
- **Extra**: Add an option to reverse the content of each line as well (i.e., reverse characters within each line).
```java
import java.io.*;  
import java.util.ArrayList;  
import java.util.Collections;  
import java.util.Scanner;  
  
public class Main {  
    public static void main(String[] args) {  
        /*  
        Write a program to reverse the lines of a text file.        Read the entire file into memory and reverse the order of lines.        Save the reversed content to a new file.        Add an option to reverse the content of each line as well (i.e., reverse characters within each line).        */  
        Scanner sc = new Scanner(System.in);  
  
        // Prompt user for the file path  
        System.out.println("Enter the path of the file to read:");  
        String filePath = sc.nextLine();  
        File fileToRead = new File(filePath);  
  
        // Check if the file exists  
        if (!fileToRead.exists() || !fileToRead.isFile()) {  
            System.out.println("The specified file does not exist or is not a valid file.");  
            sc.close();  
            return;  
        }  
        // Get user options for reversing  
        System.out.println("Do you want to reverse the order of lines? (Y/N)");  
        boolean reverseLines = sc.nextLine().trim().equalsIgnoreCase("Y");  
  
        System.out.println("Do you want to reverse the content of each line? (Y/N)");  
        boolean reverseContent = sc.nextLine().trim().equalsIgnoreCase("Y");  
  
        System.out.println("Enter the path of the output file:");  
        String outputFilePath = sc.nextLine();  
  
        try (BufferedReader reader = new BufferedReader(new FileReader(fileToRead));  
             BufferedWriter writer = new BufferedWriter(new FileWriter(outputFilePath))) {  
  
            // Read all lines from the file  
            ArrayList<String> lines = new ArrayList<>();  
            String line;  
            while ((line = reader.readLine()) != null) {  
                lines.add(line);  
            }  
  
            // Reverse the order of lines if requested  
            if (reverseLines) {  
                Collections.reverse(lines);  
            }  
  
            // Reverse the content of each line if requested  
            if (reverseContent) {  
                for (int i = 0; i < lines.size(); i++) {  
                    StringBuilder sb = new StringBuilder(lines.get(i));  
                    lines.set(i, sb.reverse().toString());  
                }  
            }  
  
            // Write the modified lines to the new file  
            for (String modifiedLine : lines) {  
                writer.write(modifiedLine);  
                writer.newLine(); // Add a new line after each line  
            }  
  
            System.out.println("The modified content has been saved to " + outputFilePath);  
  
        } catch (IOException e) {  
            System.err.println("An error occurred while processing the file: " + e.getMessage());  
        } finally {  
            sc.close();  
        }    }  
}
```
### **Word Count Tool**

- **Objective**: Implement a word count tool that displays the number of words, lines, and characters in a file.
- **Details**:
    - Use `BufferedReader` to count lines and words.
    - Ignore whitespace and punctuation when counting words.
    - Print the results in a structured output.
- **Extra**: Add a feature to find and display the most frequently occurring word.
```java
import java.io.*;  
import java.util.HashMap;  
import java.util.Map;  
import java.util.regex.Pattern;  
  
public class Main {  
    /*  
        Implement a word count tool that displays the number of words, lines, and characters in a file.        Use `BufferedReader` to count lines and words.        Ignore whitespace and punctuation when counting words.        Print the results in a structured output.        Add a feature to find and display the most frequently occurring word     */   
        public static void main(String[] args) {  
        int lineCount = 0;  
        int wordCount = 0;  
        int charCount = 0;  
        Map<String, Integer> wordFrequency = new HashMap<String, Integer>();  
  
        // Define a pattern to match words (ignoring punctuation)  
        Pattern wordPattern = Pattern.compile("\\b[a-zA-Z]+\\b");  
  
        try {  
            BufferedReader br = new BufferedReader(new InputStreamReader(new FileInputStream(new File("FileToRead"))));  
            String line;  
  
            while ((line = br.readLine()) != null) {  
                lineCount++;  
                charCount += line.length();  
  
                String[] words = line.split("\\W+");  
                for(String word : words) {  
                    if (!word.isEmpty()) {  
                        wordCount++;  
                        String wordLowerCase = word.toLowerCase();  
                        wordFrequency.put(wordLowerCase, wordFrequency.getOrDefault(wordLowerCase, 0) + 1);  
                    }  
                }  
            }  
  
            String wordMostFrequent = null;  
            int maxFrequency = 0;  
            for (Map.Entry<String, Integer> entry : wordFrequency.entrySet()) {  
                if (entry.getValue() > maxFrequency) {  
                    maxFrequency = entry.getValue();  
                    wordMostFrequent = entry.getKey();  
                }  
            }  
  
            System.out.println("File Analysis Results:");  
            System.out.println("----------------------");  
            System.out.println("Total Lines: " + lineCount);  
            System.out.println("Total Words: " + wordCount);  
            System.out.println("Total Characters: " + charCount);  
            if (wordMostFrequent != null) {  
                System.out.println("Most Frequent Word: " + wordMostFrequent + " (occurs " + maxFrequency + " times)");  
            } else {  
                System.out.println("No words found in the file.");  
            }  
  
        } catch (IOException e) {  
            throw new RuntimeException(e);  
        }    }  
}
```
### **Text File Searcher**

- **Objective**: Create a program to search for a specific word in a text file and display its line number and position.
- **Details**:
    - Prompt the user to enter a file path and the word to search for.
    - For each occurrence, print the line number and index within the line.
- **Extra**: Add support for case-insensitive search and allow the user to specify multiple words.
```java
import java.io.*;  
import java.util.*;  
  
public class Main {  
    public static void main(String[] args) {  
        /*  
        Create a program to search for a specific word in a text file and display its line number and position.        Prompt the user to enter a file path and the word(s) to search for.        For each occurrence, print the line number and index within the line.        Add support for case-insensitive search and allow the user to specify multiple words.         */  
        Scanner sc = new Scanner(System.in);  
  
        // Prompt user to enter file path  
        System.out.println("Enter the path of the file to search:");  
        String filePath = sc.nextLine();  
  
        // Prompt user to enter the word(s) to search for, separated by commas  
        System.out.println("Enter the word(s) to search for (separated by commas):");  
        String[] wordsToSearch = sc.nextLine().toLowerCase().split(",");  
  
        // Trim each word to remove extra spaces  
        for (int i = 0; i < wordsToSearch.length; i++) {  
            wordsToSearch[i] = wordsToSearch[i].trim();  
        }  
        // Try reading the file and searching for the words  
        try (BufferedReader br = new BufferedReader(new FileReader(filePath))) {  
            String line;  
            int lineNumber = 1;  
  
            // Map to store each word's occurrences with line numbers and positions  
            Map<String, List<String>> wordOccurrences = new HashMap<>();  
  
            while ((line = br.readLine()) != null) {  
                // Convert line to lowercase for case-insensitive search  
                String lowerCaseLine = line.toLowerCase();  
  
                // Check each word to see if it's in the line  
                for (String word : wordsToSearch) {  
                    int index = lowerCaseLine.indexOf(word);  
                    while (index != -1) {  
                        // Record the occurrence  
                        String occurrence = "Line " + lineNumber + ", Index " + index;  
                        wordOccurrences  
                                .computeIfAbsent(word, k -> new ArrayList<>())  
                                .add(occurrence);  
  
                        // Find the next occurrence in the line  
                        index = lowerCaseLine.indexOf(word, index + 1);  
                    }  
                }  
                lineNumber++;  
            }  
  
            // Display the results  
            for (String word : wordsToSearch) {  
                List<String> occurrences = wordOccurrences.get(word);  
                if (occurrences != null && !occurrences.isEmpty()) {  
                    System.out.println("Occurrences of \"" + word + "\":");  
                    for (String occurrence : occurrences) {  
                        System.out.println("  - " + occurrence);  
                    }  
                } else {  
                    System.out.println("No occurrences of \"" + word + "\" found.");  
                }  
            }  
        } catch (IOException e) {  
            System.err.println("Error reading the file: " + e.getMessage());  
        }  
        sc.close();  
    }  
}
```
---

### **Buffered File Operations for Efficiency**

### **Large File Copier with Progress Tracking**

- **Objective**: Create a program to copy a large text or binary file and track progress.
- **Details**:
    - Use `BufferedInputStream` and `BufferedOutputStream` for efficient copying.
    - Track and display progress in percentage (e.g., “50% complete”).
- **Extra**: Display estimated time remaining based on copying speed.
```java
import java.io.*;  
  
public class Main {  
    public static void main(String[] args) {  
        /*  
        Create a program to copy a large text or binary file and track progress.        Use `BufferedInputStream` and `BufferedOutputStream` for efficient copying.        Track and display progress in percentage (e.g., “50% complete”).        Display estimated time remaining based on copying speed.         */  
        String fileSource = "FileSource";  
        String fileDestination = "FileDestination";  
  
        copyFileWithProgress(fileSource, fileDestination);  
    }  
  
    private static void copyFileWithProgress(String fileSource, String fileDestination) {  
        File sourceFile = new File(fileSource);  
        File destinationFile = new File(fileDestination);  
  
        // Check if source file exists  
        if (!sourceFile.exists()) {  
            System.out.println("Source file does not exist");  
            return;  
        }  
  
        // Get total size of source file  
        long totalBytes = sourceFile.length();  
        long copiedBytes = 0;  
        int bufferSize = 1024 * 8; // 8 kb buffer size  
  
        try (BufferedInputStream bis = new BufferedInputStream(new FileInputStream(sourceFile));  
             BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(destinationFile))) {  
  
            byte[] buffer = new byte[bufferSize];  
            int bytesRead;  
            long startTime = System.nanoTime();  
            while ((bytesRead = bis.read(buffer)) != -1) {  
                bos.write(buffer, 0, bytesRead);  
                copiedBytes += bytesRead;  
  
                // calculate the progress percentage  
                int progress = (int) ((copiedBytes * 100) / totalBytes);  
  
                // calculate elapsed time and estimate remaining time  
                long elapsedTime = System.nanoTime() - startTime;  
                double secondsElapsed = elapsedTime / 1_000_000_000.0;  
                double speed = copiedBytes / totalBytes;  
                double timeRemaining = (totalBytes - copiedBytes) / speed;  
  
                // Display progress and estimated time remaining  
                System.out.printf("\rProgress: %d%% - Time remaining: %.2f seconds", progress, timeRemaining);  
            }  
  
            // Ensure all data is written  
            bos.flush();  
  
            System.out.println("\nFile copy completed successfully!");  
        } catch (IOException e) {  
            System.err.println("Error during file copy: " + e.getMessage());  
        }    }  
}
```
### **File Splitter and Merger**

- **Objective**: Write a program that splits a large text file into multiple smaller files and then merges them back.
- **Details**:
    - Prompt the user for the size of each chunk in lines or bytes.
    - Write each chunk to a new file (`file_part1.txt`, `file_part2.txt`, etc.).
    - Implement a method to merge the files back in the correct order.
- **Extra**: Add an option to split and merge binary files.
```java
import java.io.*;  
import java.util.Scanner;  
  
public class Main {  
    /*  
        Write a program that splits a large text file into multiple smaller files and then merges them back.        Prompt the user for the size of each chunk in lines or bytes.        Write each chunk to a new file (`file_part1.txt`, `file_part2.txt`, etc.).        Implement a method to merge the files back in the correct order.        Add an option to split and merge binary files.         */    public static void main(String[] args) {  
        Scanner sc = new Scanner(System.in);  
  
        // Get user choice for splitting  
        System.out.println("Choose an option:\n1. Split a text file\n2. Split a binary file");  
        int choice = sc.nextInt();  
        sc.nextLine(); // Consume the newline  
  
        // Get the file path        System.out.println("Enter the path of the file to split:");  
        String filePath = sc.nextLine();  
  
        // Get the chunk size  
        System.out.println("Enter the size of each chunk (in lines or bytes):");  
        int chunkSize = sc.nextInt();  
        sc.nextLine(); // Consume the newline  
  
        // Split the file        try {  
            if (choice == 1) {  
                splitTextFile(filePath, chunkSize);  
            } else if (choice == 2) {  
                splitBinaryFile(filePath, chunkSize);  
            } else {  
                System.out.println("Invalid choice.");  
            }  
  
            // Get user choice for merging  
            System.out.println("\nDo you want to merge the split files back? (Y/N)");  
            String mergeChoice = sc.nextLine().trim().toLowerCase();  
            if (mergeChoice.equals("y")) {  
                System.out.println("Enter the output file path for merging:");  
                String outputFilePath = sc.nextLine();  
  
                if (choice == 1) {  
                    mergeTextFiles(outputFilePath);  
                } else {  
                    mergeBinaryFiles(outputFilePath);  
                }  
            }  
  
        } catch (IOException e) {  
            System.err.println("An error occurred: " + e.getMessage());  
        }  
        sc.close();  
    }  
  
    // Method to split a text file by lines  
    public static void splitTextFile(String filePath, int linesPerChunk) throws IOException {  
        BufferedReader reader = new BufferedReader(new FileReader(filePath));  
        String line;  
        int filePart = 1;  
        int lineCount = 0;  
  
        PrintWriter writer = new PrintWriter(new FileWriter("file_part" + filePart + ".txt"));  
  
        while ((line = reader.readLine()) != null) {  
            writer.println(line);  
            lineCount++;  
  
            if (lineCount >= linesPerChunk) {  
                writer.close();  
                filePart++;  
                lineCount = 0;  
                writer = new PrintWriter(new FileWriter("file_part" + filePart + ".txt"));  
            }  
        }  
  
        reader.close();  
        writer.close();  
  
        System.out.println("Text file split into " + filePart + " parts.");  
    }  
  
    // Method to split a binary file by bytes  
    public static void splitBinaryFile(String filePath, int bytesPerChunk) throws IOException {  
        BufferedInputStream inputStream = new BufferedInputStream(new FileInputStream(filePath));  
        byte[] buffer = new byte[bytesPerChunk];  
        int bytesRead;  
        int filePart = 1;  
  
        while ((bytesRead = inputStream.read(buffer)) != -1) {  
            FileOutputStream outputStream = new FileOutputStream("file_part" + filePart + ".bin");  
            outputStream.write(buffer, 0, bytesRead);  
            outputStream.close();  
            filePart++;  
        }  
        inputStream.close();  
        System.out.println("Binary file split into " + (filePart - 1) + " parts.");  
    }  
  
    // Method to merge text files back  
    public static void mergeTextFiles(String outputFilePath) throws IOException {  
        PrintWriter writer = new PrintWriter(new FileWriter(outputFilePath));  
        int filePart = 1;  
        File partFile = new File("file_part" + filePart + ".txt");  
  
        while (partFile.exists()) {  
            BufferedReader reader = new BufferedReader(new FileReader(partFile));  
            String line;  
            while ((line = reader.readLine()) != null) {  
                writer.println(line);  
            }  
            reader.close();  
            filePart++;  
            partFile = new File("file_part" + filePart + ".txt");  
        }  
        writer.close();  
        System.out.println("Text files merged into " + outputFilePath);  
    }  
  
    // Method to merge binary files back  
    public static void mergeBinaryFiles(String outputFilePath) throws IOException {  
        FileOutputStream outputStream = new FileOutputStream(outputFilePath);  
        int filePart = 1;  
        File partFile = new File("file_part" + filePart + ".bin");  
  
        while (partFile.exists()) {  
            FileInputStream inputStream = new FileInputStream(partFile);  
            byte[] buffer = new byte[1024];  
            int bytesRead;  
            while ((bytesRead = inputStream.read(buffer)) != -1) {  
                outputStream.write(buffer, 0, bytesRead);  
            }  
            inputStream.close();  
            filePart++;  
            partFile = new File("file_part" + filePart + ".bin");  
        }  
        outputStream.close();  
        System.out.println("Binary files merged into " + outputFilePath);  
    }  
}
```
### **Efficient Log Filter**

- **Objective**: Create a program to filter specific log entries from a large log file.
- **Details**:
    - Read the log file using `BufferedReader` and filter entries based on keywords (e.g., "ERROR", "WARNING").
    - Write filtered lines to a new file named `filtered_log.txt`.
- **Extra**: Implement filtering by date range if the log entries include timestamps.
```Java
import java.io.*;  
import java.text.ParseException;  
import java.text.SimpleDateFormat;  
import java.util.Date;  
import java.util.Scanner;  
  
public class Main {  
    public static void main(String[] args) {  
        /*  
        Create a program to filter specific log entries from a large log file.        Read the log file using `BufferedReader` and filter entries based on keywords (e.g., "ERROR", "WARNING").        Write filtered lines to a new file named `filtered_log.txt`.        Implement filtering by date range if the log entries include timestamps.         */  
        Scanner sc = new Scanner(System.in);  
  
        // Prompt user for the log file path  
        System.out.println("Enter the path of the log file:");  
        String logFilePath = sc.nextLine();  
  
        // Prompt user for keywords to filter by (comma-separated)  
        System.out.println("Enter the keywords to filter by (e.g., ERROR, WARNING), separated by commas:");  
        String[] keywords = sc.nextLine().split(",");  
  
        // Prompt user for date range filtering  
        System.out.println("Do you want to filter by date range? (Y/N)");  
        boolean filterByDateRange = sc.nextLine().trim().equalsIgnoreCase("Y");  
  
        String startDateStr = null;  
        String endDateStr = null;  
        SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd");  
  
        if (filterByDateRange) {  
            // Prompt user for start and end dates  
            System.out.println("Enter the start date (yyyy-MM-dd):");  
            startDateStr = sc.nextLine();  
  
            System.out.println("Enter the end date (yyyy-MM-dd):");  
            endDateStr = sc.nextLine();  
        }  
        // Filter the log file  
        try {  
            filterLogFile(logFilePath, keywords, filterByDateRange, startDateStr, endDateStr, dateFormat);  
            System.out.println("Log file filtered successfully. Output saved to 'filtered_log.txt'.");  
        } catch (IOException | ParseException e) {  
            System.err.println("An error occurred: " + e.getMessage());  
        }  
        sc.close();  
    }  
  
    // Method to filter the log file based on keywords and an optional date range  
    public static void filterLogFile(String logFilePath, String[] keywords, boolean filterByDateRange,  
                                     String startDateStr, String endDateStr, SimpleDateFormat dateFormat)  
            throws IOException, ParseException {  
  
        // Convert start and end dates to Date objects, if filtering by date range  
        Date startDate = null;  
        Date endDate = null;  
  
        if (filterByDateRange) {  
            startDate = dateFormat.parse(startDateStr);  
            endDate = dateFormat.parse(endDateStr);  
        }  
        // Open the log file for reading  
        BufferedReader reader = new BufferedReader(new FileReader(logFilePath));  
        BufferedWriter writer = new BufferedWriter(new FileWriter("filtered_log.txt"));  
  
        String line;  
        while ((line = reader.readLine()) != null) {  
            // Check if the line contains any of the specified keywords  
            boolean containsKeyword = false;  
            for (String keyword : keywords) {  
                if (line.contains(keyword.trim())) {  
                    containsKeyword = true;  
                    break;  
                }  
            }  
  
            // If the line doesn't contain any keyword, skip it  
            if (!containsKeyword) {  
                continue;  
            }  
  
            // If filtering by date range, check if the log entry's date is within the range  
            if (filterByDateRange) {  
                // Assume the date is at the beginning of the line in the format yyyy-MM-dd  
                String dateStr = line.substring(0, 10);  
                Date logDate = dateFormat.parse(dateStr);  
  
                // If the log date is outside the specified range, skip it  
                if (logDate.before(startDate) || logDate.after(endDate)) {  
                    continue;  
                }  
            }  
  
            // Write the filtered line to the output file  
            writer.write(line);  
            writer.newLine();  
        }  
        // Close resources  
        reader.close();  
        writer.close();  
    }  
}
```
---
### Random Access Files

### **Indexed File Reader**

- **Objective**: Create a program to read and display specific sections of a file using random access.
- **Details**:
    - Write a method to jump to a specific byte in the file and read a defined number of bytes.
    - Print the extracted content as text.
- **Extra**: Prompt the user for start and end positions in the file.
```java
import java.io.File;  
import java.io.FileNotFoundException;  
import java.io.IOException;  
import java.io.RandomAccessFile;  
import java.util.Arrays;  
import java.util.Scanner;  
  
public class Main {  
    public static void main(String[] args) {  
        /*  
        Create a program to read and display specific sections of a file using random access.        Write a method to jump to a specific byte in the file and read a defined number of bytes.        Print the extracted content as text.        Prompt the user for start and end positions in the file.         */        Scanner sc = new Scanner(System.in);  
        String filePath = "FileToRead.txt";  
        File fileToRead = new File(filePath);  
  
        try (RandomAccessFile raf = new RandomAccessFile(fileToRead, "r")) {  
  
            System.out.println("Enter byte start position to read from:");  
            int byteStart = Integer.parseInt(sc.nextLine());  
  
            System.out.println("Enter byte end position to read to:");  
            int byteEnd = Integer.parseInt(sc.nextLine());  
  
            if (byteStart < 0 || byteEnd <= byteStart || byteEnd > raf.length()) {  
                System.out.println("Invalid byte range specified.");  
                return;  
            }  
  
            int byteAmount = byteEnd - byteStart;  
  
            byte[] data = new byte[byteAmount];  
            raf.seek(byteStart);  
            int bytesRead = raf.read(data, 0, byteAmount);  
  
            if (bytesRead == -1) {  
                System.out.println("Could not read any data from the file.");  
            } else {  
                String content = new String(data);  
                System.out.println("Extracted content: " + content);  
            }  
  
        } catch (IOException e) {  
            System.out.println("An error occurred while reading the file: " + e.getMessage());  
        }    }  
}
```
### **Binary Data Updater**

- **Objective**: Implement a program to modify specific bytes in a binary file using `RandomAccessFile`.
- **Details**:
    - Write data at specified byte positions without altering the rest of the file.
    - For example, modify the header or footer of an image file.
- **Extra**: Add functionality to restore original data if necessary.
```java
import java.io.File;  
import java.io.FileNotFoundException;  
import java.io.IOException;  
import java.io.RandomAccessFile;  
import java.util.RandomAccess;  
import java.util.Scanner;  
  
public class Main {  
    public static void main(String[] args) {  
        /*  
        Implement a program to modify specific bytes in a binary file using `RandomAccessFile`.        Write data at specified byte positions without altering the rest of the file.        For example, modify the header or footer of an image file.        Add functionality to restore original data if necessary.         */        String filePath = "image.png";  
        try (RandomAccessFile raf = new RandomAccessFile(new File(filePath), "rw")) {  
            Scanner sc = new Scanner(System.in);  
  
            // Read original data to allow for restoration later  
            byte[] originalData = new byte[(int) raf.length()];  
            raf.readFully(originalData); // Reads entire file content into the array  
  
            // Prompt user for the byte position to modify            System.out.println("Enter the byte position to modify:");  
            int position = Integer.parseInt(sc.nextLine());  
  
            // Validate the position  
            if (position < 0 || position >= raf.length()) {  
                System.out.println("Invalid byte position specified.");  
                return;  
            }  
  
            // Prompt user for the new data (in bytes) to write at the specified position  
            System.out.println("Enter the byte value (0-255) to insert at the specified position:");  
            int newByteValue = Integer.parseInt(sc.nextLine());  
            if (newByteValue < 0 || newByteValue > 255) {  
                System.out.println("Invalid byte value. Must be between 0 and 255.");  
                return;  
            }  
  
            // Move the file pointer to the specified position and write the new byte value  
            raf.seek(position);  
            raf.write(newByteValue);  
            System.out.println("Byte modified successfully.");  
  
            // Ask if the user wants to restore the original content  
            System.out.println("Do you want to restore the original content? (Y/N)");  
            boolean restore = sc.nextLine().trim().equalsIgnoreCase("Y");  
            if (restore) {  
                // Truncate the file to the original length before restoring  
                raf.setLength(originalData.length);  
                raf.seek(0);  
                raf.write(originalData);  
                System.out.println("Original content restored.");  
            }  
  
        } catch (IOException e) {  
            System.err.println("An error occurred while modifying the file: " + e.getMessage());  
        }    }  
}
```
### **Fixed-Length Record Database**

- **Objective**: Create a fixed-length record database file that allows for quick access to individual records.
- **Details**:
    - Each record should be exactly 100 bytes and contain fields like ID, name, and age.
    - Implement methods to add, update, retrieve, and delete records by record ID.
- **Extra**: Add an index file to speed up record searches by ID.
```java
import java.nio.charset.StandardCharsets;  
import java.util.Arrays;  
  
public class Record {  
    private int id;  
    private String name;  
    private int age;  
  
    public static final int RECORD_SIZE = 100;  
    private static final int NAME_SIZE = 80;  
  
    public Record(int id, String name, int age) {  
        this.id = id;  
        this.name = name;  
        this.age = age;  
    }  
  
    public int getId() {  
        return id;  
    }  
  
    public String getName() {  
        return name;  
    }  
  
    public int getAge() {  
        return age;  
    }  
  
    public byte[] toByteArray() {  
        byte[] record = new byte[RECORD_SIZE];  
        Arrays.fill(record, (byte) 0); // Fill with zeroes to finish the 100 required  
  
        // convert ID to bytes        record[0] = (byte) (id >> 24);  
        record[1] = (byte) (id >> 16);  
        record[2] = (byte) (id >> 8);  
        record[3] = (byte) id;  
  
        // convert name to bytes  
        byte[] nameBytes = name.getBytes(StandardCharsets.UTF_8);  
        System.arraycopy(nameBytes, 0, record, 4, Math.min(nameBytes.length, NAME_SIZE));  
  
        // convert age to bytes  
        record[84] = (byte) (age >> 24);  
        record[85] = (byte) (age >> 16);  
        record[86] = (byte) (age >> 8);  
        record[87] = (byte) (age);  
  
        return record;  
    }  
  
    // Create a record from a byte array  
    public static Record fromByteArray(byte[] byteArray) {  
        if (byteArray.length != RECORD_SIZE) {  
            throw new IllegalArgumentException("Invalid record size.");  
        }  
        // Read ID  
        int id = ((byteArray[0] & 0xFF) << 24) | ((byteArray[1] & 0xFF) << 16)  
                | ((byteArray[2] & 0xFF) << 8) | (byteArray[3] & 0xFF);  
  
        // Read name  
        String name = new String(byteArray, 4, NAME_SIZE, StandardCharsets.UTF_8).trim();  
  
        // Read age  
        int age = ((byteArray[84] & 0xFF) << 24) | ((byteArray[85] & 0xFF) << 16)  
                | ((byteArray[86] & 0xFF) << 8) | (byteArray[87] & 0xFF);  
  
        return new Record(id, name, age);  
    }  
}
```
```java
import java.io.File;  
import java.io.IOException;  
import java.io.RandomAccessFile;  
import java.util.HashMap;  
import java.util.Map;  
  
public class Database {  
    private final File databaseFile;  
    private final File indexFile;  
    private final Map<Integer, Long> indexMap = new HashMap<>();  
  
    public Database(String dbFilePath, String indexFilePath) throws IOException {  
        this.databaseFile = new File(dbFilePath);  
        this.indexFile = new File(indexFilePath);  
  
        // Load existing index if available  
        if (indexFile.exists()) {  
            loadIndex();  
        }    }  
  
    // Load the index file into memory  
    private void loadIndex() throws IOException {  
        try (RandomAccessFile raf = new RandomAccessFile(indexFile, "r")) {  
            while (raf.getFilePointer() < raf.length()) {  
                int id = raf.readInt();  
                long position = raf.readLong();  
                indexMap.put(id, position);  
            }  
        }  
    }  
  
    // Save the index map to the index file  
    private void saveIndex() throws IOException {  
        try (RandomAccessFile raf = new RandomAccessFile(indexFile, "rw")) {  
            raf.setLength(0); // Clear the file  
            for (Map.Entry<Integer, Long> entry : indexMap.entrySet()) {  
                raf.writeInt(entry.getKey());  
                raf.writeLong(entry.getValue());  
            }  
        }  
    }  
  
    // Add a new record  
    public void addRecord(Record record) throws IOException {  
        if (indexMap.containsKey(record.getId())) {  
            System.out.println("Record with ID already exists.");  
            return;  
        }  
        try (RandomAccessFile raf = new RandomAccessFile(databaseFile, "rw")) {  
            long position = raf.length(); // Append at the end of the file  
            raf.seek(position);  
            raf.write(record.toByteArray());  
  
            // Update index  
            indexMap.put(record.getId(), position);  
            saveIndex();  
        }    }  
  
    // Retrieve a record by ID  
    public Record getRecord(int id) throws IOException {  
        Long position = indexMap.get(id);  
        if (position == null) {  
            System.out.println("Record not found.");  
            return null;  
        }  
        try (RandomAccessFile raf = new RandomAccessFile(databaseFile, "r")) {  
            raf.seek(position);  
            byte[] buffer = new byte[Record.RECORD_SIZE];  
            raf.readFully(buffer);  
            return Record.fromByteArray(buffer);  
        }    }  
  
    // Update an existing record  
    public void updateRecord(Record record) throws IOException {  
        Long position = indexMap.get(record.getId());  
        if (position == null) {  
            System.out.println("Record not found.");  
            return;  
        }  
        try (RandomAccessFile raf = new RandomAccessFile(databaseFile, "rw")) {  
            raf.seek(position);  
            raf.write(record.toByteArray());  
        }    }  
  
    // Delete a record  
    public void deleteRecord(int id) throws IOException {  
        Long position = indexMap.remove(id);  
        if (position == null) {  
            System.out.println("Record not found.");  
            return;  
        }  
        // Mark the record as deleted by setting the ID to -1  
        try (RandomAccessFile raf = new RandomAccessFile(databaseFile, "rw")) {  
            raf.seek(position);  
            raf.writeInt(-1);  
        }  
        // Update the index file  
        saveIndex();  
    }  
}
```
```java
import java.io.IOException;  
import java.util.Scanner;  
  
public class Main {  
    public static void main(String[] args) {  
        /*  
        Create a fixed-length record database file that allows for quick access to individual records.        Each record should be exactly 100 bytes and contain fields like ID, name, and age.        Implement methods to add, update, retrieve, and delete records by record ID.        Add an index file to speed up record searches by ID.         */  
        try {  
            Database db = new Database("database.dat", "index.dat");  
            Scanner sc = new Scanner(System.in);  
  
            while (true) {  
                System.out.println("\nMenu:");  
                System.out.println("1. Add record");  
                System.out.println("2. Get record");  
                System.out.println("3. Update record");  
                System.out.println("4. Delete record");  
                System.out.println("5. Exit");  
                System.out.print("Choose an option: ");  
  
                int choice = Integer.parseInt(sc.nextLine());  
  
                switch (choice) {  
                    case 1:  
                        System.out.print("Enter ID: ");  
                        int id = Integer.parseInt(sc.nextLine());  
                        System.out.print("Enter name: ");  
                        String name = sc.nextLine();  
                        System.out.print("Enter age: ");  
                        int age = Integer.parseInt(sc.nextLine());  
                        db.addRecord(new Record(id, name, age));  
                        break;  
                    case 2:  
                        System.out.print("Enter ID: ");  
                        id = Integer.parseInt(sc.nextLine());  
                        Record record = db.getRecord(id);  
                        if (record != null) {  
                            System.out.println("ID: " + record.getId());  
                            System.out.println("Name: " + record.getName());  
                            System.out.println("Age: " + record.getAge());  
                        }  
                        break;  
                    case 3:  
                        System.out.print("Enter ID: ");  
                        id = Integer.parseInt(sc.nextLine());  
                        System.out.print("Enter new name: ");  
                        name = sc.nextLine();  
                        System.out.print("Enter new age: ");  
                        age = Integer.parseInt(sc.nextLine());  
                        db.updateRecord(new Record(id, name, age));  
                        break;  
                    case 4:  
                        System.out.print("Enter ID: ");  
                        id = Integer.parseInt(sc.nextLine());  
                        db.deleteRecord(id);  
                        break;  
                    case 5:  
                        System.out.println("Exiting...");  
                        return;  
                    default:  
                        System.out.println("Invalid choice. Try again.");  
                }  
            }  
        } catch (IOException e) {  
            System.err.println("Error: " + e.getMessage());  
        }    }  
}
```
---

### **Modern File Handling with Java NIO**

### **NIO File Attributes Viewer**

- **Objective**: Write a program that retrieves and displays detailed file attributes using NIO.
- **Details**:
    - Use `Files` and `BasicFileAttributes` to display creation time, last modified time, and file size.
- **Extra**: Also display owner, permissions, and access control lists (ACLs) if supported by the OS.
```Java
import java.io.IOException;  
import java.nio.file.FileSystems;  
import java.nio.file.Files;  
import java.nio.file.Path;  
import java.nio.file.attribute.*;  
import java.util.List;  
import java.util.Set;  
  
public class Main {  
    public static void main(String[] args) {  
        /*  
        This program retrieves and displays detailed file attributes using NIO.        It uses `Files` and `BasicFileAttributes` to display creation time, last modified time, and file size.        It also displays owner, permissions, and access control lists (ACLs) if supported by the OS.         */  
        String filePath = "FileToManipulate";   
Path path = Path.of(filePath);  
  
        try {  
            // Retrieve basic file attributes  
            BasicFileAttributes attributes = Files.readAttributes(path, BasicFileAttributes.class);  
            System.out.println("Creation Time -> " + attributes.creationTime());  
            System.out.println("Last Access Time -> " + attributes.lastAccessTime());  
            System.out.println("Last Modified Time -> " + attributes.lastModifiedTime());  
            System.out.println("Is Directory -> " + attributes.isDirectory());  
            System.out.println("Is Regular File -> " + attributes.isRegularFile());  
            System.out.println("Is Symbolic Link -> " + attributes.isSymbolicLink());  
            System.out.println("Is Other -> " + attributes.isOther());  
            System.out.println("File Size (bytes) -> " + attributes.size());  
  
            // Retrieve file owner  
            UserPrincipal owner = Files.getOwner(path);  
            System.out.println("File Owner -> " + owner.getName());  
  
            // Retrieve file permissions (POSIX-compliant file systems only)  
            if (FileSystems.getDefault().supportedFileAttributeViews().contains("posix")) {  
                Set<PosixFilePermission> permissions = Files.getPosixFilePermissions(path);  
                System.out.println("File Permissions -> " + PosixFilePermissions.toString(permissions));  
            } else {  
                System.out.println("File Permissions -> Not supported on this file system.");  
            }  
  
            // Retrieve Access Control Lists (ACLs)  
            if (Files.getFileAttributeView(path, AclFileAttributeView.class) != null) {  
                AclFileAttributeView aclView = Files.getFileAttributeView(path, AclFileAttributeView.class);  
                List<AclEntry> aclList = aclView.getAcl();  
                System.out.println("Access Control List (ACL):");  
                for (AclEntry entry : aclList) {  
                    System.out.println(entry);  
                }  
            } else {  
                System.out.println("Access Control List (ACL) -> Not supported on this file system.");  
            }  
  
        } catch (IOException e) {  
            System.err.println("An error occurred while retrieving file attributes: " + e.getMessage());  
        }    }  
}
```
### **Directory Watcher for Real-time Monitoring**

- **Objective**: Implement a real-time directory monitoring program using `WatchService` in NIO.
- **Details**:
    - Monitor a directory for changes (file creation, modification, deletion).
    - Display notifications in the console for each detected event.
- **Extra**: Allow the user to specify multiple directories to watch simultaneously.
```Java
import java.io.File;  
import java.io.IOException;  
import java.nio.file.*;  
import java.util.ArrayList;  
import java.util.List;  
import java.util.Scanner;  
  
public class Main {  
    public static void main(String[] args) {  
        /*  
        Implement a real-time directory monitoring program using `WatchService` in NIO.        Monitor a directory for changes (file creation, modification, deletion).        Display notifications in the console for each detected event.        Allow the user to specify multiple directories to watch simultaneously.         */        Scanner scanner = new Scanner(System.in);  
        List<Path> directoriesToWatch = new ArrayList<>();  
  
        // Allow the user to specify multiple directories to watch  
        System.out.println("Enter directories to watch (comma-separated):");  
        String input = scanner.nextLine();  
        String[] paths = input.split(",");  
  
        // Add each specified directory to the list  
        for (String pathStr : paths) {  
            Path path = Paths.get(pathStr.trim());  
            if (Files.isDirectory(path)) {  
                directoriesToWatch.add(path);  
            } else {  
                System.out.println("Invalid directory: " + pathStr.trim());  
            }  
        }  
  
        if (directoriesToWatch.isEmpty()) {  
            System.out.println("No valid directories specified to watch.");  
            return;  
        }  
        // Start monitoring the directories  
        try {  
            WatchService watchService = FileSystems.getDefault().newWatchService();  
  
            // Register each directory with the WatchService  
            for (Path dir : directoriesToWatch) {  
                dir.register(watchService, StandardWatchEventKinds.ENTRY_CREATE,  
                        StandardWatchEventKinds.ENTRY_MODIFY,  
                        StandardWatchEventKinds.ENTRY_DELETE);  
                System.out.println("Watching directory: " + dir);  
            }  
  
            // Monitor the WatchService for events  
            System.out.println("Monitoring directories for changes...");  
            while (true) {  
                WatchKey key;  
                try {  
                    // Wait for a watch key to be signaled  
                    key = watchService.take();  
                } catch (InterruptedException ex) {  
                    System.out.println("Directory monitoring interrupted.");  
                    return;  
                }  
  
                for (WatchEvent<?> event : key.pollEvents()) {  
                    WatchEvent.Kind<?> kind = event.kind();  
  
                    // Context for the event is the file name (relative to the watched directory)  
                    Path fileName = (Path) event.context();  
                    Path directory = (Path) key.watchable();  
  
                    // Display event type and file name  
                    System.out.println("Event detected: " + kind.name() + " - " + directory.resolve(fileName));  
                }  
  
                // Reset the key (important for further event processing)  
                boolean valid = key.reset();  
                if (!valid) {  
                    System.out.println("WatchKey no longer valid. Stopping monitoring for a directory.");  
                    break;  
                }  
            }  
        } catch (IOException e) {  
            System.err.println("An error occurred while setting up the WatchService: " + e.getMessage());  
        }    }  
}
```
### **Recursive Directory Copier**

- **Objective**: Create a program to recursively copy all files and subdirectories from a source directory to a destination.
- **Details**:
    - Use `Files.walkFileTree` to traverse directories and `Files.copy` to perform the copying.
    - Maintain the original directory structure in the destination.
- **Extra**: Display progress and handle cases where files already exist in the destination (overwrite, skip, rename).
```Java
import java.io.IOException;  
import java.nio.file.*;  
import java.nio.file.attribute.BasicFileAttributes;  
import java.util.Scanner;  
  
public class Main {  
    public static void main(String[] args) {  
        /*  
        This program recursively copies all files and subdirectories from a source directory to a destination.        It uses `Files.walkFileTree` for traversal and `Files.copy` for copying files.        The original directory structure is maintained in the destination.        */  
        Scanner scanner = new Scanner(System.in);  
  
        System.out.print("Enter the source directory: ");  
        String sourceDir = scanner.nextLine().trim();  
        System.out.print("Enter the destination directory: ");  
        String destinationDir = scanner.nextLine().trim();  
  
        Path sourcePath = Paths.get(sourceDir);  
        Path destinationPath = Paths.get(destinationDir);  
  
        // Validate source directory  
        if (!Files.isDirectory(sourcePath)) {  
            System.out.println("Source is not a valid directory.");  
            return;  
        }  
        try {  
            // Use a custom FileVisitor to recursively copy files  
            Files.walkFileTree(sourcePath, new SimpleFileVisitor<Path>() {  
                @Override                public FileVisitResult preVisitDirectory(Path dir, BasicFileAttributes attrs) throws IOException {  
                    // Calculate the destination path for the current directory  
                    Path targetDir = destinationPath.resolve(sourcePath.relativize(dir));  
                    if (!Files.exists(targetDir)) {  
                        // Create the directory if it does not exist in the destination  
                        Files.createDirectories(targetDir);  
                        System.out.println("Created directory: " + targetDir);  
                    }  
                    return FileVisitResult.CONTINUE;  
                }  
  
                @Override  
                public FileVisitResult visitFile(Path file, BasicFileAttributes attrs) throws IOException {  
                    // Calculate the destination path for the current file  
                    Path targetFile = destinationPath.resolve(sourcePath.relativize(file));  
  
                    // Check if the file already exists in the destination  
                    if (Files.exists(targetFile)) {  
                        // Prompt user for action if file already exists  
                        System.out.print("File already exists: " + targetFile + ". Overwrite (O), Skip (S), Rename (R)? ");  
                        String choice = scanner.nextLine().trim().toUpperCase();  
  
                        switch (choice) {  
                            case "O": // Overwrite  
                                Files.copy(file, targetFile, StandardCopyOption.REPLACE_EXISTING);  
                                System.out.println("Overwritten file: " + targetFile);  
                                break;  
                            case "S": // Skip  
                                System.out.println("Skipped file: " + targetFile);  
                                break;  
                            case "R": // Rename  
                                Path renamedFile = getUniqueFileName(targetFile);  
                                Files.copy(file, renamedFile);  
                                System.out.println("Renamed and copied file: " + renamedFile);  
                                break;  
                            default:  
                                System.out.println("Invalid choice. Skipping file: " + targetFile);  
                                break;  
                        }  
                    } else {  
                        // If file does not exist, simply copy it  
                        Files.copy(file, targetFile);  
                        System.out.println("Copied file: " + targetFile);  
                    }  
                    return FileVisitResult.CONTINUE;  
                }  
            });  
  
            System.out.println("File copy completed.");  
        } catch (IOException e) {  
            System.err.println("An error occurred during the file copy process: " + e.getMessage());  
        }    }  
  
    // Helper method to generate a unique file name if the file already exists  
    private static Path getUniqueFileName(Path file) {  
        int count = 1;  
        String fileName = file.getFileName().toString();  
        String fileExtension = "";  
        int dotIndex = fileName.lastIndexOf('.');  
  
        // Separate the base name and extension if the file has an extension  
        if (dotIndex != -1) {  
            fileExtension = fileName.substring(dotIndex);  
            fileName = fileName.substring(0, dotIndex);  
        }  
        // Keep generating new file names until a unique one is found  
        Path newFileName = file.resolveSibling(fileName + "_" + count + fileExtension);  
        while (Files.exists(newFileName)) {  
            count++;  
            newFileName = file.resolveSibling(fileName + "_" + count + fileExtension);  
        }        return newFileName;  
    }  
}
```
### **File Comparison Tool**

- **Objective**: Write a program to compare two files for equality, byte-by-byte.
- **Details**:
    - Read both files using `Files.readAllBytes` or `Files.newInputStream` and compare each byte.
    - Print if the files are identical or different.
- **Extra**: For large files, compare using a buffer and read chunks instead of the entire file at once.
Simple Approach ( small files ) -> 
```Java
import java.io.IOException;  
import java.nio.file.Files;  
import java.nio.file.Path;  
  
public class Main {  
    public static void main(String[] args) {  
        /*  
        This program compares two files byte-by-byte using `Files.readAllBytes`.        It prints whether the files are identical or different.        */  
        String filePath1 = "File1.txt";  
        String filePath2 = "File2.txt";  
  
        try {  
            // Read all bytes from both files  
            byte[] file1Bytes = Files.readAllBytes(Path.of(filePath1));  
            byte[] file2Bytes = Files.readAllBytes(Path.of(filePath2));  
  
            // Compare the byte arrays  
            if (file1Bytes.length != file2Bytes.length) {  
                System.out.println("Files are different (size mismatch).");  
            } else {  
                boolean areIdentical = true;  
                for (int i = 0; i < file1Bytes.length; i++) {  
                    if (file1Bytes[i] != file2Bytes[i]) {  
                        areIdentical = false;  
                        break;  
                    }  
                }  
  
                if (areIdentical) {  
                    System.out.println("Files are identical.");  
                } else {  
                    System.out.println("Files are different.");  
                }  
            }  
  
        } catch (IOException e) {  
            System.err.println("An error occurred while reading the files: " + e.getMessage());  
        }    }  
}
```
 large files -> 
```Java
import java.io.IOException;  
import java.io.InputStream;  
import java.nio.file.Files;  
import java.nio.file.Path;  
  
public class Main {  
    private static final int BUFFER_SIZE = 8192; // 8 KB buffer size  
  
    public static void main(String[] args) {  
        /*  
        This program compares two files byte-by-byte using a buffer for large files.        It reads both files in chunks and compares each chunk to avoid memory issues.        */  
        String filePath1 = "File1.txt";  
        String filePath2 = "File2.txt";  
  
        try {  
            boolean areIdentical = compareFiles(Path.of(filePath1), Path.of(filePath2));  
            if (areIdentical) {  
                System.out.println("Files are identical.");  
            } else {  
                System.out.println("Files are different.");  
            }  
        } catch (IOException e) {  
            System.err.println("An error occurred while comparing the files: " + e.getMessage());  
        }    }  
  
    private static boolean compareFiles(Path file1, Path file2) throws IOException {  
        // Compare file sizes first  
        if (Files.size(file1) != Files.size(file2)) {  
            return false; // Different sizes, so files are not identical  
        }  
  
        // Use try-with-resources to ensure streams are closed  
        try (InputStream is1 = Files.newInputStream(file1);  
             InputStream is2 = Files.newInputStream(file2)) {  
  
            byte[] buffer1 = new byte[BUFFER_SIZE];  
            byte[] buffer2 = new byte[BUFFER_SIZE];  
  
            int bytesRead1;  
            int bytesRead2;  
  
            // Read both files in chunks and compare each chunk  
            while ((bytesRead1 = is1.read(buffer1)) != -1 &&  
                    (bytesRead2 = is2.read(buffer2)) != -1) {  
                if (bytesRead1 != bytesRead2 || !compareBuffers(buffer1, buffer2, bytesRead1)) {  
                    return false; // Files are different  
                }  
            }  
        }  
  
        return true; // Files are identical  
    }  
  
    // Helper method to compare buffers  
    private static boolean compareBuffers(byte[] buffer1, byte[] buffer2, int length) {  
        for (int i = 0; i < length; i++) {  
            if (buffer1[i] != buffer2[i]) {  
                return false;  
            }  
        }  
        return true;  
    }  
}
```
---

### **General Best Practices Exercises**

### **Try-with-Resources Mastery**

- **Objective**: Practice using try-with-resources to ensure all resources are managed efficiently.
- **Details**:
    - Rewrite previous exercises (e.g., file reading and writing) to use try-with-resources.
- **Extra**: Handle multiple resources (e.g., a `BufferedReader` and a `BufferedWriter` in the same try block).

### **Exception Handling and Logging**

- **Objective**: Build a logging utility for file operations that writes errors to a log file.
- **Details**:
    - In each file operation, catch exceptions and log detailed error messages to a log file.
- **Extra**: Use `java.util.logging` or a third-party logging library like Log4j for structured logging.