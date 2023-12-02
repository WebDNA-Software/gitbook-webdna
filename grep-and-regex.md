# Grep & Regex

### \[grep] [context](https://docs.webdna.us/contexts-and-tags)

This popular UNIX utility has been adapted to WebDNA. \[grep] replaces text based on a regular expression.

```
[grep search=regexp&replace=regexp] Any Text [/grep]
```

\


**Parameters**

| Parameter  | Description                                                                                   |
| ---------- | --------------------------------------------------------------------------------------------- |
| search     | (Required) Regular expression that defines what text to search for in the body of the context |
| replace    | (Required) Regular expression that defines how to output the resulting text                   |
| ignorecase | (Optional) T/F ignores case sensitivity while performing the grep function                    |

**Example WebDNA code:**

```
[grep search=([0-9]*-[0-9]*)&replace=<strong>\1</strong>]
  Hi, my phone number is 555-1234, and I'd like you to call me
[/grep]
```

Results:\
_Hi, my phone number is **555-1234**, and I'd like you to call me_

Using 'ignorecase' parameter in \[grep] context. Search for 'usa' and replace with 'USA'

**Example WebDNA code:**

```
[grep search=usa&replace=USA&ignorecase=T]
I was born in the usA
I was born in the uSa
I was born in the Usa
[/grep]
```

Results:\
I was born in the USA\
I was born in the USA\
I was born in the USA

Removing html tags from text

**Example WebDNA code:**

```
[grep search=(<[a-z]*>[^<>]*<\/[a-z]*>|<[a-z]*>)&replace=]
  <script>remove the code that's here 1</script> but not here 1 <br> <hr>
  <script>remove the code that's here 2</script> but not here 2 <br> <hr>
  <script>remove the code that's here 3</script> but not here 3 <br> <hr>
[/grep]
```

Results:\
but not here 1 but not here 2 but not here 3

Grep is perhaps one of the most powerful and least understood features of WebDNA. The version of grep that is used in WebDNA is a very basic version, similar to UNIX's egrep. Here are a few tips.

**Characters to Use in Search Patterns**

| Character Typed                    | What it Represents                                                                         | Example                                                                                                                                                                                                                                                                                                                                                                                                                      |
| ---------------------------------- | ------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| any character                      | represents the character typed, with the exception of the special characters defined below | a represents a, b represents b, etc.                                                                                                                                                                                                                                                                                                                                                                                         |
| . (dot)                            | any character (except line breaks)                                                         | will match c, 3, space, etc.                                                                                                                                                                                                                                                                                                                                                                                                 |
| #                                  | any digit                                                                                  | will match 0, 1, 2, 3, 4, 5, 6, 7, 8, or 9                                                                                                                                                                                                                                                                                                                                                                                   |
| \d                                 | any digit                                                                                  | will match 0, 1, 2, 3, 4, 5, 6, 7, 8, or 9 (same as #)                                                                                                                                                                                                                                                                                                                                                                       |
| \D                                 | any non digit                                                                              | will match anything except 0, 1, 2, 3, 4, 5, 6, 7, 8, or 9 (anything not matched by \d)                                                                                                                                                                                                                                                                                                                                      |
| ^                                  | beginning of line                                                                          |                                                                                                                                                                                                                                                                                                                                                                                                                              |
| $                                  | end of line                                                                                |                                                                                                                                                                                                                                                                                                                                                                                                                              |
|                                    | tab                                                                                        |                                                                                                                                                                                                                                                                                                                                                                                                                              |
|                                    | line break (return)                                                                        |                                                                                                                                                                                                                                                                                                                                                                                                                              |
|                                    | newline                                                                                    | matches a Unix line break (line feed)                                                                                                                                                                                                                                                                                                                                                                                        |
| \f                                 | form feed                                                                                  |                                                                                                                                                                                                                                                                                                                                                                                                                              |
| \s                                 | whitespace                                                                                 | matches any whitespace character (space, tab, line break, newline, form feed)                                                                                                                                                                                                                                                                                                                                                |
| \S                                 | non whitespace                                                                             | matches any non whitespace character (anything not matched by \s)                                                                                                                                                                                                                                                                                                                                                            |
| \w                                 | word character                                                                             | matches any word character. According to the release notes: "A word is any run of non-word-break characters bounded by word breaks. Word characters in the ASCII range are generally alphanumeric, and characters whose value is greater than 127 are also considered word characters." Typically this is letters, numbers, and underscores, but also includes some other characters you might not cosinder word characters. |
| \W                                 | non word character                                                                         | matches any non word character (anything not matched by \w)                                                                                                                                                                                                                                                                                                                                                                  |
| \character                         | represents a character that is normally a _special character_                              | \\\ means \\, \\. means ., etc.                                                                                                                                                                                                                                                                                                                                                                                              |
| \[any series of characters]        | represents any of the characters inside the brackets                                       | \[abc] represents an a, b, or c                                                                                                                                                                                                                                                                                                                                                                                              |
| \[any character-another character] | represents any characters within the range of characters specified                         | \[a-c] represents a, b, or c                                                                                                                                                                                                                                                                                                                                                                                                 |
| \[^any series of characters]       | represents any character except the ones specified after the ^                             | \[^c3] represents any character except c or 3                                                                                                                                                                                                                                                                                                                                                                                |

### \[regex] WebDNA v8.1+ [context](https://docs.webdna.us/contexts-and-tags)

\[regex] is a full implementation of \[grep], starting from version 8.1 of WebDNA. The older implementation of \[grep] remains for backward compatibility for its simple search and replace capabilities, however, \[regex] is a better choice.

Syntax is the same as for \[grep], except that it does not have the limitations of WebDNA's implementation of \[grep]

Some specific operators used by \[regex] will be interpreted by WebDNA, like "%2B" or "%26" for instance. In this case, it is safer to \[url]\[/url] the operator like in the following

**Example WebDNA code:**

```
[text]value=\b0%2B[/text]
[regex search=[value]&replace=]080.010.001.305[/regex]
```

or

```
[text]value=\b0+[/text]
[regex search=[url][value][/url]&replace=]080.010.001.305[/regex]
```

Results: both will show 80.10.1.305\
