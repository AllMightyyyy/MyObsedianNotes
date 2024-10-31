---
tags:
  - FileManipulation
  - JavaIO
  - XML
---

## XML Structure & Syntax
### what is XML ? 
 * XML (eXtensible Markup Language) is a versatile, structured format used to store and transport data
### Structure of a XML :
* Elements -> building blocks of XML, defined by tags
* Attributes -> Provide additional info about elements
* Namespaces -> Prevent naming conflicts by qualifying names used in XML
* CDATA Sections -> Contain character data that should not be parsed by the XML parser
* Comments -> non-executable notes
* Processing instructions -> provide directives to applications processing the XML

### Basic XML Syntax Rules :

* Well-Formedness -> must adhere to syntax rules
	* Every start tag must have a corresponding end tag
	* Elements must be properly nested
	* Attribute values MUST be quoted
* Case sensitivity (`<Item>` ≠ `<item>`).
* Single Root Element -> XML document must have exactly one root element

### XML Schema (XSD)

#### **What is XML Schema (XSD)?**

XML Schema Definition (XSD) is a language used to define the structure, content, and data types of XML documents. Unlike DTD (Document Type Definition), XSD is written in XML syntax and supports data types, namespaces, and more complex constraints.

#### **Why Use XML Schema?**

- **Validation:** Ensures XML documents adhere to a defined structure and data types.
- **Data Types:** Supports built-in and custom data types (e.g., string, integer, date).
- **Namespaces:** Better support for namespaces compared to DTD.
- **Extensibility:** More powerful and flexible than DTD.
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
#### **Basic Components of XSD:**

- **Elements:** Define the elements in the XML document.
- **Attributes:** Define attributes for elements.
- **Complex Types:** Define elements that contain other elements or attributes.
- **Simple Types:** Define elements that contain only text.
```XSD
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">

    <xs:element name="students">
        <xs:complexType>
            <xs:sequence>
                <xs:element name="student" maxOccurs="unbounded">
                    <xs:complexType>
                        <xs:sequence>
                            <xs:element name="name" type="xs:string"/>
                            <xs:element name="major" type="xs:string"/>
                            <xs:element name="gpa" type="xs:decimal"/>
                        </xs:sequence>
                        <xs:attribute name="id" type="xs:string" use="required"/>
                    </xs:complexType>
                </xs:element>
            </xs:sequence>
        </xs:complexType>
    </xs:element>

</xs:schema>

```
#### **Explanation:**

- **Root Element:** `<students>` containing multiple `<student>` elements.
- **Student Element:**
    - **Attributes:** `id` (required).
    - **Child Elements:** `<name>`, `<major>`, `<gpa>` with respective data types.

#### **Validating XML with XSD in Java**

To validate an XML document against an XSD schema in Java, you can use the `javax.xml.validation` package.
```Java
import javax.xml.XMLConstants;
import javax.xml.transform.stream.StreamSource;
import javax.xml.validation.*;
import java.io.File;

public class XMLValidator {
    public static void main(String[] args) {
        try {
            // Create a SchemaFactory capable of understanding WXS schemas
            SchemaFactory factory = SchemaFactory.newInstance(XMLConstants.W3C_XML_SCHEMA_NS_URI);

            // Load the schema
            Schema schema = factory.newSchema(new File("students.xsd"));

            // Create a Validator
            Validator validator = schema.newValidator();

            // Validate the XML file
            validator.validate(new StreamSource(new File("students.xml")));

            System.out.println("XML is valid.");
        } catch (Exception e) {
            System.out.println("XML is NOT valid.");
            System.out.println("Reason: " + e.getMessage());
        }
    }
}
```
