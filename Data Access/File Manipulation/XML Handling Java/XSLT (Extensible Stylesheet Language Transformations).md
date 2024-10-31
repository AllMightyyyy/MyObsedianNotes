---
tags:
  - FileManipulation
  - JavaIO
  - XML
  - XSLT
---

#### **What is XSLT?**
XSLT is a language for transforming XML documents into other formats, such as HTML, plain text, or another XML structure. It uses a stylesheet to define the transformation rules.
#### **Why Use XSLT?**
- **Separation of Concerns:** Separates data (XML) from presentation (HTML).
- **Reusability:** Stylesheets can be reused for multiple XML documents.
- **Flexibility:** Can produce various output formats from a single XML source.
#### **Basic XSLT Structure:**
- **`<xsl:stylesheet>`:** Root element defining the XSLT version and namespace.
- **`<xsl:template>`:** Defines a rule for transforming specific XML elements.
- **`<xsl:value-of>`:** Extracts the value of an XML element or attribute.
- **`<xsl:for-each>`:** Iterates over a set of nodes.
- **`<xsl:if>` and `<xsl:choose>`:** Conditional processing.
#### **Example: Transforming XML to HTML Using XSLT**
**XML File (`books.xml`):**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<books>
    <book isbn="978-0134685991">
        <title>Effective Java</title>
        <author>Joshua Bloch</author>
        <year>2018</year>
    </book>
    <book isbn="978-0596009205">
        <title>Head First Design Patterns</title>
        <author>Eric Freeman</author>
        <year>2004</year>
    </book>
    <book isbn="978-1617294945">
        <title>Java Concurrency in Practice</title>
        <author>Brian Goetz</author>
        <year>2006</year>
    </book>
</books>
```
**XSLT Stylesheet** (`books.xslt`) :
```xml
<?xml version="1.0" encoding="UTF-8"?>
<xsl:stylesheet version="1.0"
    xmlns:xsl="http://www.w3.org/1999/XSL/Transform">

    <!-- Match the root element -->
    <xsl:template match="/books">
        <html>
            <head>
                <title>Bookstore</title>
            </head>
            <body>
                <h2>Book List</h2>
                <table border="1">
                    <tr bgcolor="#9acd32">
                        <th>ISBN</th>
                        <th>Title</th>
                        <th>Author</th>
                        <th>Year</th>
                    </tr>
                    <!-- Iterate over each book -->
                    <xsl:for-each select="book">
                        <tr>
                            <td><xsl:value-of select="@isbn"/></td>
                            <td><xsl:value-of select="title"/></td>
                            <td><xsl:value-of select="author"/></td>
                            <td><xsl:value-of select="year"/></td>
                        </tr>
                    </xsl:for-each>
                </table>
            </body>
        </html>
    </xsl:template>

</xsl:stylesheet>
```
**Explanation:**
1. **Match Root:** The template matches the root `<books>` element.
2. **HTML Structure:** Defines the HTML document structure with `<html>`, `<head>`, and `<body>`.
3. **Table Creation:**
    - **Header Row:** Contains column titles.
    - **Data Rows:** Uses `<xsl:for-each>` to iterate over each `<book>` element.
    - **Data Cells:** Extracts `@isbn`, `<title>`, `<author>`, and `<year>` using `<xsl:value-of>`