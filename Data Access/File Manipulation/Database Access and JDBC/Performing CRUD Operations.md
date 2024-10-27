Once connected to the database, you can perform CRUD operations using JDBC's `Statement` and `PreparedStatement` classes.
### 1. Using Statement for SQL Operations
The `Statement` interface is used to execute static SQL statements without parameters.
#### **Executing a SELECT Query:**
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class StatementSelectExample {
    public static void main(String[] args) {
        String jdbcURL = "jdbc:mysql://localhost:3306/companydb?useSSL=false&serverTimezone=UTC";
        String username = "admin";
        String password = "admin123";

        String sql = "SELECT employee_id, name, department FROM Employees";

        try (Connection connection = DriverManager.getConnection(jdbcURL, username, password);
             Statement statement = connection.createStatement();
             ResultSet resultSet = statement.executeQuery(sql)) {

            while (resultSet.next()) {
                int id = resultSet.getInt("employee_id");
                String name = resultSet.getString("name");
                String department = resultSet.getString("department");

                System.out.println("ID: " + id + ", Name: " + name + ", Department: " + department);
            }

        } catch (SQLException e) {
            System.out.println("Error executing SELECT query: " + e.getMessage());
        }
    }
}
```
**Explanation:**
- **Statement.executeQuery():** Executes the `SELECT` statement and returns a `ResultSet`.
- **ResultSet:** Iterates through the result set to retrieve data.
#### **Executing an INSERT Statement:
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

public class StatementInsertExample {
    public static void main(String[] args) {
        String jdbcURL = "jdbc:mysql://localhost:3306/companydb?useSSL=false&serverTimezone=UTC";
        String username = "admin";
        String password = "admin123";

        String sql = "INSERT INTO Employees (employee_id, name, department) VALUES (101, 'John Doe', 'Engineering')";

        try (Connection connection = DriverManager.getConnection(jdbcURL, username, password);
             Statement statement = connection.createStatement()) {

            int rowsInserted = statement.executeUpdate(sql);
            if (rowsInserted > 0) {
                System.out.println("A new employee was inserted successfully!");
            }

        } catch (SQLException e) {
            System.out.println("Error executing INSERT statement: " + e.getMessage());
        }
    }
}
```
**Explanation:**
- **Statement.executeUpdate():** Executes `INSERT`, `UPDATE`, or `DELETE` statements and returns the number of affected rows.
#### **Limitations of Statement:**
- **SQL Injection Vulnerability:** Since `Statement` executes SQL directly, it is susceptible to SQL injection if user inputs are concatenated into SQL strings.
- **Performance:** Cannot benefit from precompilation and caching of SQL statements.
### 2. Using PreparedStatement for Parameterized Queries
`PreparedStatement` allows you to execute parameterized SQL queries, enhancing security and performance.
#### **Advantages of PreparedStatement:**
- **Precompilation:** The SQL statement is precompiled, which can improve performance, especially for repeated executions.
- **Security:** Helps prevent SQL injection by separating SQL logic from data.
- **Convenience:** Handles escaping of special characters automatically.
#### **Example: Executing a Parameterized SELECT Query:**
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class PreparedStatementSelectExample {
    public static void main(String[] args) {
        String jdbcURL = "jdbc:mysql://localhost:3306/companydb?useSSL=false&serverTimezone=UTC";
        String username = "admin";
        String password = "admin123";

        String sql = "SELECT employee_id, name, department FROM Employees WHERE department = ?";

        try (Connection connection = DriverManager.getConnection(jdbcURL, username, password);
             PreparedStatement preparedStatement = connection.prepareStatement(sql)) {

            preparedStatement.setString(1, "Engineering"); // Set the parameter

            ResultSet resultSet = preparedStatement.executeQuery();

            while (resultSet.next()) {
                int id = resultSet.getInt("employee_id");
                String name = resultSet.getString("name");
                String department = resultSet.getString("department");

                System.out.println("ID: " + id + ", Name: " + name + ", Department: " + department);
            }

        } catch (SQLException e) {
            System.out.println("Error executing SELECT query: " + e.getMessage());
        }
    }
}
```
#### Example: Executing a Parameterized INSERT Statement:
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class PreparedStatementInsertExample {
    public static void main(String[] args) {
        String jdbcURL = "jdbc:mysql://localhost:3306/companydb?useSSL=false&serverTimezone=UTC";
        String username = "admin";
        String password = "admin123";

        String sql = "INSERT INTO Employees (employee_id, name, department) VALUES (?, ?, ?)";

        try (Connection connection = DriverManager.getConnection(jdbcURL, username, password);
             PreparedStatement preparedStatement = connection.prepareStatement(sql)) {

            preparedStatement.setInt(1, 102);
            preparedStatement.setString(2, "Jane Smith");
            preparedStatement.setString(3, "Marketing");

            int rowsInserted = preparedStatement.executeUpdate();
            if (rowsInserted > 0) {
                System.out.println("A new employee was inserted successfully!");
            }

        } catch (SQLException e) {
            System.out.println("Error executing INSERT statement: " + e.getMessage());
        }
    }
}
```
#### **Preventing SQL Injection:**
By using `PreparedStatement`, you ensure that user inputs are treated as data rather than executable code, effectively preventing SQL injection attacks.

**Vulnerable Code Using Statement:**
```java
String userInput = "Engineering'; DROP TABLE Employees; --";
String sql = "SELECT * FROM Employees WHERE department = '" + userInput + "'";
Statement statement = connection.createStatement();
ResultSet resultSet = statement.executeQuery(sql);
// This can lead to SQL injection
```
**Secure Code Using PreparedStatement:**
```java
String userInput = "Engineering'; DROP TABLE Employees; --";
String sql = "SELECT * FROM Employees WHERE department = ?";
PreparedStatement preparedStatement = connection.prepareStatement(sql);
preparedStatement.setString(1, userInput);
ResultSet resultSet = preparedStatement.executeQuery();
// SQL injection is prevented
```
### 3. Preventing SQL Injection
**SQL Injection** is a security vulnerability that allows an attacker to interfere with the queries an application makes to its database. It typically occurs when untrusted input is concatenated directly into SQL statements.
#### **Best Practices to Prevent SQL Injection:**
1. **Use PreparedStatement:**
    - Always use `PreparedStatement` or parameterized queries instead of `Statement`.
2. **Input Validation:**
    - Validate and sanitize all user inputs.
3. **Least Privilege:**
    - Use database accounts with the least privileges necessary.
4. **Stored Procedures:**
    - Utilize stored procedures that encapsulate SQL statements.
5. **ORM Frameworks:**
    - Use Object-Relational Mapping (ORM) frameworks like Hibernate that handle SQL generation securely.
#### **Example: Secure Query with PreparedStatement**
```java
String usernameInput = "johndoe";
String passwordInput = "securepassword";

String sql = "SELECT * FROM Users WHERE username = ? AND password = ?";
PreparedStatement preparedStatement = connection.prepareStatement(sql);
preparedStatement.setString(1, usernameInput);
preparedStatement.setString(2, passwordInput);
ResultSet resultSet = preparedStatement.executeQuery();
```
**Explanation:**
- **Parameter Binding:** `setString()` methods bind user inputs to the SQL statement as parameters, ensuring they're treated as data.
- **No Concatenation:** Avoids concatenating user inputs directly into SQL statements.