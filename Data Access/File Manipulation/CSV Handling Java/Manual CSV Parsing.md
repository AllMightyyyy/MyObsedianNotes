#### **Reading CSV Files Manually**
Using Java's `BufferedReader` and `String.split()` method, you can read and parse CSV files line by line.

**Example: Reading a CSV File Manually**
```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class ManualCSVReader {
    public static void main(String[] args) {
        String csvFile = "inventory.csv";
        String line;
        String delimiter = ",";

        try (BufferedReader br = new BufferedReader(new FileReader(csvFile))) {
            // Read the header line
            String headerLine = br.readLine();
            String[] headers = headerLine.split(delimiter);
            System.out.println("Headers:");
            for (String header : headers) {
                System.out.print(header + " | ");
            }
            System.out.println("\n-------------------------");

            // Read data lines
            while ((line = br.readLine()) != null) {
                // Split line by comma
                String[] fields = line.split(delimiter, -1); // -1 to include trailing empty strings
                for (String field : fields) {
                    System.out.print(field + " | ");
                }
                System.out.println();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
```Methematica
Headers:
productId | productName | quantity | price | 
-------------------------
P001 | Widget A | 100 | 9.99 | 
P002 | "Gadget |  B" | 50 | 19.99 | 
P003 | Thingamajig C | 75 | 14.99 | 
P004 | "Doohickey ""D""" | 200 | 4.99 | 
```
**Limitations:**
- **Quoted Fields:** The `split()` method doesn't handle commas within quoted fields properly.
- **Escaping Quotes:** Handling escaped quotes and other special characters is cumbersome.
- **Performance:** Not optimized for large files or complex parsing rules.
#### **Writing CSV Files Manually**
Using Java's `BufferedWriter` or `PrintWriter`, you can write data to CSV files by joining fields with commas.
**Example: Writing to a CSV File Manually**
```java
import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;

public class ManualCSVWriter {
    public static void main(String[] args) {
        String csvFile = "output_inventory.csv";
        String[] headers = {"productId", "productName", "quantity", "price"};
        String[][] data = {
            {"P005", "Widget E", "150", "12.99"},
            {"P006", "Gizmo F", "80", "22.50"},
            {"P007", "Contraption G", "60", "18.75"}
        };

        try (BufferedWriter bw = new BufferedWriter(new FileWriter(csvFile))) {
            // Write headers
            bw.write(String.join(",", headers));
            bw.newLine();

            // Write data rows
            for (String[] row : data) {
                bw.write(String.join(",", row));
                bw.newLine();
            }

            System.out.println("CSV file written successfully.");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
```
Output (`output_inventory.csv`):
productId,productName,quantity,price
P005,Widget E,150,12.99
P006,Gizmo F,80,22.50
P007,Contraption G,60,18.75
```

**Limitations:**
- **Data Sanitization:** Must manually handle special characters, quoting, and escaping.
- **Complex Structures:** Not suitable for complex or nested data.
- **Error Handling:** Requires additional logic to handle errors and edge cases.
