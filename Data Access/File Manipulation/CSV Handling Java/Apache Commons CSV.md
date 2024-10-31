---
tags:
  - FileManipulation
  - JavaIO
  - CSV
  - ExceptionHandling
  - ApacheCommons
---

##### **Overview:**
Apache Commons CSV is a robust and flexible library for reading and writing CSV files. It supports various CSV formats, custom delimiters, quoting, and parsing configurations.
##### **Setting Up Apache Commons CSV:**
To use Apache Commons CSV, include the following Maven dependency in your `pom.xml`:
```xml
<dependencies>
    <!-- Apache Commons CSV -->
    <dependency>
        <groupId>org.apache.commons</groupId>
        <artifactId>commons-csv</artifactId>
        <version>1.10.0</version>
    </dependency>
</dependencies>
```
##### **Reading CSV Files with Apache Commons CSV:**
**Example: Reading a CSV File Using Apache Commons CSV
```java
import org.apache.commons.csv.CSVFormat;
import org.apache.commons.csv.CSVParser;
import org.apache.commons.csv.CSVRecord;

import java.io.FileReader;
import java.io.Reader;
import java.util.Arrays;

public class ApacheCommonsCSVReader {
    public static void main(String[] args) {
        String csvFile = "inventory.csv";

        try (
            Reader reader = new FileReader(csvFile);
            CSVParser csvParser = new CSVParser(reader, CSVFormat.DEFAULT
                    .withFirstRecordAsHeader()
                    .withIgnoreHeaderCase()
                    .withTrim())
        ) {
            for (CSVRecord csvRecord : csvParser) {
                // Accessing values by column names
                String productId = csvRecord.get("productId");
                String productName = csvRecord.get("productName");
                String quantity = csvRecord.get("quantity");
                String price = csvRecord.get("price");

                System.out.println("Product ID: " + productId);
                System.out.println("Product Name: " + productName);
                System.out.println("Quantity: " + quantity);
                System.out.println("Price: " + price);
                System.out.println("---------------------------");
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
```
Product ID: P001
Product Name: Widget A
Quantity: 100
Price: 9.99
---------------------------
Product ID: P002
Product Name: Gadget, B
Quantity: 50
Price: 19.99
---------------------------
Product ID: P003
Product Name: Thingamajig C
Quantity: 75
Price: 14.99
---------------------------
Product ID: P004
Product Name: Doohickey "D"
Quantity: 200
Price: 4.99
---------------------------
```

**Explanation:**
- **CSVFormat.DEFAULT:** Uses standard CSV format.
- **withFirstRecordAsHeader():** Treats the first line as headers.
- **withIgnoreHeaderCase():** Makes header matching case-insensitive.
- **withTrim():** Trims leading and trailing spaces from fields.
- **CSVRecord:** Represents a single record in the CSV.
##### **Writing CSV Files with Apache Commons CSV:**
**Example: Writing to a CSV File Using Apache Commons CSV**
```java
import org.apache.commons.csv.CSVFormat;
import org.apache.commons.csv.CSVPrinter;

import java.io.FileWriter;
import java.io.Writer;
import java.util.Arrays;
import java.util.List;

public class ApacheCommonsCSVWriter {
    public static void main(String[] args) {
        String csvFile = "output_inventory.csv";
        List<String> headers = Arrays.asList("productId", "productName", "quantity", "price");
        List<List<String>> data = Arrays.asList(
                Arrays.asList("P005", "Widget E", "150", "12.99"),
                Arrays.asList("P006", "Gizmo F", "80", "22.50"),
                Arrays.asList("P007", "Contraption G", "60", "18.75")
        );

        try (
            Writer writer = new FileWriter(csvFile);
            CSVPrinter csvPrinter = new CSVPrinter(writer, CSVFormat.DEFAULT
                    .withHeader(headers.toArray(new String[0])))
        ) {
            for (List<String> row : data) {
                csvPrinter.printRecord(row);
            }
            csvPrinter.flush();
            System.out.println("CSV file written successfully using Apache Commons CSV.");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
**Explanation:**
- **withHeader():** Defines the header row.
- **printRecord():** Writes each row to the CSV file.
- **flush():** Ensures all data is written to the file.
##### **Advantages of Apache Commons CSV:**
- **Flexibility:** Supports various CSV formats and custom configurations.
- **Robustness:** Handles complex scenarios like quoted fields, escaped characters, and custom delimiters.
- **Ease of Use:** Provides a straightforward API for reading and writing CSV files.