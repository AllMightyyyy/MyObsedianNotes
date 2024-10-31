---
tags:
  - FileManipulation
  - JavaIO
  - XML
  - DOMParsing
---

### DOM (Document Object Model)

**What is DOM ?**
	 * represents an entire XML document as a tree structure in memory, where each node corresponds to a part of the document ( elements, attributes, text, etc. ). It allows for easy navigation and modification of the XML Structure
		 * Prons and Cons : 
			 * Pros -> 
				 * Easy to navigate and manipulate XML Tree
				 * Random access to any part of the document
			 * Cons ->
				 * High memory consumption, especially for large XML Files
				 * Slower performance for large documents due to in-memory representation
* Java Package for DOM :
	* `javax.xml.parsers.DocumentBuilderFactory`
	- `javax.xml.parsers.DocumentBuilder`
	- `org.w3c.dom.Document`
	- `org.w3c.dom.Element`, etc.
**XML File (students.xml) :** 
```XML
<?xml version="1.0" encoding="UTF-8"?>
<students>
    <student id="s1">
        <name>Jane Doe</name>
        <major>Computer Science</major>
        <gpa>3.8</gpa>
    </student>
    <student id="s2">
        <name>John Smith</name>
        <major>Mathematics</major>
        <gpa>3.6</gpa>
    </student>
</students>
```
**Java Code to parse and modify :**
```java
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.DocumentBuilder;
import org.w3c.dom.*;
import javax.xml.transform.*;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import java.io.File;

public class DOMParserExample {
    public static void main(String[] args) {
        try {
            // Initialize Document Builder
            DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
            DocumentBuilder builder = factory.newDocumentBuilder();

            // Parse the XML file
            Document doc = builder.parse(new File("students.xml"));
            doc.getDocumentElement().normalize();

            // Display root element
            System.out.println("Root element: " + doc.getDocumentElement().getNodeName());

            // Get all student nodes
            NodeList nodeList = doc.getElementsByTagName("student");

            for (int i = 0; i < nodeList.getLength(); i++) {
                Node node = nodeList.item(i);

                if (node.getNodeType() == Node.ELEMENT_NODE) {
                    Element student = (Element) node;
                    String id = student.getAttribute("id");
                    String name = student.getElementsByTagName("name").item(0).getTextContent();
                    String major = student.getElementsByTagName("major").item(0).getTextContent();
                    String gpa = student.getElementsByTagName("gpa").item(0).getTextContent();

                    System.out.println("\nStudent ID: " + id);
                    System.out.println("Name: " + name);
                    System.out.println("Major: " + major);
                    System.out.println("GPA: " + gpa);
                }
            }

            // Modify the GPA of student s1
            for (int i = 0; i < nodeList.getLength(); i++) {
                Element student = (Element) nodeList.item(i);
                if (student.getAttribute("id").equals("s1")) {
                    student.getElementsByTagName("gpa").item(0).setTextContent("4.0");
                }
            }

            // Write the updated document back to file
            TransformerFactory transformerFactory = TransformerFactory.newInstance();
            Transformer transformer = transformerFactory.newTransformer();
            DOMSource source = new DOMSource(doc);
            StreamResult result = new StreamResult(new File("students_updated.xml"));
            transformer.setOutputProperty(OutputKeys.INDENT, "yes");
            transformer.transform(source, result);

            System.out.println("\nUpdated XML saved as students_updated.xml");

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
**Explanation:**

1. **Initialize Document Builder:** Set up the DOM parser.
2. **Parse XML:** Load and parse `students.xml`.
3. **Normalize Document:** Merges adjacent text nodes and simplifies the tree.
4. **Traverse Nodes:** Iterate through `<student>` elements and display their details.
5. **Modify Node:** Change the GPA of the student with `id="s1"` to `4.0`.
6. **Write Back to File:** Save the modified XML to `students_updated.xml`.

**When to use:**
- When you need to navigate and manipulate the XML tree extensively.
- Suitable for small to medium-sized XML documents.