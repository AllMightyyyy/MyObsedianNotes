---
tags:
  - FileManipulation
  - JavaIO
  - JDBC
---

### Purpose and Architecture of JDBC
**JDBC (Java Database Connectivity)** is an API in Java that defines how a client may access a database. It provides methods to query and update data in a database, and is oriented towards relational databases.
#### **Purpose of JDBC:**
- **Standardized Access:** Offers a standard interface for Java applications to interact with various databases.
- **Database Independence:** Allows switching between different databases with minimal code changes.
- **SQL Execution:** Facilitates the execution of SQL statements and retrieval of results.
#### **JDBC Architecture:**
The JDBC architecture consists of two main layers:
1. **JDBC API:** Provides the interfaces and classes that Java applications use to interact with the database.
2. **JDBC Drivers:** Implement the JDBC interfaces for specific databases, translating Java calls into database-specific calls.
3. 
**Components:**
- **DriverManager:** Manages a list of database drivers and establishes connections.
- **Connection:** Represents a connection to a specific database.
- **Statement:** Used to execute SQL queries.
- **PreparedStatement:** A precompiled version of `Statement` that can accept parameters.
- **ResultSet:** Represents the result of a `SELECT` query.
### Core JDBC Classes
#### **1. DriverManager**
- **Purpose:** Manages JDBC drivers and establishes connections to databases.
- **Key Methods:**
    - `getConnection(String url, String user, String password)`: Establishes a connection to the specified database.
#### **2. Connection**
- **Purpose:** Represents a session with a specific database.
- **Key Methods:**
    - `createStatement()`: Creates a `Statement` object.
    - `prepareStatement(String sql)`: Creates a `PreparedStatement` object.
    - `setAutoCommit(boolean autoCommit)`: Sets the auto-commit mode.
    - `commit()`: Commits the current transaction.
    - `rollback()`: Rolls back the current transaction.
    - `close()`: Closes the connection.
#### **3. Statement**
- **Purpose:** Executes static SQL statements and returns results.
- **Key Methods:**
    - `executeQuery(String sql)`: Executes a `SELECT` statement and returns a `ResultSet`.
    - `executeUpdate(String sql)`: Executes `INSERT`, `UPDATE`, or `DELETE` statements and returns the number of affected rows.
    - `close()`: Closes the statement.
#### **4. PreparedStatement**
- **Purpose:** Executes precompiled SQL statements with or without parameters.
- **Key Methods:**
    - `setInt(int parameterIndex, int x)`: Sets an `int` parameter.
    - `setString(int parameterIndex, String x)`: Sets a `String` parameter.
    - `executeQuery()`: Executes a `SELECT` statement.
    - `executeUpdate()`: Executes an `INSERT`, `UPDATE`, or `DELETE` statement.
    - `close()`: Closes the prepared statement.
#### **5. ResultSet**
- **Purpose:** Represents the result of a `SELECT` query.
- **Key Methods:**
    - `next()`: Moves the cursor to the next row.
    - `getInt(String columnLabel)`: Retrieves an `int` from the specified column.
    - `getString(String columnLabel)`: Retrieves a `String` from the specified column.
    - `close()`: Closes the `ResultSet`.
#### **6. SQLException**
- **Purpose:** Thrown when a database access error occurs or other errors related to SQL operations.
- **Key Methods:**
    - `getMessage()`: Returns a description of the error.
    - `getSQLState()`: Returns the SQLState code.
    - `getErrorCode()`: Returns the vendor-specific error code.
    - `getNextException()`: Retrieves the next `SQLException` in the chain.
## Connecting to Databases

### 1. Setting Up Database Drivers
To interact with a database using JDBC, you need the appropriate JDBC driver for that database. Each database vendor provides its own driver.
#### **Common JDBC Drivers:**
- **MySQL:** Connector/J
- **PostgreSQL:** PostgreSQL JDBC Driver
- **H2:** H2 Database Engine
- **SQLite:** SQLite JDBC Driver
- **Oracle:** Oracle JDBC Driver
#### **Adding JDBC Drivers to Your Project:**
If you're using Maven, add the driver as a dependency in your `pom.xml`. For example, to add MySQL Connector/J:
``` XML
<dependencies>
    <!-- MySQL Connector/J -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.32</version>
    </dependency>
</dependencies>
```
### 2. Understanding Connection Strings
A **connection string** (also known as a **JDBC URL**) specifies the database to connect to and provides necessary parameters. Its format varies depending on the database.
#### **General Format:
`jdbc:subprotocol://host:port/database?property1=value1&property2=value2`
#### **Examples:**

- **MySQL:** `jdbc:mysql://localhost:3306/mydatabase?useSSL=false&serverTimezone=UTC`
- **PostgreSQL:** `jdbc:postgresql://localhost:5432/mydatabase`
- **H2 (Embedded):**`jdbc:h2:~/test`
- SQLite:**`jdbc:sqlite:sample.db
`
#### **Components:**
- **jdbc:** The protocol.
- **subprotocol:** The database type (e.g., `mysql`, `postgresql`).
- **host:** The database server's hostname or IP address.
- **port:** The port number the database server is listening on.
- **database:** The specific database to connect to.
- **properties:** Optional parameters for the connection.
### 3. Establishing Connections
Using JDBC's `DriverManager`, you can establish a connection to the database.
#### **Basic Connection Example:**
```Java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class DatabaseConnectionExample {
    public static void main(String[] args) {
        String jdbcURL = "jdbc:mysql://localhost:3306/mydatabase?useSSL=false&serverTimezone=UTC";
        String username = "root";
        String password = "password";

        try {
            // Establish the connection
            Connection connection = DriverManager.getConnection(jdbcURL, username, password);
            System.out.println("Connected to the database successfully.");

            // Perform database operations

            // Close the connection
            connection.close();
            System.out.println("Connection closed.");
        } catch (SQLException e) {
            System.out.println("Error connecting to the database: " + e.getMessage());
        }
    }
}
```
**Explanation:**
- **DriverManager.getConnection():** Attempts to establish a connection using the provided JDBC URL, username, and password.
- **Exception Handling:** Catches `SQLException` to handle any issues during connection.
- **Resource Management:** Closes the connection after operations are complete.
#### **Using try-with-resources:**
To ensure that the `Connection` is closed automatically, use try-with-resources.
```Java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class TryWithResourcesExample {
    public static void main(String[] args) {
        String jdbcURL = "jdbc:mysql://localhost:3306/mydatabase?useSSL=false&serverTimezone=UTC";
        String username = "root";
        String password = "password";

        try (Connection connection = DriverManager.getConnection(jdbcURL, username, password)) {
            System.out.println("Connected to the database successfully.");

            // Perform database operations

        } catch (SQLException e) {
            System.out.println("Error connecting to the database: " + e.getMessage());
        }
        // Connection is automatically closed here
    }
}
```
**Advantages:**
- **Automatic Resource Management:** Ensures that the `Connection` is closed even if an exception occurs.
- **Cleaner Code:** Reduces boilerplate code for closing resources.
#### **Practice Exercise: Establishing a Connection**
**Task:** Write a Java program that connects to a PostgreSQL database named `companydb` running on `localhost` at port `5432` with username `admin` and password `admin123`. Upon successful connection, print a confirmation message and then close the connection.
**Sample Code:**
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class PostgreSQLConnectionExample {
    public static void main(String[] args) {
        String jdbcURL = "jdbc:postgresql://localhost:5432/companydb";
        String username = "admin";
        String password = "admin123";

        try (Connection connection = DriverManager.getConnection(jdbcURL, username, password)) {
            System.out.println("Connected to PostgreSQL database successfully.");
        } catch (SQLException e) {
            System.out.println("Error connecting to PostgreSQL database: " + e.getMessage());
        }
    }
}
```