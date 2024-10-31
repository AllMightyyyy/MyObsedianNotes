---
tags:
  - FileManipulation
  - JavaIO
  - JSON
---

#### **What is Jackson?**
Jackson is a high-performance JSON processor for Java. It's widely used for serializing Java objects to JSON and deserializing JSON back to Java objects. Jackson is known for its flexibility and extensive feature set.
#### **Key Features:**
- **Data Binding:** Convert Java objects to JSON and vice versa.
- **Streaming API:** Process JSON content incrementally.
- **Tree Model:** Represent JSON as a tree structure.
- **Annotations:** Control serialization/deserialization behavior.
#### **Setting Up Jackson:**
To use Jackson, include the following Maven dependencies in your `pom.xml`:
```xml
<dependencies>
    <!-- Jackson Databind -->
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>2.15.2</version>
    </dependency>
</dependencies>
```
#### **Basic Usage:**
##### **1. POJO (Plain Old Java Object) Example:**
```Java
public class Student {
    private String id;
    private String name;
    private String major;
    private double gpa;

    // Constructors
    public Student() {}
    
    public Student(String id, String name, String major, double gpa) {
        this.id = id;
        this.name = name;
        this.major = major;
        this.gpa = gpa;
    }

    // Getters and Setters
    // ... (omitted for brevity)
}
```
2. **Serializing Java Objects to JSON:**
```Java
import com.fasterxml.jackson.databind.ObjectMapper;

public class JacksonSerializeExample {
    public static void main(String[] args) {
        try {
            ObjectMapper mapper = new ObjectMapper();
            
            // Create a Student object
            Student student = new Student("s1", "Jane Doe", "Computer Science", 3.8);
            
            // Convert Java object to JSON string
            String jsonString = mapper.writeValueAsString(student);
            System.out.println("Serialized JSON:");
            System.out.println(jsonString);
            
            // Write JSON to file
            mapper.writeValue(new File("student.json"), student);
            System.out.println("JSON file created successfully.");
            
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
**Output** :
```JSON
{
    "id":"s1",
    "name":"Jane Doe",
    "major":"Computer Science",
    "gpa":3.8
}
```
**3.Deserializing JSON to JAVA Objects : **
```java
import com.fasterxml.jackson.databind.ObjectMapper;
import java.io.File;

public class JacksonDeserializeExample {
    public static void main(String[] args) {
        try {
            ObjectMapper mapper = new ObjectMapper();
            
            // Read JSON from file and convert to Student object
            Student student = mapper.readValue(new File("student.json"), Student.class);
            
            // Display Student details
            System.out.println("Deserialized Student:");
            System.out.println("ID: " + student.getId());
            System.out.println("Name: " + student.getName());
            System.out.println("Major: " + student.getMajor());
            System.out.println("GPA: " + student.getGpa());
            
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
**Output :**
```
Deserialized Student:
ID: s1
Name: Jane Doe
Major: Computer Science
GPA: 3.8
```
#### **Advanced Jackson Features:**
##### **Custom Serializers and Deserializers:**
Sometimes, you need to customize how objects are serialized or deserialized. Jackson allows you to create custom serializers/deserializers.
**Example: Custom Serializer for GPA (e.g., format to one decimal place):**
```java
import com.fasterxml.jackson.core.JsonGenerator;
import com.fasterxml.jackson.databind.SerializerProvider;
import com.fasterxml.jackson.databind.ser.std.StdSerializer;
import java.io.IOException;

public class GPASerializer extends StdSerializer<Double> {

    public GPASerializer() {
        this(null);
    }

    public GPASerializer(Class<Double> t) {
        super(t);
    }

    @Override
    public void serialize(Double gpa, JsonGenerator gen, SerializerProvider provider) throws IOException {
        gen.writeNumber(String.format("%.1f", gpa));
    }
}
```
Annotate the `gpa` field in `Student` class:
```java
import com.fasterxml.jackson.databind.annotation.JsonSerialize;

public class Student {
    // Other fields...

    @JsonSerialize(using = GPASerializer.class)
    private double gpa;

    // Getters and Setters
    // ...
}
```
##### **Using Annotations:**
- **`@JsonIgnore`:** Exclude a field from serialization/deserialization.
- **`@JsonProperty`:** Define a different name for a JSON property.
- **`@JsonFormat`:** Specify format for dates or other data types.
```java
import com.fasterxml.jackson.annotation.JsonIgnore;
import com.fasterxml.jackson.annotation.JsonProperty;
import com.fasterxml.jackson.annotation.JsonFormat;
import java.util.Date;

public class Student {
    private String id;
    private String name;
    private String major;
    private double gpa;

    @JsonIgnore
    private String password;

    @JsonProperty("enrollmentDate")
    @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd")
    private Date enrollmentDate;

    // Constructors, Getters, and Setters
    // ...
}
```