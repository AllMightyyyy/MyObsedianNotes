---
tags:
  - FileManipulation
  - JavaIO
  - JSON
  - ExceptionHandling
  - CustomExceptions
---

#### **Why Customize Serialization?**
Sometimes, the default serialization/deserialization behavior doesn't meet your application's requirements. Customizing allows you to control how objects are converted to JSON and vice versa.

#### **Strategies for Customization:**
1. **Custom Serializers/Deserializers:** Define how specific types or classes are serialized/deserialized.
2. **Annotations:** Use library-specific annotations to influence the serialization process.
3. **Exclusion Strategies:** Exclude fields based on criteria (e.g., transient fields, custom annotations).
4. **Pretty Printing and Formatting:** Control the output format for readability.
#### **Custom Serializers with Jackson:**
**Example: Custom Serializer for `Date` Field**
```java
import com.fasterxml.jackson.core.JsonGenerator;
import com.fasterxml.jackson.databind.SerializerProvider;
import com.fasterxml.jackson.databind.ser.std.StdSerializer;
import java.io.IOException;
import java.text.SimpleDateFormat;
import java.util.Date;

public class DateSerializer extends StdSerializer<Date> {
    private static SimpleDateFormat formatter = new SimpleDateFormat("dd-MM-yyyy");

    public DateSerializer() {
        this(null);
    }

    public DateSerializer(Class<Date> t) {
        super(t);
    }

    @Override
    public void serialize(Date value, JsonGenerator gen, SerializerProvider provider) throws IOException {
        gen.writeString(formatter.format(value));
    }
}
```
```java
import com.fasterxml.jackson.databind.annotation.JsonSerialize;
import java.util.Date;

public class Person {
    private String name;
    
    @JsonSerialize(using = DateSerializer.class)
    private Date dateOfBirth;

    // Constructors, Getters, and Setters
    // ...
}
```
##### **Using Annotations:**
- **`@JsonIgnore`:** Exclude a field.
- **`@JsonProperty`:** Rename a field in JSON.
- **`@JsonInclude`:** Specify inclusion criteria (e.g., non-null).
**Example:**
```java
import com.fasterxml.jackson.annotation.JsonIgnore;
import com.fasterxml.jackson.annotation.JsonProperty;
import com.fasterxml.jackson.annotation.JsonInclude;

public class User {
    private String username;
    
    @JsonIgnore
    private String password;
    
    @JsonProperty("emailAddress")
    private String email;
    
    @JsonInclude(JsonInclude.Include.NON_NULL)
    private String phoneNumber;

    // Constructors, Getters, and Setters
    // ...
}
```
```Json
{
    "username": "johndoe",
    "emailAddress": "johndoe@example.com",
    "phoneNumber": null
}
```
Note: `phoneNumber` is excluded due to `NON_NULL` inclusion.

Example: Exclude Fields Using `transient` Keyword
```java
public class Account {
    private String accountId;
    private String ownerName;
    private transient double balance; // Excluded from serialization

    // Constructors, Getters, and Setters
    // ...
}
```
Example: Custom Serializer with Gson:
```java
import com.google.gson.*;
import java.lang.reflect.Type;

public class SalarySerializer implements JsonSerializer<Double> {
    @Override
    public JsonElement serialize(Double src, Type typeOfSrc, JsonSerializationContext context) {
        return new JsonPrimitive("$" + String.format("%.2f", src));
    }
}
```
Register the Serializer:
```java
Gson gson = new GsonBuilder()
    .registerTypeAdapter(Double.class, new SalarySerializer())
    .create();
```