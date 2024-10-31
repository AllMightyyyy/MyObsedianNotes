---
tags:
  - FileManipulation
  - JavaIO
  - XML
  - SAXParsing
---

**What is SAX?**
* SAX is an event-driven, serial-access mechanism for parsing XML documents. It reads the XML document sequentially and triggers events (like start and end of elements) that you can handle through callback methods
#### **Pros and Cons of SAX:**
- **Pros:**
    - Low memory footprint since it doesn't load the entire XML into memory.
    - Faster for large XML files.
- **Cons:**
    - One-way, forward-only parsing; difficult to navigate backward or randomly access elements.
    - More complex to implement for certain tasks, especially modifications.
#### **Java Packages for SAX:**
- `org.xml.sax`
- `javax.xml.parsers.SAXParserFactory`
- `javax.xml.parsers.SAXParser`
#### **Example: Parsing XML with SAX**
```Java
import javax.xml.parsers.SAXParser;
import javax.xml.parsers.SAXParserFactory;
import org.xml.sax.*;
import org.xml.sax.helpers.DefaultHandler;

public class SAXParserExample {
    public static void main(String[] args) {
        try {
            // Create a SAXParserFactory instance
            SAXParserFactory factory = SAXParserFactory.newInstance();
            SAXParser saxParser = factory.newSAXParser();

            // Define a handler
            DefaultHandler handler = new DefaultHandler() {
                boolean bName = false;
                boolean bMajor = false;
                boolean bGPA = false;
                String currentId = "";

                @Override
                public void startElement(String uri, String localName, String qName, Attributes attributes) {
                    if (qName.equalsIgnoreCase("student")) {
                        currentId = attributes.getValue("id");
                        System.out.println("Start Student ID: " + currentId);
                    } else if (qName.equalsIgnoreCase("name")) {
                        bName = true;
                    } else if (qName.equalsIgnoreCase("major")) {
                        bMajor = true;
                    } else if (qName.equalsIgnoreCase("gpa")) {
                        bGPA = true;
                    }
                }

                @Override
                public void characters(char ch[], int start, int length) {
                    if (bName) {
                        System.out.println("Name: " + new String(ch, start, length));
                        bName = false;
                    }
                    if (bMajor) {
                        System.out.println("Major: " + new String(ch, start, length));
                        bMajor = false;
                    }
                    if (bGPA) {
                        System.out.println("GPA: " + new String(ch, start, length));
                        bGPA = false;
                    }
                }

                @Override
                public void endElement(String uri, String localName, String qName) {
                    if (qName.equalsIgnoreCase("student")) {
                        System.out.println("End Student ID: " + currentId + "\n");
                    }
                }
            };

            // Parse the XML file
            saxParser.parse("students.xml", handler);

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
**Explanation:**

1. **SAXParserFactory & SAXParser:** Initialize SAX parser.
2. **DefaultHandler:** Implement callback methods (`startElement`, `characters`, `endElement`).
3. **Handle Events:**
    - **startElement:** Detect start of elements and extract attributes.
    - **characters:** Capture text content within elements.
    - **endElement:** Detect end of elements.
4. **Parse XML:** Process `students.xml` using the handler.
#### **When to Use SAX:**
- When dealing with large XML files where memory consumption is a concern.
- When you need to process XML data sequentially without modifications.