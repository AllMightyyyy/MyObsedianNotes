Java's NIO.2 (introduced in Java 7 as part of the `java.nio` package) provides a comprehensive set of APIs for advanced file operations, offering greater flexibility and performance compared to the traditional I/O APIs. This section explores NIO.2's features, focusing on file locking, asynchronous I/O, and the `Path` class.

### Introduction to NIO.2
**Java NIO (New I/O)** enhances Java's I/O capabilities by introducing non-blocking I/O operations, channels, buffers, and selectors. **NIO.2** extends these features, providing improved file system access, file metadata handling, and asynchronous I/O operations.

**Key Features of NIO.2:**
- **Path API:** Simplifies file system path operations.
- **Asynchronous I/O:** Enables non-blocking I/O operations.
- **File Locking:** Prevents concurrent access issues.
- **Watch Service API:** Monitors file system changes.
- **Advanced File Attributes:** Access and modify file metadata.
### The `Path` Class and Filesystem Operations
The `Path` class, part of the `java.nio.file` package, represents a path in the file system. It provides methods for path manipulation, comparison, and file operations.

**Creating a Path:
```java
import java.nio.file.Path;
import java.nio.file.Paths;

public class PathExample {
    public static void main(String[] args) {
        Path path1 = Paths.get("C:/Users/Example/Documents/file.txt");
        Path path2 = Paths.get("C:", "Users", "Example", "Documents", "file.txt");

        System.out.println("Path1: " + path1);
        System.out.println("Path2: " + path2);
    }
}
```
**Common `Path` Methods:**
- **`getFileName()`**: Retrieves the file name.
- **`getParent()`**: Retrieves the parent path.
- **`resolve(String other)`**: Combines paths.
- **`normalize()`**: Removes redundant elements.
- **`toAbsolutePath()`**: Converts to an absolute path.
- **`toUri()`**: Converts to a URI.

**Example:**
```java
Path path = Paths.get("C:/Users/Example/Documents/../file.txt");
System.out.println("Original Path: " + path);
System.out.println("Normalized Path: " + path.normalize());
```
Output : 
`Original Path: C:\Users\Example\Documents\..\file.txt
`Normalized Path: C:\Users\Example\file.txt

### File Locking in Java
File locking ensures that multiple processes or threads do not concurrently modify a file, preventing data corruption and ensuring data integrity.

**Java Implementation:**
- **`FileChannel`**: Provides a channel for reading, writing, mapping, and manipulating a file.
- **`FileLock`**: Represents a lock on a file region.
**Types of Locks:**
- **Shared Lock (Read Lock)**: Multiple threads can hold a shared lock simultaneously.
- **Exclusive Lock (Write Lock)**: Only one thread can hold an exclusive lock, preventing other threads from accessing the locked region.

**Example: Acquiring an Exclusive Lock**
```java
import java.io.RandomAccessFile;
import java.nio.channels.FileChannel;
import java.nio.channels.FileLock;

public class FileLockExample {
    public static void main(String[] args) {
        try (RandomAccessFile raf = new RandomAccessFile("lockedFile.txt", "rw");
             FileChannel channel = raf.getChannel()) {

            // Acquire an exclusive lock on the file
            FileLock lock = channel.lock();

            System.out.println("File locked.");

            // Perform file operations
            raf.writeUTF("Writing to the locked file.");

            // Release the lock
            lock.release();
            System.out.println("File lock released.");

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
**Key Points:**
- **Blocking vs. Non-blocking Locks:** By default, `lock()` is a blocking call. Use `tryLock()` for non-blocking attempts.
- **Lock Scope:** You can lock the entire file or specific regions using `lock(long position, long size, boolean shared)`.
- **Automatic Lock Release:** Locks are automatically released when the associated channel is closed.
**Example: Non-blocking Lock Attempt**
```java
FileLock lock = channel.tryLock();
if (lock != null) {
    try {
        // Perform operations
    } finally {
        lock.release();
    }
} else {
    System.out.println("Could not acquire lock.");
}
```
**Caveats:**
- **Platform Dependency:** Locking behavior can vary across operating systems.
- **Deadlocks:** Always ensure that locks are released to prevent deadlocks.
### Asynchronous I/O
Asynchronous I/O allows Java applications to perform I/O operations without blocking the executing thread. This is particularly useful for high-performance and scalable applications.

**Key Components:**
- **Asynchronous Channels:** `AsynchronousFileChannel`, `AsynchronousSocketChannel`.
- **Completion Handlers:** Callback interfaces that handle the result of asynchronous operations.

**Example: Asynchronous File Read**
```java
import java.nio.ByteBuffer;
import java.nio.channels.AsynchronousFileChannel;
import java.nio.file.Paths;
import java.nio.file.StandardOpenOption;
import java.util.concurrent.Future;

public class AsyncFileReadExample {
    public static void main(String[] args) {
        try {
            AsynchronousFileChannel asyncFileChannel = AsynchronousFileChannel.open(
                    Paths.get("asyncFile.txt"),
                    StandardOpenOption.READ
            );

            ByteBuffer buffer = ByteBuffer.allocate(1024);
            Future<Integer> result = asyncFileChannel.read(buffer, 0);

            // Do other tasks while reading

            while (!result.isDone()) {
                System.out.println("Reading file asynchronously...");
                Thread.sleep(100);
            }

            int bytesRead = result.get();
            System.out.println("Bytes Read: " + bytesRead);

            buffer.flip();
            byte[] data = new byte[buffer.limit()];
            buffer.get(data);
            System.out.println("Data: " + new String(data));

            asyncFileChannel.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
Example: Asynchronous File Write with CompletionHandler
```java
import java.nio.ByteBuffer;
import java.nio.channels.AsynchronousFileChannel;
import java.nio.channels.CompletionHandler;
import java.nio.file.Paths;
import java.nio.file.StandardOpenOption;

public class AsyncFileWriteExample {
    public static void main(String[] args) {
        try {
            AsynchronousFileChannel asyncFileChannel = AsynchronousFileChannel.open(
                    Paths.get("asyncWrite.txt"),
                    StandardOpenOption.WRITE,
                    StandardOpenOption.CREATE
            );

            String data = "Asynchronous Write Example";
            ByteBuffer buffer = ByteBuffer.wrap(data.getBytes());

            asyncFileChannel.write(buffer, 0, buffer, new CompletionHandler<Integer, ByteBuffer>() {
                @Override
                public void completed(Integer result, ByteBuffer attachment) {
                    System.out.println("Successfully wrote " + result + " bytes.");
                    try {
                        asyncFileChannel.close();
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                }

                @Override
                public void failed(Throwable exc, ByteBuffer attachment) {
                    System.out.println("Write operation failed.");
                    exc.printStackTrace();
                    try {
                        asyncFileChannel.close();
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                }
            });

            // Keep the main thread alive to see the completion
            Thread.sleep(1000);

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
**Advantages of Asynchronous I/O:**
- **Non-blocking Operations:** Threads are not held waiting for I/O operations to complete.
- **Scalability:** Efficiently handles a large number of I/O-bound tasks.
- **Performance:** Reduces context switching and improves resource utilization.

**Considerations:**
- **Complexity:** Asynchronous code can be more complex to write and debug.
- **Callback Hell:** Excessive nested callbacks can lead to unreadable code. Consider using `CompletableFuture` or other abstractions.
### Watch Service API
The Watch Service API allows Java applications to monitor changes to the file system, such as creation, deletion, or modification of files and directories. This is useful for applications that need to respond to file system events in real-time.

**Key Components:**
- **WatchService:** Registers and monitors file system events.
- **WatchKey:** Represents a registration with the WatchService.
- **WatchEvent:** Represents a specific event (e.g., ENTRY_CREATE, ENTRY_DELETE).

**Example: Monitoring a Directory for Changes**
```java
import java.nio.file.*;
import static java.nio.file.StandardWatchEventKinds.*;
import java.io.IOException;

public class WatchServiceExample {
    public static void main(String[] args) {
        Path path = Paths.get("watched_directory");

        try (WatchService watchService = FileSystems.getDefault().newWatchService()) {
            // Register the directory with the watch service for specific events
            path.register(watchService, ENTRY_CREATE, ENTRY_DELETE, ENTRY_MODIFY);

            System.out.println("Monitoring directory: " + path.toAbsolutePath());

            while (true) {
                WatchKey key = watchService.take(); // Wait for a key to be available

                for (WatchEvent<?> event : key.pollEvents()) {
                    WatchEvent.Kind<?> kind = event.kind();

                    // Context for directory entry event is the file name
                    WatchEvent<Path> ev = (WatchEvent<Path>) event;
                    Path fileName = ev.context();

                    System.out.println(kind.name() + ": " + fileName);
                }

                boolean valid = key.reset();
                if (!valid) {
                    break; // Exit loop if key is no longer valid
                }
            }

        } catch (IOException | InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```
**Explanation:**
- **Registering Events:** The directory is registered to listen for creation, deletion, and modification events.
- **Event Processing:** The application enters an infinite loop, waiting for events and processing them as they occur.
- **Key Reset:** After processing events, the key is reset to continue monitoring.

**Use Cases:**
- **Automatic File Processing:** Triggering actions when new files are added to a directory.
- **Real-Time Monitoring:** Keeping track of configuration changes or log file updates.
- **Security Auditing:** Monitoring unauthorized file access or modifications.
### Advanced File Attributes and Operations
NIO.2 provides extensive capabilities to interact with file attributes and perform advanced operations, enhancing control over the file system.

**Accessing File Attributes:
```java
import java.nio.file.*;
import java.nio.file.attribute.*;
import java.io.IOException;

public class FileAttributesExample {
    public static void main(String[] args) {
        Path path = Paths.get("example.txt");

        try {
            // Basic attributes
            BasicFileAttributes attrs = Files.readAttributes(path, BasicFileAttributes.class);
            System.out.println("Creation Time: " + attrs.creationTime());
            System.out.println("Last Modified Time: " + attrs.lastModifiedTime());
            System.out.println("Size: " + attrs.size() + " bytes");
            System.out.println("Is Directory: " + attrs.isDirectory());

            // View-specific attributes
            DosFileAttributes dosAttrs = Files.readAttributes(path, DosFileAttributes.class);
            System.out.println("Read Only: " + dosAttrs.isReadOnly());

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
Modifying File Attributes:
```java
import java.nio.file.*;
import java.nio.file.attribute.*;
import java.io.IOException;

public class ModifyFileAttributesExample {
    public static void main(String[] args) {
        Path path = Paths.get("example.txt");

        try {
            // Set file as read-only
            DosFileAttributeView dosView = Files.getFileAttributeView(path, DosFileAttributeView.class);
            if (dosView != null) {
                dosView.setReadOnly(true);
                System.out.println("File set to read-only.");
            } else {
                System.out.println("DOS attributes not supported.");
            }

            // Set last modified time
            FileTime newTime = FileTime.fromMillis(System.currentTimeMillis());
            Files.setLastModifiedTime(path, newTime);
            System.out.println("Last modified time updated.");

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
**Symbolic Links and Hard Links:**

- **Symbolic Links:** References to other files or directories.
    **Creating a Symbolic Link:**
```java
    Path target = Paths.get("target.txt");
Path link = Paths.get("link.txt");

try {
    Files.createSymbolicLink(link, target);
    System.out.println("Symbolic link created.");
} catch (UnsupportedOperationException e) {
    System.out.println("Symbolic links not supported.");
} catch (IOException | SecurityException e) {
    e.printStackTrace();
}
```
**Hard Links:** Additional directory entries for the same file.
**Creating a Hard Link:**
```java
Path original = Paths.get("original.txt");
Path hardLink = Paths.get("hardlink.txt");

try {
    Files.createLink(hardLink, original);
    System.out.println("Hard link created.");
} catch (UnsupportedOperationException e) {
    System.out.println("Hard links not supported.");
} catch (IOException | SecurityException e) {
    e.printStackTrace();
}
```
File Copy, Move, and Delete Operations:
```java
import java.nio.file.*;
import java.io.IOException;

public class FileOperationsExample {
    public static void main(String[] args) {
        Path source = Paths.get("source.txt");
        Path destination = Paths.get("destination.txt");

        try {
            // Copy file
            Files.copy(source, destination, StandardCopyOption.REPLACE_EXISTING);
            System.out.println("File copied successfully.");

            // Move file
            Path movedPath = Paths.get("moved.txt");
            Files.move(destination, movedPath, StandardCopyOption.REPLACE_EXISTING);
            System.out.println("File moved successfully.");

            // Delete file
            Files.delete(movedPath);
            System.out.println("File deleted successfully.");

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
**Atomic Operations:**
- **Atomic Moves and Deletes:** Ensure operations are completed fully or not at all, maintaining data integrity.

**Example: Atomic Move**
`Files.move(source, destination, StandardCopyOption.ATOMIC_MOVE);

**Caveats:**
- **File System Support:** Not all file systems support atomic operations.
- **Exception Handling:** Always handle potential `IOException` and related exceptions.
