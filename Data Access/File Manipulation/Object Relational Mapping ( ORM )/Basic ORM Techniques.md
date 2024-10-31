---
tags:
  - FileManipulation
  - JavaIO
  - ORM
---

Before delving into ORM frameworks like Hibernate, it's essential to understand manual mapping strategies. This foundational knowledge will help you appreciate the abstractions and conveniences provided by ORM tools.
### Manual Mapping Strategies
Manual mapping involves writing JDBC code to map Java objects to database tables and vice versa. This process includes:

- **Creating Java Classes:**
    - Define classes that represent database entities.
- **Establishing Database Connections:**
    - Use JDBC to connect to the database.
- **Executing SQL Queries:**
    - Perform CRUD operations by writing SQL statements.
- **Mapping ResultSets to Objects:**
    - Convert `ResultSet` data into Java objects.
#### **Example: Manual Mapping**
Suppose we have a `User` table in the database:
```sql
CREATE TABLE User (
    id INT PRIMARY KEY,
    username VARCHAR(50),
    email VARCHAR(100),
    age INT
);
```
Java Class:
```java
public class User {
    private int id;
    private String username;
    private String email;
    private int age;

    // Constructors
    public User() {}

    public User(int id, String username, String email, int age) {
        this.id = id;
        this.username = username;
        this.email = email;
        this.age = age;
    }

    // Getters and Setters
    // ... (omitted for brevity)
}
```
JDBC Code to Retrieve Users:
```java
import java.sql.*;
import java.util.ArrayList;
import java.util.List;

public class ManualORMExample {
    private static final String URL = "jdbc:mysql://localhost:3306/your_database";
    private static final String USER = "your_username";
    private static final String PASSWORD = "your_password";

    public List<User> getAllUsers() {
        List<User> users = new ArrayList<>();
        String query = "SELECT * FROM User";

        try (Connection conn = DriverManager.getConnection(URL, USER, PASSWORD);
             Statement stmt = conn.createStatement();
             ResultSet rs = stmt.executeQuery(query)) {

            while (rs.next()) {
                User user = new User(
                    rs.getInt("id"),
                    rs.getString("username"),
                    rs.getString("email"),
                    rs.getInt("age")
                );
                users.add(user);
            }

        } catch (SQLException e) {
            e.printStackTrace();
        }

        return users;
    }

    public static void main(String[] args) {
        ManualORMExample example = new ManualORMExample();
        List<User> users = example.getAllUsers();

        for (User user : users) {
            System.out.println(user.getUsername() + " - " + user.getEmail());
        }
    }
}
```
**Limitations of Manual Mapping:**
- **Boilerplate Code:** Requires repetitive code for CRUD operations.
- **Maintenance Overhead:** Changes in the database schema necessitate corresponding changes in Java classes and SQL statements.
- **Error-Prone:** Increased likelihood of bugs due to manual handling of data mapping.

### Creating Java Classes for Database Tables

When manually mapping, it's crucial to design Java classes that accurately represent the database schema. This includes:
- **Field Naming:** Use consistent naming conventions between Java fields and database columns.
- **Data Types:** Ensure Java data types align with SQL data types.
- **Constructors:** Provide constructors for easy object instantiation.
- **Getters and Setters:** Facilitate access and modification of fields.
- 
**Best Practices:**
- **Use Meaningful Names:** Class and field names should clearly represent their purpose.
- **Encapsulation:** Keep fields private and provide public getters and setters.
- **Override `toString()`:** For easier debugging and logging.
- **Implement `equals()` and `hashCode()`:** To compare objects based on their fields, not references.

**Example: Enhanced `User` Class**
```java
public class User {
    private int id;
    private String username;
    private String email;
    private int age;

    // Constructors
    public User() {}

    public User(int id, String username, String email, int age) {
        this.id = id;
        this.username = username;
        this.email = email;
        this.age = age;
    }

    // Getters and Setters
    public int getId() { return id; }
    public void setId(int id) { this.id = id; }

    public String getUsername() { return username; }
    public void setUsername(String username) { this.username = username; }

    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }

    public int getAge() { return age; }
    public void setAge(int age) { this.age = age; }

    // Override toString for better readability
    @Override
    public String toString() {
        return "User{id=" + id + ", username='" + username + "', email='" + email + "', age=" + age + "}";
    }

    // Override equals and hashCode for object comparison
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;

        User user = (User) o;

        if (id != user.id) return false;
        if (age != user.age) return false;
        if (!username.equals(user.username)) return false;
        return email.equals(user.email);
    }

    @Override
    public int hashCode() {
        int result = id;
        result = 31 * result + username.hashCode();
        result = 31 * result + email.hashCode();
        result = 31 * result + age;
        return result;
    }
}
```
