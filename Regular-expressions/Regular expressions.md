
---

# Regular Expressions (Regex) Cheat Sheet

## 1. Basic Syntax

### Characters
- **Literal Characters:** Match themselves directly.
  - Example: `abc` matches the string "abc".
- **Metacharacters:** Characters with special meanings (e.g., `. ^ $ * + ? { } [ ] \ | ( )`).
  - Example: `.` matches any character except a newline.

### Escaping Metacharacters
- Use `\` to escape metacharacters to match them literally.
  - Example: `\.` matches the literal period character.

### Special Characters and Sequences
- **`.`:** Matches any single character except newline.
- **`^`**: Matches the start of a string.
- **`$`**: Matches the end of a string.
- **`\d`**: Matches any digit (`[0-9]`).
- **`\D`**: Matches any non-digit.
- **`\w`**: Matches any word character (alphanumeric plus `_`).
- **`\W`**: Matches any non-word character.
- **`\s`**: Matches any whitespace character (spaces, tabs, line breaks).
- **`\S`**: Matches any non-whitespace character.

### Examples
- `^hello`: Matches "hello" at the beginning of a string.
- `world$`: Matches "world" at the end of a string.
- `h.t`: Matches "hat", "hit", etc.
- `\d\d\d`: Matches any three digits, like "123" or "456".

## 2. Quantifiers

### Basic Quantifiers
- **`*`**: Matches 0 or more of the preceding element.
  - Example: `a*` matches "", "a", "aa", etc.
- **`+`**: Matches 1 or more of the preceding element.
  - Example: `a+` matches "a", "aa", etc.
- **`?`**: Matches 0 or 1 of the preceding element.
  - Example: `a?` matches "", "a".
- **`{n}`**: Matches exactly `n` occurrences of the preceding element.
  - Example: `a{3}` matches "aaa".
- **`{n,}`**: Matches `n` or more occurrences.
  - Example: `a{2,}` matches "aa", "aaa", etc.
- **`{n,m}`**: Matches between `n` and `m` occurrences.
  - Example: `a{2,4}` matches "aa", "aaa", or "aaaa".

### Examples
- `a{3}`: Matches exactly three 'a's ("aaa").
- `a{2,}`: Matches two or more 'a's ("aa", "aaa", etc.).
- `a{1,3}`: Matches between one and three 'a's ("a", "aa", "aaa").

## 3. Character Classes

### Basic Character Classes
- **`[...]`**: Matches any single character within the brackets.
  - Example: `[abc]` matches "a", "b", or "c".
- **`[^...]`**: Matches any single character not within the brackets.
  - Example: `[^abc]` matches any character except "a", "b", or "c".
- **`[a-z]`**: Matches any character in the specified range.
  - Example: `[a-z]` matches any lowercase letter.

### Predefined Character Classes
- **`\d`**: Matches any digit.
- **`\D`**: Matches any non-digit.
- **`\w`**: Matches any word character.
- **`\W`**: Matches any non-word character.
- **`\s`**: Matches any whitespace character.
- **`\S`**: Matches any non-whitespace character.

### Examples
- `[aeiou]`: Matches any vowel.
- `[^0-9]`: Matches any character that is not a digit.
- `[a-zA-Z]`: Matches any letter (uppercase or lowercase).

## 4. Groups and Ranges

### Capturing Groups
- **`( ... )`**: Captures the matched text for later use.
  - Example: `(abc)` captures "abc".
- **`\1, \2, ...`**: References the captured groups.
  - Example: `(a)(b)\2\1` matches "abba".

### Non-Capturing Groups
- **`(?: ... )`**: Groups without capturing.
  - Example: `(?:abc)` matches "abc" without capturing.

### Alternation
- **`|`**: Matches either the expression before or after the `|`.
  - Example: `a|b` matches "a" or "b".
- **`(...)|(...)`**: Matches any of the grouped alternatives.
  - Example: `(abc|def)` matches "abc" or "def".

### Examples
- `(abc|def)`: Matches "abc" or "def".
- `(ha|he)` matches "ha" or "he".
- `(hello|world)$`: Matches "hello" or "world" at the end of a string.

## 5. Anchors

### String Anchors
- **`^`**: Anchors the match to the start of a string.
  - Example: `^start` matches "start" at the beginning of a string.
- **`$`**: Anchors the match to the end of a string.
  - Example: `end$` matches "end" at the end of a string.

### Word Boundaries
- **`\b`**: Matches a word boundary (between a word and a non-word character).
  - Example: `\bword\b` matches "word" as a whole word.
- **`\B`**: Matches a non-word boundary.
  - Example: `\Bword\B` matches "word" within another word, like "password".

### Examples
- `^The`: Matches "The" at the start of a string.
- `end$`: Matches "end" at the end of a string.
- `\bcat\b`: Matches "cat" as a whole word, not in "scatter".

## 6. Lookaheads and Lookbehinds

### Lookaheads
- **Positive Lookahead (`?=`)**: Asserts that what follows matches a specified pattern.
  - Example: `a(?=b)` matches "a" only if followed by "b".
- **Negative Lookahead (`?!`)**: Asserts that what follows does not match a specified pattern.
  - Example: `a(?!b)` matches "a" only if not followed by "b".

### Lookbehinds
- **Positive Lookbehind (`?<=`)**: Asserts that what precedes matches a specified pattern.
  - Example: `(?<=b)a` matches "a" only if preceded by "b".
- **Negative Lookbehind (`?<!`)**: Asserts that what precedes does not match a specified pattern.
  - Example: `(?<!b)a` matches "a" only if not preceded by "b".

### Examples
- `a(?=b)`: Matches "a" only if followed by "b".
- `a(?!b)`: Matches "a" only if not followed by "b".
- `(?<=b)a`: Matches "a" only if preceded by "b".
- `(?<!b)a`: Matches "a" only if not preceded by "b".

## 7. Flags and Modifiers

### Common Flags
- **`i`**: Case-insensitive matching.
  - Example: `/abc/i` matches "abc", "ABC", etc.
- **`g`**: Global matching (find all matches).
  - Example: `/abc/g` matches all occurrences of "abc".
- **`m`**: Multiline matching (`^` and `$` match the start and end of lines).
  - Example: `/^abc/m` matches "abc" at the start of any line.
- **`s`**: Dotall mode (`.` matches newline characters).
  - Example: `/a.b/s` matches "a\nb".

### Examples
- `/hello/i`: Matches "hello", "HELLO", "Hello", etc.
- `/world/g`: Finds all occurrences of "world".
- `/^start/m`: Matches "start" at the beginning of any line in a multiline string.

## 8. Examples and Use Cases

### Email Validation
```regex
^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$
```
- Matches: "user@example.com", "user.name+tag@domain.co"
- Explanation: 
  - `^[a-zA-Z0-9._%+-]+`: Start with one or more alphanumeric or special characters.
  - `@`: Followed by an `@` symbol.
  - `[a-zA-Z0-9.-]+`: Domain name with alphanumeric or dots/hyphens.
  - `\.[a-zA-Z]{2,}$`: Ends with a dot followed by 2 or more alphabetic characters.

### URL Matching
```regex
^(https?|ftp)://[^\s/$.?#].[^\s]*$
```
- Matches: "http://example.com", "https://example.org"
- Explanation:
  - `^(https?|ftp)://`:

 Matches "http", "https", or "ftp" followed by `://`.
  - `[^\s/$.?#].[^\s]*$`: Domain name and path without spaces.

### Phone Number Validation
```regex
^\+?(\d{1,3})?[-.\s]?(\d{1,4})?[-.\s]?\d{1,4}[-.\s]?\d{1,9}$
```
- Matches: "+123-456-7890", "123.456.7890"
- Explanation:
  - `^\+?(\d{1,3})?`: Optional international code.
  - `[-.\s]?`: Optional separator (space, dot, or hyphen).
  - `(\d{1,4})?`: Optional area code.
  - `[-.\s]?\d{1,4}[-.\s]?\d{1,9}$`: Main number.

### IP Address Matching
```regex
\b\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}\b
```
- Matches: "192.168.1.1", "127.0.0.1"
- Explanation:
  - `\b`: Word boundary to ensure exact match.
  - `\d{1,3}`: Matches 1 to 3 digits.
  - `\.`: Literal dot.

## Conclusion

Regular expressions are incredibly versatile and can be used for a wide range of text processing tasks. Understanding the basic syntax and components will allow you to construct complex patterns and efficiently match, search, and replace text.

---

