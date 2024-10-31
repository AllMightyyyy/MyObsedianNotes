---
tags:
  - FileManipulation
  - JavaIO
  - ExceptionHandling
  - CustomExceptions
---

#### **Why is Error Handling Important?**
When dealing with JSON data, especially from external sources, it's crucial to handle errors gracefully to prevent application crashes and ensure data integrity.
#### **Common JSON Errors:**
- **Malformed JSON:** Syntax errors like missing braces or quotes.
- **Unexpected Data Types:** Mismatched types (e.g., expecting a string but receiving a number).
- **Missing Fields:** Required fields are absent.
- **Extra Fields:** Fields not mapped to Java classes.
#### **Handling Errors with Jackson:**
**Example: Handling Malformed JSON**
```java
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.core.JsonParseException;
import com.fasterxml.jackson.databind.JsonMappingException;
import java.io.File;

public class JacksonErrorHandlingExample {
    public static void main(String[] args) {
        try {
            ObjectMapper mapper = new ObjectMapper();
            // Attempt to deserialize malformed JSON
            Student student = mapper.readValue(new File("malformed_student.json"), Student.class);
        } catch (JsonParseException e) {
            System.out.println("Malformed JSON: " + e.getMessage());
        } catch (JsonMappingException e) {
            System.out.println("Mapping Error: " + e.getMessage());
        } catch (Exception e) {
            System.out.println("General Error: " + e.getMessage());
        }
    }
}
```
output -> 
```
Malformed JSON: Unexpected character ('}' (code 125)): was expecting double-quote to start field name
 at [Source: (File); line: 3, column: 10]
```
#### **Best Practices for Error Handling:**
1. **Use Specific Exceptions:** Catch specific exceptions like `JsonParseException`, `JsonMappingException` for more precise error handling.
2. **Validate JSON Before Processing:** Use schema validation (e.g., JSON Schema) to ensure JSON adheres to expected structure.
3. **Logging Errors:** Log error details for debugging purposes.
4. **Graceful Degradation:** Allow the application to continue functioning even if some data fails to parse.
#### **Handling Errors with Gson:**
**Example: Handling Unexpected Data Types
```java
import com.google.gson.Gson;
import com.google.gson.JsonSyntaxException;
import java.io.FileReader;

public class GsonErrorHandlingExample {
    public static void main(String[] args) {
        try {
            Gson gson = new Gson();
            // Attempt to deserialize JSON with incorrect data types
            FileReader reader = new FileReader("incorrect_types.json");
            Student student = gson.fromJson(reader, Student.class);
            reader.close();
        } catch (JsonSyntaxException e) {
            System.out.println("JSON Syntax Error: " + e.getMessage());
        } catch (Exception e) {
            System.out.println("General Error: " + e.getMessage());
        }
    }
}
```
output -> (for incorrect data types)
```
JSON Syntax Error: java.lang.NumberFormatException: For input string: "three.point.five"
```
