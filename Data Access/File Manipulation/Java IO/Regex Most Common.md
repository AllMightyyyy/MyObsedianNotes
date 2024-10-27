Regular expressions (regex) are powerful tools for pattern matching and text processing. Below are some basic and commonly used regex patterns that can help you perform various tasks efficiently.

### 1. **Basic Character Matching**

- `.`: Matches any single character except newline.
    - Example: `a.b` matches "acb", "a5b", but not "ab" or "acbd".
- `\w`: Matches any word character (alphanumeric character + underscore).
    - Equivalent to `[a-zA-Z0-9_]`.
- `\W`: Matches any non-word character.
- `\d`: Matches any digit (0-9).
- `\D`: Matches any non-digit character.
- `\s`: Matches any whitespace character (spaces, tabs, line breaks).
- `\S`: Matches any non-whitespace character.

### 2. **Anchors (Position Matching)**

- `^`: Matches the start of a line or string.
    - Example: `^Hello` matches "Hello" at the beginning of a line.
- `$`: Matches the end of a line or string.
    - Example: `world$` matches "world" at the end of a line.
- `\b`: Matches a word boundary (the position between a word character and a non-word character).
    - Example: `\bcat\b` matches "cat" in "cat is here" but not in "caterpillar".
- `\B`: Matches a position that is not a word boundary.

### 3. **Quantifiers**

- `*`: Matches 0 or more occurrences of the preceding character or group.
    - Example: `a*` matches "", "a", "aa", "aaa", etc.
- `+`: Matches 1 or more occurrences.
    - Example: `a+` matches "a", "aa", "aaa", but not "".
- `?`: Matches 0 or 1 occurrence (optional).
    - Example: `colou?r` matches "color" and "colour".
- `{n}`: Matches exactly `n` occurrences.
    - Example: `a{3}` matches "aaa".
- `{n,}`: Matches `n` or more occurrences.
    - Example: `a{2,}` matches "aa", "aaa", etc.
- `{n,m}`: Matches between `n` and `m` occurrences.
    - Example: `a{2,4}` matches "aa", "aaa", "aaaa".

### 4. **Character Classes**

- `[abc]`: Matches any one character listed within the brackets.
    - Example: `[abc]` matches "a", "b", or "c".
- `[^abc]`: Matches any character NOT listed within the brackets.
    - Example: `[^abc]` matches anything except "a", "b", or "c".
- `[a-z]`: Matches any character in the range from "a" to "z".
- `[A-Z]`: Matches any uppercase letter.
- `[0-9]`: Matches any digit.
- `[a-zA-Z0-9]`: Matches any alphanumeric character.

### 5. **Groups and Alternation**

- `(abc)`: Capturing group for matching "abc".
- `|`: Alternation (OR) operator.
    - Example: `cat|dog` matches "cat" or "dog".

### 6. **Escape Sequences**

- `\.`: Matches a literal period (".").
- `\*`, `\+`, `\?`, `\{`, `\}`, `\(`, `\)`, `\|`, etc., escape special characters to match them literally.

### 7. **Lookaheads and Lookbehinds (Advanced)**

- `(?=...)`: Positive lookahead. Asserts that what follows the current position matches the pattern inside.
    - Example: `a(?=b)` matches "a" only if followed by "b".
- `(?!...)`: Negative lookahead. Asserts that what follows does NOT match the pattern inside.
    - Example: `a(?!b)` matches "a" only if NOT followed by "b".
- `(?<=...)`: Positive lookbehind. Asserts that what precedes the current position matches the pattern inside.
    - Example: `(?<=b)a` matches "a" only if preceded by "b".
- `(?<!...)`: Negative lookbehind. Asserts that what precedes does NOT match the pattern inside.
    - Example: `(?<!b)a` matches "a" only if NOT preceded by "b".

### 8. **Commonly Used Regex Patterns**

#### Matching an Email Address

- Pattern: `^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$`
    - Explanation:
        - `^[a-zA-Z0-9._%+-]+`: Starts with one or more allowed characters before the "@".
        - `@[a-zA-Z0-9.-]+`: The domain name.
        - `\.[a-zA-Z]{2,}$`: Top-level domain (at least two characters).

#### Matching a URL

- Pattern: `^(https?|ftp)://[^\s/$.?#].[^\s]*$`
    - Explanation:
        - `^(https?|ftp)`: The protocol ("http", "https", or "ftp").
        - `://`: Literal "://".
        - `[^\s/$.?#].[^\s]*$`: The domain and optional path.

#### Matching a Phone Number

- Pattern: `^\+?[0-9]{1,3}?[-.●]?[0-9]{1,4}?[-.●]?[0-9]{1,4}?[-.●]?[0-9]{1,9}$`
    - Explanation:
        - `^\+?`: Optional "+" at the beginning.
        - `[0-9]{1,3}`: Country code (1-3 digits).
        - `[-.●]?`: Optional separator.
        - `[0-9]{1,4}`: Area code.
        - The rest allows for different number formats.

#### Matching a Date (YYYY-MM-DD)

- Pattern: `^\d{4}-\d{2}-\d{2}$`
    - Explanation:
        - `^\d{4}`: Four-digit year.
        - `-\d{2}`: Two-digit month.
        - `-\d{2}$`: Two-digit day.

#### Matching an IPv4 Address

- Pattern: `^(\d{1,3}\.){3}\d{1,3}$`
    - Explanation:
        - `(\d{1,3}\.){3}`: Three groups of 1-3 digits followed by a period.
        - `\d{1,3}$`: Final group of 1-3 digits.