---
tags:
  - FileManipulation
  - JavaIO
  - JSON
---

#### **What is org.json?**
The `org.json` library is a minimalistic JSON library for Java, offering basic functionalities to parse, generate, and manipulate JSON data. It's straightforward and easy to use for simple JSON operations.
#### **Key Features:**
- **JSONObject and JSONArray Classes:** Represent JSON objects and arrays.
- **Easy Parsing and Construction:** Simple methods to build and parse JSON data.
- **Lightweight:** Minimal dependencies and easy to integrate.
#### **Setting Up org.json:**
To use `org.json`, include the following Maven dependency in your `pom.xml`:
```xml
<dependencies>
    <!-- org.json -->
    <dependency>
        <groupId>org.json</groupId>
        <artifactId>json</artifactId>
        <version>20230618</version>
    </dependency>
</dependencies>
```
#### **Basic Usage:**
##### **1. Creating JSON Objects and Arrays:**
```java
import org.json.JSONArray;
import org.json.JSONObject;

public class OrgJsonExample {
    public static void main(String[] args) {
        // Create a JSONObject for a single student
        JSONObject student1 = new JSONObject();
        student1.put("id", "s1");
        student1.put("name", "Jane Doe");
        student1.put("major", "Computer Science");
        student1.put("gpa", 3.8);

        // Create another JSONObject
        JSONObject student2 = new JSONObject();
        student2.put("id", "s2");
        student2.put("name", "John Smith");
        student2.put("major", "Mathematics");
        student2.put("gpa", 3.6);

        // Create a JSONArray and add student objects
        JSONArray studentsArray = new JSONArray();
        studentsArray.put(student1);
        studentsArray.put(student2);

        // Create the root JSONObject
        JSONObject root = new JSONObject();
        root.put("students", studentsArray);

        // Print the JSON
        System.out.println(root.toString(4)); // Indented with 4 spaces
    }
}
```
```json
{
    "students": [
        {
            "id": "s1",
            "name": "Jane Doe",
            "major": "Computer Science",
            "gpa": 3.8
        },
        {
            "id": "s2",
            "name": "John Smith",
            "major": "Mathematics",
            "gpa": 3.6
        }
    ]
}
```
2. Parsing JSON Strings:
```JAVA
import org.json.JSONArray;
import org.json.JSONObject;

public class OrgJsonParseExample {
    public static void main(String[] args) {
        String jsonString = "{ \"students\": [ { \"id\": \"s1\", \"name\": \"Jane Doe\", \"major\": \"Computer Science\", \"gpa\": 3.8 }, { \"id\": \"s2\", \"name\": \"John Smith\", \"major\": \"Mathematics\", \"gpa\": 3.6 } ] }";

        // Parse the JSON string
        JSONObject root = new JSONObject(jsonString);
        JSONArray students = root.getJSONArray("students");

        // Iterate through the array
        for (int i = 0; i < students.length(); i++) {
            JSONObject student = students.getJSONObject(i);
            String id = student.getString("id");
            String name = student.getString("name");
            String major = student.getString("major");
            double gpa = student.getDouble("gpa");

            System.out.println("Student ID: " + id);
            System.out.println("Name: " + name);
            System.out.println("Major: " + major);
            System.out.println("GPA: " + gpa);
            System.out.println();
        }
    }
}
```
3. Modifying JSON DATA :
```java
import org.json.JSONArray;
import org.json.JSONObject;
import java.io.FileWriter;

public class OrgJsonModifyExample {
    public static void main(String[] args) {
        try {
            // Existing JSON data
            String jsonString = "{ \"students\": [ { \"id\": \"s1\", \"name\": \"Jane Doe\", \"major\": \"Computer Science\", \"gpa\": 3.8 }, { \"id\": \"s2\", \"name\": \"John Smith\", \"major\": \"Mathematics\", \"gpa\": 3.6 } ] }";

            // Parse the JSON string
            JSONObject root = new JSONObject(jsonString);
            JSONArray students = root.getJSONArray("students");

            // Add a new student
            JSONObject newStudent = new JSONObject();
            newStudent.put("id", "s3");
            newStudent.put("name", "Alice Johnson");
            newStudent.put("major", "Physics");
            newStudent.put("gpa", 3.9);
            students.put(newStudent);

            // Update an existing student's GPA
            for (int i = 0; i < students.length(); i++) {
                JSONObject student = students.getJSONObject(i);
                if (student.getString("id").equals("s1")) {
                    student.put("gpa", 4.0);
                }
            }

            // Write the updated JSON to a file
            FileWriter file = new FileWriter("students_updated.json");
            file.write(root.toString(4));
            file.flush();
            file.close();

            System.out.println("Updated JSON saved as students_updated.json");

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
Generated output : 
```json
{
    "students": [
        {
            "id": "s1",
            "name": "Jane Doe",
            "major": "Computer Science",
            "gpa": 4.0
        },
        {
            "id": "s2",
            "name": "John Smith",
            "major": "Mathematics",
            "gpa": 3.6
        },
        {
            "id": "s3",
            "name": "Alice Johnson",
            "major": "Physics",
            "gpa": 3.9
        }
    ]
}
```
