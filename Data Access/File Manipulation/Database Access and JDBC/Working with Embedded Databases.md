**Embedded databases** are lightweight, self-contained database engines that run within the same process as the application. They are ideal for development, testing, and small-scale applications.
### 6.1. Introduction to Embedded Databases
**Advantages:**
- **Zero Configuration:** No need to install or manage a separate database server.
- **Portability:** The database resides within the application, making deployment easier.
- **Performance:** Typically faster for small to medium-sized datasets.
- **Resource Efficiency:** Consumes fewer system resources compared to server-based databases.
- 
**Popular Embedded Databases:**
- **H2 Database Engine**
- **Apache Derby**
- **SQLite**
- **HSQLDB (HyperSQL Database)**
### 6.2. Setting Up H2 Database
**H2** is a fast, open-source, and lightweight embedded database written in Java. It can also run in server mode.
#### **Adding H2 Dependency (Maven):**
```xml
<dependencies>
    <!-- H2 Database -->
    <dependency>
        <groupId>com.h2database</groupId>
        <artifactId>h2</artifactId>
        <version>2.2.220</version>
    </dependency>
</dependencies>
```
#### **Connecting to H2 Database:**
H2 supports both embedded and server modes. For embedded mode, the database is stored in the local file system or in memory.

**Example: Connecting to an H2 Embedded Database (File-based):**
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class H2ConnectionExample {
    public static void main(String[] args) {
        String jdbcURL = "jdbc:h2:~/testdb"; // Stores the database in the user's home directory
        String username = "sa";
        String password = ""; // Default password for H2

        try (Connection connection = DriverManager.getConnection(jdbcURL, username, password)) {
            if (connection != null) {
                System.out.println("Connected to H2 database successfully.");
            }
        } catch (SQLException e) {
            System.out.println("Error connecting to H2 database: " + e.getMessage());
        }
    }
}
```
**Example: Connecting to an H2 In-Memory Database:**
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class H2InMemoryConnectionExample {
    public static void main(String[] args) {
        String jdbcURL = "jdbc:h2:mem:testdb"; // In-memory database
        String username = "sa";
        String password = "";

        try (Connection connection = DriverManager.getConnection(jdbcURL, username, password)) {
            if (connection != null) {
                System.out.println("Connected to H2 in-memory database successfully.");
            }
        } catch (SQLException e) {
            System.out.println("Error connecting to H2 in-memory database: " + e.getMessage());
        }
    }
}
```
#### **Executing SQL Commands in H2:**
Once connected, you can perform CRUD operations just like with any other JDBC-compliant database.

**Example: Creating and Populating a Table in H2:**
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

public class H2CreateTableExample {
    public static void main(String[] args) {
        String jdbcURL = "jdbc:h2:~/testdb";
        String username = "sa";
        String password = "";

        String createTableSQL = "CREATE TABLE IF NOT EXISTS Employees (" +
                                "employee_id INT PRIMARY KEY," +
                                "name VARCHAR(100) NOT NULL," +
                                "department VARCHAR(100)" +
                                ");";

        String insertDataSQL = "INSERT INTO Employees (employee_id, name, department) VALUES " +
                               "(201, 'Alice Wonderland', 'Engineering')," +
                               "(202, 'Bob Builder', 'Construction')," +
                               "(203, 'Charlie Chaplin', 'Marketing');";

        try (Connection connection = DriverManager.getConnection(jdbcURL, username, password);
             Statement statement = connection.createStatement()) {

            // Create table
            statement.execute(createTableSQL);
            System.out.println("Employees table created successfully.");

            // Insert data
            int rowsInserted = statement.executeUpdate(insertDataSQL);
            System.out.println(rowsInserted + " records inserted successfully.");

        } catch (SQLException e) {
            System.out.println("Error executing SQL commands: " + e.getMessage());
        }
    }
}
```
### Using Apache Derby and SQLite
**Apache Derby** and **SQLite** are other popular embedded databases that offer different features and capabilities.
#### **Apache Derby:**
- **Features:**
    
    - Pure Java implementation.
    - Can run in embedded and client-server modes.
    - Small footprint.
- **Adding Derby Dependency (Maven):**
```xml
<dependencies>
    <!-- Apache Derby -->
    <dependency>
        <groupId>org.apache.derby</groupId>
        <artifactId>derby</artifactId>
        <version>10.15.2.0</version>
    </dependency>
</dependencies>
```
Connecting to Derby Embedded Database:
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class DerbyConnectionExample {
    public static void main(String[] args) {
        String jdbcURL = "jdbc:derby:myderbydb;create=true"; // Creates database if it doesn't exist
        String username = "";
        String password = "";

        try (Connection connection = DriverManager.getConnection(jdbcURL, username, password)) {
            if (connection != null) {
                System.out.println("Connected to Apache Derby embedded database successfully.");
            }
        } catch (SQLException e) {
            System.out.println("Error connecting to Apache Derby database: " + e.getMessage());
        }
    }
}
```
#### **SQLite:**
- **Features:**
    - Lightweight and fast.
    - Serverless, self-contained, zero-configuration.
    - Public domain, making it free for any use.
- **Adding SQLite JDBC Dependency (Maven):**
```xml
<dependencies>
    <!-- SQLite JDBC -->
    <dependency>
        <groupId>org.xerial</groupId>
        <artifactId>sqlite-jdbc</artifactId>
        <version>3.42.0.0</version>
    </dependency>
</dependencies>
```
Connecting to SQLite Database:
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class SQLiteConnectionExample {
    public static void main(String[] args) {
        String jdbcURL = "jdbc:sqlite:sample.db"; // Creates sample.db if it doesn't exist

        try (Connection connection = DriverManager.getConnection(jdbcURL)) {
            if (connection != null) {
                System.out.println("Connected to SQLite database successfully.");
            }
        } catch (SQLException e) {
            System.out.println("Error connecting to SQLite database: " + e.getMessage());
        }
    }
}
```

**Explanation:**
- **Connection Strings:** Vary based on the embedded database being used.
- **Database Creation:** Embedded databases often allow automatic creation of databases if they do not exist.