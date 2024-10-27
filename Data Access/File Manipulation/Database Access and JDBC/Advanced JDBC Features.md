it's essential to explore advanced features that enhance performance, ensure data integrity, and provide deeper insights into the database.

### 5.1. Batch Updates

**Batch updates** allow you to execute multiple SQL statements as a single batch, which can significantly improve performance by reducing the number of database round-trips.
#### **When to Use Batch Updates:**
- **Bulk Operations:** Inserting, updating, or deleting large numbers of records.
- **Performance Optimization:** Minimizing network latency and increasing throughput.
#### **Example: Batch Insert Using PreparedStatement**
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class BatchInsertExample {
    public static void main(String[] args) {
        String jdbcURL = "jdbc:mysql://localhost:3306/companydb?useSSL=false&serverTimezone=UTC";
        String username = "admin";
        String password = "admin123";

        String sql = "INSERT INTO Employees (employee_id, name, department) VALUES (?, ?, ?)";

        try (Connection connection = DriverManager.getConnection(jdbcURL, username, password);
             PreparedStatement preparedStatement = connection.prepareStatement(sql)) {

            connection.setAutoCommit(false); // Disable auto-commit

            for (int i = 103; i <= 112; i++) {
                preparedStatement.setInt(1, i);
                preparedStatement.setString(2, "Employee " + i);
                preparedStatement.setString(3, "Department " + (i % 5));

                preparedStatement.addBatch();

                if (i % 10 == 0) { // Execute batch every 10 inserts
                    preparedStatement.executeBatch();
                }
            }

            preparedStatement.executeBatch(); // Execute remaining batch
            connection.commit(); // Commit transaction

            System.out.println("Batch insert completed successfully.");

        } catch (SQLException e) {
            e.printStackTrace();
            // Handle rollback in case of error
        }
    }
}
```
**Explanation:**

- **setAutoCommit(false):** Disables auto-commit to manage transactions manually.
- **addBatch():** Adds the SQL statement to the batch.
- **executeBatch():** Executes the batch of SQL statements.
- **commit():** Commits the transaction after successful execution.

### 5.2. Managing Transactions
**Transactions** ensure that a sequence of operations either all succeed or all fail, maintaining data integrity.
#### **ACID Properties:**
1. **Atomicity:** Ensures that all operations within the transaction are completed; if not, the transaction is aborted.
2. **Consistency:** Ensures that the database remains in a consistent state before and after the transaction.
3. **Isolation:** Ensures that concurrent transactions do not interfere with each other.
4. **Durability:** Ensures that once a transaction is committed, it remains so, even in the event of a system failure.
#### **Transaction Control in JDBC:**
- **Auto-commit Mode:** By default, each SQL statement is committed immediately. Disabling auto-commit allows grouping multiple statements into a single transaction.
- **Commit:** Permanently saves changes made during the transaction.
- **Rollback:** Undoes all changes made during the transaction if an error occurs.
#### **Example: Managing Transactions**
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class TransactionExample {
    public static void main(String[] args) {
        String jdbcURL = "jdbc:mysql://localhost:3306/companydb?useSSL=false&serverTimezone=UTC";
        String username = "admin";
        String password = "admin123";

        String insertSQL = "INSERT INTO Employees (employee_id, name, department) VALUES (?, ?, ?)";
        String updateSQL = "UPDATE Departments SET budget = budget - ? WHERE department_name = ?";

        try (Connection connection = DriverManager.getConnection(jdbcURL, username, password);
             PreparedStatement insertStatement = connection.prepareStatement(insertSQL);
             PreparedStatement updateStatement = connection.prepareStatement(updateSQL)) {

            // Disable auto-commit
            connection.setAutoCommit(false);

            try {
                // Insert a new employee
                insertStatement.setInt(1, 113);
                insertStatement.setString(2, "Diana Prince");
                insertStatement.setString(3, "HR");
                insertStatement.executeUpdate();

                // Update department budget
                updateStatement.setDouble(1, 5000.0);
                updateStatement.setString(2, "HR");
                updateStatement.executeUpdate();

                // Commit the transaction
                connection.commit();
                System.out.println("Transaction committed successfully.");

            } catch (SQLException e) {
                // Rollback the transaction in case of error
                connection.rollback();
                System.out.println("Transaction rolled back due to an error.");
                e.printStackTrace();
            }

        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```
**Explanation:**
- **setAutoCommit(false):** Begins a transaction.
- **Commit:** Saves both the `INSERT` and `UPDATE` operations.
- **Rollback:** Reverts both operations if any exception occurs during the transaction.
### 5.3. Connection Pooling
**Connection Pooling** involves maintaining a pool of database connections that can be reused, reducing the overhead of establishing a connection each time one is needed.
#### **Benefits:**
- **Performance Improvement:** Reusing connections reduces latency.
- **Resource Management:** Limits the number of active connections, preventing resource exhaustion.
- **Scalability:** Supports higher levels of concurrency.
#### **Popular Connection Pooling Libraries:**
- **HikariCP:** Known for high performance and low overhead.
- **Apache DBCP (Database Connection Pool):** Part of the Apache Commons project.
- **C3P0:** Offers robust configuration options.
#### **Example: Using HikariCP**
1. **Add HikariCP Dependency (Maven):**
```xml
<dependencies>
    <!-- HikariCP -->
    <dependency>
        <groupId>com.zaxxer</groupId>
        <artifactId>HikariCP</artifactId>
        <version>5.0.1</version>
    </dependency>
    
    <!-- MySQL Connector/J -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.32</version>
    </dependency>
</dependencies>
```
```java
import com.zaxxer.hikari.HikariConfig;
import com.zaxxer.hikari.HikariDataSource;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class HikariCPExample {
    public static void main(String[] args) {
        // Configure HikariCP
        HikariConfig config = new HikariConfig();
        config.setJdbcUrl("jdbc:mysql://localhost:3306/companydb?useSSL=false&serverTimezone=UTC");
        config.setUsername("admin");
        config.setPassword("admin123");
        config.setMaximumPoolSize(10);
        config.setMinimumIdle(2);
        config.setIdleTimeout(30000);
        config.setConnectionTimeout(30000);
        config.setPoolName("MyHikariCP");

        HikariDataSource dataSource = new HikariDataSource(config);

        String sql = "SELECT employee_id, name, department FROM Employees";

        try (Connection connection = dataSource.getConnection();
             PreparedStatement preparedStatement = connection.prepareStatement(sql);
             ResultSet resultSet = preparedStatement.executeQuery()) {

            while (resultSet.next()) {
                int id = resultSet.getInt("employee_id");
                String name = resultSet.getString("name");
                String department = resultSet.getString("department");

                System.out.println("ID: " + id + ", Name: " + name + ", Department: " + department);
            }

        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            // Close the data source when done
            dataSource.close();
        }
    }
}
```
**Explanation:**
- **HikariConfig:** Configures the connection pool settings.
- **HikariDataSource:** Manages the pool of connections.
- **Connection Reuse:** Retrieves connections from the pool instead of creating new ones.
- **Resource Management:** Closes the `HikariDataSource` to release resources.
#### **Best Practices for Connection Pooling:**
1. **Configure Pool Size Appropriately:** Balance between available database resources and application demand.
2. **Set Timeouts:** Define connection and idle timeouts to manage stale or unused connections.
3. **Monitor Pool Usage:** Use monitoring tools to observe pool performance and adjust configurations as needed.
4. **Handle Exceptions Gracefully:** Ensure that exceptions during connection retrieval are handled to prevent application crashes.
### 5.4. Retrieving Metadata
**Metadata** provides information about the database, such as its structure, tables, columns, and supported features.
#### **Types of Metadata:**
- **DatabaseMetaData:** Provides information about the database as a whole.
- **ResultSetMetaData:** Provides information about the columns in a `ResultSet`.
#### **Example: Using DatabaseMetaData**
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.DatabaseMetaData;
import java.sql.ResultSet;
import java.sql.SQLException;

public class DatabaseMetaDataExample {
    public static void main(String[] args) {
        String jdbcURL = "jdbc:mysql://localhost:3306/companydb?useSSL=false&serverTimezone=UTC";
        String username = "admin";
        String password = "admin123";

        try (Connection connection = DriverManager.getConnection(jdbcURL, username, password)) {
            DatabaseMetaData dbMetaData = connection.getMetaData();

            System.out.println("Database Product Name: " + dbMetaData.getDatabaseProductName());
            System.out.println("Database Product Version: " + dbMetaData.getDatabaseProductVersion());
            System.out.println("JDBC Driver Name: " + dbMetaData.getDriverName());
            System.out.println("JDBC Driver Version: " + dbMetaData.getDriverVersion());

            // List all tables
            ResultSet tables = dbMetaData.getTables(null, null, "%", new String[] { "TABLE" });
            System.out.println("\nList of Tables:");
            while (tables.next()) {
                System.out.println(tables.getString("TABLE_NAME"));
            }

        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}

```
**Example: Using ResultSetMetaData**
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.sql.ResultSetMetaData;

public class ResultSetMetaDataExample {
    public static void main(String[] args) {
        String jdbcURL = "jdbc:mysql://localhost:3306/companydb?useSSL=false&serverTimezone=UTC";
        String username = "admin";
        String password = "admin123";

        String sql = "SELECT employee_id, name, department FROM Employees";

        try (Connection connection = DriverManager.getConnection(jdbcURL, username, password);
             Statement statement = connection.createStatement();
             ResultSet resultSet = statement.executeQuery(sql)) {

            ResultSetMetaData rsMetaData = resultSet.getMetaData();
            int columnCount = rsMetaData.getColumnCount();

            System.out.println("Number of Columns: " + columnCount);
            for (int i = 1; i <= columnCount; i++) {
                System.out.println("Column " + i + ": " + rsMetaData.getColumnName(i) + " - " + rsMetaData.getColumnTypeName(i));
            }

        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```