---
tags:
  - FileManipulation
  - JavaIO
  - JSON
---

#### **What is Gson?**
Gson is a Java library developed by Google for converting Java objects to JSON and vice versa. It's known for its simplicity and ease of use.
#### **Key Features:**
- **Serialization and Deserialization:** Easy conversion between Java objects and JSON.
- **Support for Generics:** Handles complex generic types.
- **Custom Type Adapters:** Customize the serialization/deserialization process.
- **Exclusion Strategies:** Control which fields are included or excluded.
#### **Setting Up Gson:**
To use Gson, include the following Maven dependency in your `pom.xml`:

```xml
<dependencies>
    <!-- Gson -->
    <dependency>
        <groupId>com.google.code.gson</groupId>
        <artifactId>gson</artifactId>
        <version>2.10.1</version>
    </dependency>
</dependencies>
```
#### **Basic Usage:**
##### **1. POJO Example:**
```java
public class Book {
    private String isbn;
    private String title;
    private String author;
    private double price;
    private String publishDate;

    // Constructors
    public Book() {}
    
    public Book(String isbn, String title, String author, double price, String publishDate) {
        this.isbn = isbn;
        this.title = title;
        this.author = author;
        this.price = price;
        this.publishDate = publishDate;
    }

    // Getters and Setters
    // ... (omitted for brevity)
}
```
2. Serializing Java Objects to JSON:
```java
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import java.io.FileWriter;
import java.util.Arrays;
import java.util.List;

public class GsonSerializeExample {
    public static void main(String[] args) {
        try {
            Gson gson = new GsonBuilder().setPrettyPrinting().create();
            
            // Create Book objects
            Book book1 = new Book("978-0134685991", "Effective Java", "Joshua Bloch", 45.00, "2018-01-06");
            Book book2 = new Book("978-0596009205", "Head First Design Patterns", "Eric Freeman", 37.35, "2004-10-25");
            
            // Create a list of books
            List<Book> books = Arrays.asList(book1, book2);
            
            // Convert Java objects to JSON string
            String jsonString = gson.toJson(books);
            System.out.println("Serialized JSON:");
            System.out.println(jsonString);
            
            // Write JSON to file
            FileWriter writer = new FileWriter("books.json");
            gson.toJson(books, writer);
            writer.close();
            System.out.println("JSON file created successfully.");
            
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
```json
[
  {
    "isbn": "978-0134685991",
    "title": "Effective Java",
    "author": "Joshua Bloch",
    "price": 45.0,
    "publishDate": "2018-01-06"
  },
  {
    "isbn": "978-0596009205",
    "title": "Head First Design Patterns",
    "author": "Eric Freeman",
    "price": 37.35,
    "publishDate": "2004-10-25"
  }
]
```
3. Deserializing JSON to Java Objects:
```java
import com.google.gson.Gson;
import com.google.gson.reflect.TypeToken;
import java.io.FileReader;
import java.lang.reflect.Type;
import java.util.List;

public class GsonDeserializeExample {
    public static void main(String[] args) {
        try {
            Gson gson = new Gson();
            
            // Define the type for List<Book>
            Type bookListType = new TypeToken<List<Book>>() {}.getType();
            
            // Read JSON from file and convert to List<Book>
            FileReader reader = new FileReader("books.json");
            List<Book> books = gson.fromJson(reader, bookListType);
            reader.close();
            
            // Display Book details
            System.out.println("Deserialized Books:");
            for (Book book : books) {
                System.out.println("ISBN: " + book.getIsbn());
                System.out.println("Title: " + book.getTitle());
                System.out.println("Author: " + book.getAuthor());
                System.out.println("Price: $" + book.getPrice());
                System.out.println("Publish Date: " + book.getPublishDate());
                System.out.println();
            }
            
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
```
Deserialized Books:
ISBN: 978-0134685991
Title: Effective Java
Author: Joshua Bloch
Price: $45.0
Publish Date: 2018-01-06

ISBN: 978-0596009205
Title: Head First Design Patterns
Author: Eric Freeman
Price: $37.35
Publish Date: 2004-10-25
```
#### **Advanced Gson Features:**
##### **Custom Type Adapters:**
Customize how specific types are serialized or deserialized.
**Example: Custom Serializer for `publishDate`:**
```java
import com.google.gson.*;
import java.lang.reflect.Type;

public class DateSerializer implements JsonSerializer<String>, JsonDeserializer<String> {

    @Override
    public JsonElement serialize(String src, Type typeOfSrc, JsonSerializationContext context) {
        // Custom format if needed
        return new JsonPrimitive(src);
    }

    @Override
    public String deserialize(JsonElement json, Type typeOfT, JsonDeserializationContext context) throws JsonParseException {
        // Custom parsing if needed
        return json.getAsString();
    }
}
```
Register the Adapter:
```java
Gson gson = new GsonBuilder()
    .registerTypeAdapter(String.class, new DateSerializer())
    .create();
```
##### **Exclusion Strategies:**
Exclude certain fields from serialization/deserialization based on criteria.
Example: Exclude Fields with `transient` Modifier:
```java
public class Book {
    private String isbn;
    private String title;
    private String author;
    private double price;
    private transient String internalCode; // This field will be excluded
    private String publishDate;

    // Constructors, Getters, and Setters
    // ...
}
```
