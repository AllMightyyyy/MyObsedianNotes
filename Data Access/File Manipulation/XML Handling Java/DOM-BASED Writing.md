---
tags:
  - FileManipulation
  - JavaIO
  - DOMWriting
  - XML
---

#### **Creating XML with DOM:**

Using DOM to write XML involves building the XML tree in memory and then transforming it into an XML file.
#### **Example: Creating an XML Document with DOM**
```Java
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.DocumentBuilder;
import org.w3c.dom.*;
import javax.xml.transform.*;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import java.io.File;

public class DOMWriterExample {
    public static void main(String[] args) {
        try {
            // Initialize Document Builder
            DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
            DocumentBuilder builder = factory.newDocumentBuilder();

            // Create a new Document
            Document doc = builder.newDocument();

            // Root element
            Element root = doc.createElement("library");
            doc.appendChild(root);

            // Create a book element
            Element book = doc.createElement("book");
            book.setAttribute("isbn", "978-0134685991");
            root.appendChild(book);

            // Title
            Element title = doc.createElement("title");
            title.appendChild(doc.createTextNode("Effective Java"));
            book.appendChild(title);

            // Author
            Element author = doc.createElement("author");
            author.appendChild(doc.createTextNode("Joshua Bloch"));
            book.appendChild(author);

            // Year
            Element year = doc.createElement("year");
            year.appendChild(doc.createTextNode("2018"));
            book.appendChild(year);

            // Transform DOM to XML
            TransformerFactory transformerFactory = TransformerFactory.newInstance();
            Transformer transformer = transformerFactory.newTransformer();
            // Pretty print
            transformer.setOutputProperty(OutputKeys.INDENT, "yes");
            DOMSource source = new DOMSource(doc);
            StreamResult result = new StreamResult(new File("library.xml"));
            transformer.transform(source, result);

            System.out.println("XML file created successfully.");

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
**Explanation:**
1. **Initialize Document Builder:** Set up the DOM environment.
2. **Create Document:** Start a new XML document.
3. **Build Elements:**
    - **Root Element:** `<library>`
    - **Child Elements:** `<book>`, `<title>`, `<author>`, `<year>` with attributes and text.
4. **Transform and Save:**
    - Use `Transformer` to convert the DOM tree into an XML file (`library.xml`).
    - Set output properties for formatting (indentation).
```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<library>
    <book isbn="978-0134685991">
        <title>Effective Java</title>
        <author>Joshua Bloch</author>
        <year>2018</year>
    </book>
</library>
```
#### **Adding More Elements:**
To add more books or other elements, repeat the element creation and appending process.