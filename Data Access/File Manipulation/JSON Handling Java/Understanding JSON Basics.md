---
tags:
  - FileManipulation
  - JavaIO
  - JSON
---

### Learn JSON Syntax

#### **What is JSON?**
JSON (JavaScript Object Notation) is a lightweight data interchange format that's easy for humans to read and write and easy for machines to parse and generate. It's language-independent but uses conventions familiar to programmers of the C-family of languages, including Java.
#### **JSON Structure:**
- **Objects:** Encapsulated within `{}` and consist of key-value pairs.
- **Arrays:** Ordered lists of values encapsulated within `[]`.
- **Values:** Can be strings, numbers, booleans, null, objects, or arrays.
#### **Basic JSON Syntax Rules:**
1. **Data is in key/value pairs:** `"key": "value"`
2. **Data is separated by commas.**
3. **Curly braces hold objects.**
4. **Square brackets hold arrays.**
5. **Keys must be strings enclosed in double quotes.**
6. **Values can be of various types:**
    - **String:** `"Hello World"`
    - **Number:** `42`
    - **Boolean:** `true` or `false`
    - **Null:** `null`
    - **Object:** `{ "name": "John" }`
    - **Array:** `[1, 2, 3]`

**Example of Well-Formed JSON Document :
```JSON
{
    "library": {
        "books": [
            {
                "isbn": "978-0134685991",
                "title": "Effective Java",
                "author": "Joshua Bloch",
                "year": 2018,
                "available": true
            },
            {
                "isbn": "978-0596009205",
                "title": "Head First Design Patterns",
                "author": "Eric Freeman",
                "year": 2004,
                "available": false
            }
        ],
        "location": "Downtown Branch"
    }
}
```
#### **Key Components:**
- **Root Object:** The entire JSON data is enclosed within `{}`.
- **Nested Objects:** The `library` object contains `books` (an array) and `location`.
- **Arrays:** The `books` array holds multiple book objects.
- **Data Types:** Demonstrates strings, numbers, booleans, and nested structures.