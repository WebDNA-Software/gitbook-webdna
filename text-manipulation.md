# Text Manipulation

### \[boldwords] [context](https://docs.webdna.us/contexts-and-tags)

Simply wraps \<b> html tags around the text within the \[boldwords] word list context.

```
[boldwords words=wordlist]Any Text[/boldwords]
```

To automatically boldface matching words in portions of a template, put a \[boldwords] context around the text. Any words inside the context found to match words in the _wordlist_ will automatically be wrapped by the html \<b>\</b> tags so they appear bold on the page.

**Example WebDNA code:**

```
[boldwords words=Grant,John]
  John and Grant helped write WebDNA.
  John's cats are displayed on his personal web page.
[/boldwords]
```

Results:

John and Grant helped write WebDNA.\
John's cats are displayed on his personal web page.

Anywhere the words Grant or John appear in the contained text, they are wrapped with \<b>Grant\</b> or \<b>John\</b>, respectively. You may use any \[xxx] tags in the _wordlist_ or the container, as in \[boldwords \[name]]Please embolden this \[name]\[/boldwords]. All \[xxx] tags inside the context are first substituted for their real values, then WebDNA searches for and boldfaces the matching words.

This context is handy for helping visitors easily see what they are searching for in a database. In the following example, the visitor has previously typed some text into a search form with a \<input name="wodescriptiondata"> form variable. The \[foundItems] will loop through all the matching records in the database, and words matching the search text will be displayed boldface in the \[description] field.

**Example WebDNA code:**

```
[founditems]
  [boldwords words=[wodescriptiondata]][description][/boldwords]
[/founditems]
```

A maximum of 50 words is allowed in the _wordlist_. You can put as many words as you like inside the context, and all occurrences of matching words will be bolded, but the list of words to look for is limited to 50.

### \[capitalize] [context](https://docs.webdna.us/contexts-and-tags)

Capitalizes the first letter of all words within the \[capitalize] context.

```
[capitalize]any text[/capitalize]
```

To convert the words of a sentence to capitalized form, put them inside a \[capitalize] context. Only the first letter of each word (any letter following a space) is capitalized. If the remaining letters in the word are uppercase, they will be changed to lowercase. If the word begins with a numeral, the numeral is left unchanged, and the rest of the letters will become lowercase. No attempt is made to be grammatically correct - articles such as "a, an, of" are capitalized just like any other word.

**Example WebDNA code:**

```
[capitalize]Some Text that contains upper - and lower case letters[/capitalize]
[capitalize]HI THERE[/capitalize]
[capitalize]this is my 1st time at bat[/capitalize]
[capitalize]a of on at the in or[/capitalize]
```

Results:

Some Text That Contains Upper - And Lower Case Letters\
Hi There\
This Is My 1st Time At Bat\
A Of On At The In Or

You can use this context to display user-entered text (such as first and last names) so they look more consistent, regardless of how the user typed them.

### \[uppercase] [context](https://docs.webdna.us/contexts-and-tags)

\[uppercase] will change all lower case letters to upper case within the context. The 'charset' optionally can be nominated.

```
[uppercase charset=charset-name]Any Text[/uppercase]
```

To convert certain characters to upper case, place them inside an \[uppercase] context.

**Example WebDNA code:**

```
[uppercase]
  Some Text containing upper - and lower case letters
[/uppercase]
```

Results:

SOME TEXT CONTAINING UPPER - AND LOWER CASE LETTERS

This context is most often used with \[username] and \[password] tags, because many web servers do not allow lower case letters to be sent from the browser's realm protection dialog. By ensuring all usernames/passwords entered into a database are uppercase, you can be sure all browsers and web servers will work properly for password authentication.

You can optionally change the behavior of uppercase with charset=mac or charset=iso, which causes high-ASCII characters to change case differently depending on which character set your data is.

**Example WebDNA code:**

```
[uppercase charset=iso]...some text...[/uppercase]
```

### \[lowercase] WebDNA v4.0+ [context](https://docs.webdna.us/contexts-and-tags)

The \[lowercase] context changes all upper case letters to lower case. The 'charset' optionally can be nominated.

```
[lowercase charset=charset-name]Any Text[/lowercase]
```

**Example WebDNA code:**

```
[lowercase]
  Some Text that contains upper - and lower case letters
[/lowercase]
```

Results:

some text that contains upper - and lower case letters

This context is included for completeness as a complement to \[uppercase].

You can optionally change the behavior of uppercase with charset=mac or charset=iso, which causes high-ASCII characters to change case differently depending on which character set your data is.

**Example WebDNA code:**

```
[lowercase charset=iso]...some text...[/lowercase]
```

### \[convertchars] [context](https://docs.webdna.us/contexts-and-tags)

\[convertchars] along with \[url] and \[input] assure proper formatting and interpretation of text.

\[convertchars] is a useful context for converting characters, and especially valuable because it enables the ability of defining custom conversions. It is used primarily for displaying database values on the browser page, and will make unsure upper ASCII characters display properly. Used without a specified database, it will convert a short list of common characters, such as curly quotes into straight quotes, and < and > into their html counterparts, this will prevent users from forcing html code into forms. When used without a nominated database, the conversions are sourced from the StandardConversions.db in the globals folder. This database may be customised to suit a project's needs.

```
[convertchars db=somedb.db][value][/convertchars]
```

\


**Parameters**

| Parameter  | Description                                                                                                                                                                                                                                         |
| ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| db         | Path to conversion database which contains list of "from" and "to" conversions of words to other words. If not defined, the StandardConversions.db in the globals folder is used. In a custom database the two headers FROM and TO must be defined. |
| table      | In place of a db file, a WebDNA \[table] may be used.                                                                                                                                                                                               |
| case       | (Optional) T or F to indicate that word comparisons should be case-sensitive or not. Default is F, case-insensitive                                                                                                                                 |
| word       | (Optional) SS, WW, SW to indicate that words should be matched as SubString, Whole Word, or Start of Word, in the same way as when matching text in a database \[search] is used.                                                                   |
| delimiters | <p>New from WebDNA 5.1<br>(Optional) List of single characters which define word boundaries.</p>                                                                                                                                                    |

\[convertchars] secondary use is to clean data before inserting into a database, or 'cleaning' a file name when uploading and ensuring that it does not have any 'illegal' characters.

**Example WebDNA code:**

```
[convertchars]New Red Shoes[/convertchars]
[text]cleanfilename=[convertchars db=noillegals.db][filename][/convertchars][/text]
```

A conversion database can be called anything and located anywhere,\
but must have these two specified fields: FROM and TO

Using this example WebDNA table named _convert_ it is possible to convert the word "café" to "cafe"

```
[table name=convert&fields=from,to]
é    e
è    e
ê    e
ë    e
[/table]
```

**Example WebDNA code:**

```
[convertchars table=convert]café[/convertchars]
```

Results:

cafe

Some handy uses for \[convertwords] include removing foul language from online message boards, spelling out acronyms, changing nicknames to full names, inserting hyperlinks, and expanding glossary terms.

**Example WebDNA database:**

```
-- conversion.db --
```

\[convertchars] is excellent for replacing one character with one or multiple characters. Replacing a multiple character string with one or more other characters, then \[convertwords] is the tool of choice.

\[convertchars] does two relevant things differently from \[convertwords]:

1\) \[convertchars] automatically UNURLs the TO and FROM fields in the table, so "%26" is treated as an "&" character rather than the literal "%26"\
2\) \[convertchars] treats a "+" in the TO field as a space

There are reasons for these differences. The UNURLing of TO and FROM allows specification of invisible characters that you may need to strip out of or add to a data source. The + to SPACE mapping is a bit more esoteric, but I would bet the internal WebDNA program uses (or maybe "used") the same block of code for interpreting query parameters in a URL - the stuff after the ? - where spaces are mapped to + characters by the browser. For example "http://example.com/thispage.dna?user+name=Jane+Doe" gets a formvariable named "user name" with a value of "Jane Doe".

To illustrate convertchars vs. convertwords, I spent time working out these examples...

**Example WebDNA code:**

```
[table name=t1&fields=from,to]
& %26
[/table]
CHARS: [convertchars table=t1]L John & Joanna + Jane[/convertchars]
WORDS: [convertwords table=t1]L John & Joanna + Jane[/convertwords]
```

**Example result:**

```

CHARS: L John & Joanna + Jane
WORDS: L John %26 Joanna + Jane
```

Notes:\
The & was replaced in both cases, but for convertchars it was replaced with & instead of literally %26\


**Example WebDNA code:**

```
[table name=t1&fields=from,to]
%26 %2526
[/table]
CHARS: [convertchars table=t1]L John & Joanna + Jane[/convertchars]
WORDS: [convertwords table=t1]L John & Joanna + Jane[/convertwords]
```

**Example result:**

```
CHARS: L John %26 Joanna + Jane
WORDS: L John & Joanna + Jane
```

Notes:\
convertchars treats the "to" value of "%26" as the character "&", and the "from" value of "%2526" as "%26" (after unurling %25 to %)\
convertwords treated "%26" literally as "%26", so it did not find and replace the & character\


**Example WebDNA code:**

```
[table name=t1&fields=from,to]
+ %252B
[/table]
CHARS: [convertchars table=t1]L John & Joanna + Jane[/convertchars]
WORDS: [convertwords table=t1]L John & Joanna + Jane[/convertwords]
```

**Example result:**

```
CHARS: L%2BJohn%2B&%2BJoanna%2B+%2BJane
WORDS: L John & Joanna %252B Jane
```

Notes:\
convertchars treats the "to" value of "+" as the SPACE character, so all spaces were replaced with %2526, unurled to %2B, and the "+" character was not replaced\
convertwords treated the to and from as entered, replacing "+" with, literally, "%2526"\


**Example WebDNA code:**

```
[table name=t1&fields=from,to]
%2B %252B
[/table]
CHARS: [convertchars table=t1]L John & Joanna + Jane[/convertchars]
WORDS: [convertwords table=t1]L John & Joanna + Jane[/convertwords]
```

**Example result:**

```
CHARS: L John & Joanna %2B Jane
WORDS: L John & Joanna + Jane
```

Notes:\
convertchars treats the "to" value of "%2B" as the "+" character, so "+" was replaced with %2526, unurled to %2B, and spaces were not replaced\
convertwords found no literal "%2B" strings to replace\


**Example WebDNA code:**

```
[table name=t1&fields=from,to]
' %27
+ %2B
# %23
% %25
& %26
! %21
" %22
[/table]
CHARS: [convertchars table=t1]L John & Joanna + Jane[/convertchars]
WORDS: [convertwords table=t1]L John & Joanna + Jane[/convertwords]
```

**Example result:**

```
CHARS: L+John+&+Joanna+++Jane
WORDS: L John %26 Joanna %2B Jane
```

Notes:\
This is Michael's original example (with " + Jane" added to the source string).\
convertchars treats the "to" value of "+" as the SPACE character, so:\
all spaces were replaced with %2B, unurled to "+"\
the "&" character was replaced with %26, unurled back to the "&" character\
the "+" character was not replaced\
convertwords treated the to and from as entered, so:\
spaces were not changed\
the "&" character was replaced with %26\
the "+" character was replaced with %2B\
So, in this case, the results of convertwords represent the desired outcome\


**Example WebDNA code:**

```
[table name=t1&fields=from,to]
' %2527
%2B %252B
# %2523
% %2525
& %2526
! %2521
" %2522
[/table]
CHARS: [convertchars table=t1]L John & Joanna + Jane[/convertchars]
WORDS: [convertwords table=t1]L John & Joanna + Jane[/convertwords]
```

**Example result:**

```
CHARS: L John %26 Joanna %2B Jane
WORDS: L John %2526 Joanna + Jane
```

Notes:\
From example 5, the "+" from value was replaced with "%2B" and the from "%" characters were replaced with "%25"\
convertchars treats the "to" value of "%2B" as the "+" character, so:\
all spaces were left alone\
the "&" character was replaced with the desired "%26", after unurling "%25" to "%"\
the "+" character was replaced with the desired "%2B"\
convertwords treated the to and from as entered, so:\
spaces were not changed\
the "&" character was replaced with %2526 (not the desired %26)\
the "+" character was not replaced\
So, in this case, the results of convertchars represent the desired outcome\


**Example WebDNA code:**

```
[grep search=[url]%20[/url]&replace= ][url]L John & Joanna + Jane[/url][/grep]
```

**Example result:**

```
L John %26 Joanna %2B Jane
```

Notes:\
I'm not sure of the specifics of the desired mapping beyond what is shown, but if it is to URL everything except space characters, as it appears, then using the built in URL then reverting %20 to the space character does the trick in one line.\


### \[countchars] [context](https://docs.webdna.us/contexts-and-tags)

Counts the number of letters inside the context.

```
[countchars]Any Text[/countchars]
```

\


**Example WebDNA code:**

```
[countchars]Some Text[/countchars]
```

**Example result:**

```
9
```

\[countchars] counts spaces, tabs returns, carriage, line feeds and other characters that are sometimes invisible - literally every keystroke that has been used to create the content to be counted

### \[getchars] [context](https://docs.webdna.us/contexts-and-tags)

Extracts a portion of the text (also known as substring or Mid$).

**Parameters**

| Parameter | Description                                                                                                                                      |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| start     | Index placement of the first character to return.                                                                                                |
| end       | (Optional) The index placement of the last character to return. If no end is specified, all of the text up to the end of the string is returned. |
| from      | (Optional) Set to "end" to get characters from the ending of the string instead of the beginning.                                                |
| Trim      | (Optional) To remove leading and trailing white space in a given text string.                                                                    |

The parameters above are optional to the \[getchars] context

To display a subsection of text, place the text inside a \[getchars] context.

```
[getchars start=x&end=y]Any Text[/getchars]
```

**Example WebDNA code:**

```
[getchars start=14&end=18]Hello there, Edwina[/getchars]
```

**Example result:**

```
Edwin
```

The example above returns "Edwin," which is the sequence of letters starting at the 14th position and extending to the 18th position. Extracting sub-portions of text is useful when you need to get text from within files.

```
[getchars start=1&end=2&from=end]Hello there, Edwina[/getchars]
```

**Example result:**

```
na
```

The example above returns "na" which is the sequence of letters starting at the 1st position from the end and extending to the 2nd position from the end.

To display a subsection of some text, put the text inside a \[getchars] context. Most often, the contents of the \[getchars] context will be the value of an input field or database field that WebDNA will replace.

**Example #1**

```
[getchars start=1&end=2][field1][/getchars]
```

**Example #2**

```
[getchars start=14&end=18]Hello there, Edwina[/getchars]
```

Example 2 returns "Edwin", which is the sequence of letters starting at the 14th position and extending to the 18th position. Extracting sub-portions of text is useful when you need to get text from files.

**Example #3**

```
[getchars start=1&end=2&from=end]Hello there, Edwina[/getchars]
```

Example 3 returns "na", which is the sequence of letters starting at the 1st position from the end and extending to the 2nd position from the end.

You can use the new 'TRIM=Right/Left/Both' parameter for the \[GetChars] context to remove leading and trailing white space in a given text string.

**Example WebDNA code:**

```
[text]test_string=
This is a test. 
[/text]Example input string: "[test_string]"Remove leading white space:
-[getchars start=1&end=&trim=left][test_string][/getchars]-Remove trailing white space:
-[getchars start=1&end=&trim=right][test_string][/getchars]-Remove leading and trailing white space:
-[getchars start=1&end=&trim=both][test_string][/getchars]-Remove leading and trailing white space, and retrieve characters in the 1-4 positions:
-[getchars start=1&end=4&trim=both][test_string][/getchars]-Remove leading and trailing white space, and retrieve characters in the 1-5 positions, from the end:
-[getchars start=1&end=5&trim=both&from=end][test_string][/getchars]-
```

**Example result**

```
Example input string: "This is a test."Remove leading white space:
-This is a test. -Remove trailing white space:
-This is a test.-Remove leading and trailing white space:
-This is a test.-Remove leading and trailing white space, and retrieve characters in the 1-4 positions:
-This-Remove leading and trailing white space, and retrieve characters in the 1-5 positions, from the end:
-test.-
```

Note that you still need to supply the 'start' and 'end' parameters

\[getchars] or \[listchars] have never been multi-byte capable. The code assumes every character is a single byte. We produce a multi-byte capable version of \[listchars] and \[getchars]. They are named differently, \[mblistchars] and \[mbgetchars]. This way they don't mess up existing sites that aren't expecting them to be used on UTF-8

### \[listchars] WebDNA v6.0++ [context](https://docs.webdna.us/contexts-and-tags)

Breaks a string of text into separate characters.

```
[listchars Chars]Some Text[/listchars]
```

\


**Parameters**

| Parameter | Description                                                                                                                                                                                                                                                                                                                               |
| --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| chars     | (Required) string of text to break up.                                                                                                                                                                                                                                                                                                    |
| from      | (Optional) Sets the direction the 'chars' string is parsed. Default is 'START'. If set to 'END' , characters will be listed starting from the end of the 'chars' string, to the beginning.                                                                                                                                                |
| start     | (Optional) Sets the start posotion, in the 'chars' string, from which to list the characters. If not specified, characters will be listed starting from the first position in the string. If the 'From' parameter is set to 'END', the 'start' position is relative to the end of the 'chars' string.                                     |
| end       | (Optional) Sets the end posotion, in the 'chars' string, from which to stop listing the characters. If not specified, characters will be listed starting from the 'start' position, and up to the last position in the string. If the 'From' parameter is set to 'END', the 'last' position is relative to the end of the 'chars' string. |

The following tags are available inside a \[listchars] context:

| Tag         | Description                                                                                                                                                                                                                                                               |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \[index]    | A number from 1 to the total number of chars in the text indicating the iteration index.                                                                                                                                                                                  |
| \[char]     | The current character of the 'chars' string being iterated.                                                                                                                                                                                                               |
| \[position] | The current character position. The position is '1' based, and is relative to the start of the 'chars' string, unless the from parameters is set to 'END', in which case 'position' is relative to the end of the 'chars' string.                                         |
| \[break]    | From version 8.1, if the \[listchars] context sees the \[break] tag while executing a loop, it will stop looping, once it finishes the current loop. Thus the \[break] tag should only appear in a \[showif] statement that is evaluated at the end (bottom) of the loop. |

To display a list of all the separate characters in a string, use a \[listchars] context.

**Example WebDNA code:**

```
[listchars chars=WebDNA 6.0]
[index]: [char]
[/listchars]
```

**Example result**

```
1: W
2: e
3: b
4: D
5: N
6: A
7:
8: 6
9: .
10: 0
```

The following parameters are used in the \[listchars] context:\
**Example WebDNA code:**

```
[listchars chars=1234567890.abcdefg&start=4&end=10&from=end] Character at string position [position] = '[char]'
[/listchars]
```

**Example result**

```
Character at string position 4 = 'd'
Character at string position 5 = 'c'
Character at string position 6 = 'b'
Character at string position 7 = 'a'
Character at string position 8 = '.'
Character at string position 9 = '0'
Character at string position 10 = '9
```

This contest is more powerful than it looks; you can use it as a \[loop] and introduce other contexts in it. See the example below

**Example WebDNA code:**

```
[listchars chars=abcdefg&start=1&end=7]
[append db=base.db]ref=[position]&character=[char][/append]
[/listchars]
```

\[getchars] or \[listchars] have never been multi-byte capable. The code assumes every character is a single byte. We produce a multi-byte capable version of \[listchars] and \[getchars]. They are named differently, \[mblistchars] and \[mbgetchars]. This way they don't mess up existing sites that aren't expecting them to be used on UTF-8

\


### \[convertwords] WebDNA v4.0+ [context](https://docs.webdna.us/contexts-and-tags)

Changes specified words in a string of text to different words, based on a database of conversions.

```
[convertwords db=xx.db]Any Text[/convertwords]
```

\


**Parameters**

| Parameter  | Description                                                                                                                                                                                                                           |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| db         | (Required) path to conversion database which contains list of "from" and "to" conversions of words to other words                                                                                                                     |
| table      | In place of a db file, you can specify a named reference to a WebDNA \[table] object.                                                                                                                                                 |
| case       | (Optional) **T** or **F** to indicate that word comparisons should be case-sensitive or not. Default is F, case-insensitive                                                                                                           |
| word       | (Optional) **SS**, **WW**, **SW** to indicate that words should be matched as **S**ub**S**tring, **W**hole **W**ord, or **S**tart of **W**ord, same as \[search] parameters (word breaks subchapter) when matching text in a database |
| delimiters | (Optional) List of single characters which define word boundaries.                                                                                                                                                                    |

To automatically convert certain words into other words, create a database of words to be changed, and put the text to be converted inside a \[convertwords] context. Any matching words will be changed to corresponding words in the database lookup table.

**Example WebDNA code:**

```
[convertwords db=glossary.db]
A HTTP request first uses DNS to look up the ip address
[/convertwords]
```

The above line of WebDNA would produce the following (given the proper glossary.db):

_A Hypertext Transport Protocol request first uses Domain Name System to look up the Internet Protocol address_

Here's what the glossary.db file would look like for the example above:

**Example result:**

```
-- glossary.db --
from      to
HTTP      Hypertext Transport Protocol
DNS       Dynamic Naming System
ip        Internet Protocol
```

Anywhere the words in the from column appear in the text, they are replaced with whatever is in the to column. There is no limit to the length text in either the from or to columns. You may put any kind of text into either column; for example, HTML is legal in either column.

All the dynamic links in this website are dynamically generated, based on the document titles; a \[table] is first generated with all document titles to dynamically create links:

**Example WebDNA code:**

```
[table name=t2&fields=from,to]
[search db=base.db&netitledata=[blank]][founditems]
[title]          <a href="page.dna?numero=[ref]">[title]</a>h
[/founditems][/search]
[/table]
```

This way, \[search] would automatically be associated with \<a href="page.dna?numero=69">\[search]\</a>

Then we use

**Example WebDNA code:**

```
[convertwords table=t2][text][/convertwords]
```

to replace all the titles that could appear in the text with a link: all the technical titles in this website are now properly and dynamically linked.

Some handy uses for \[convertwords] include removing foul language from online message boards, spelling out acronyms, changing nicknames to full names, inserting hyperlinks, and expanding glossary terms.

### \[countwords] WebDNA v3.0+ [context](https://docs.webdna.us/contexts-and-tags)

The number displayed is the number of words inside the context

```
[CountWords]Any Text[/CountWords]
```

Counts the number of words inside the context.

To count the number of words in something, put the text inside a \[CountWords] context.

Example (normally you would put the following text into a .tpl file on your server and use a web browser to link to it):

**Example WebDNA code:**

```
[countwords delimiters= ,]This is a big long sentence, don't you think?[/countwords]
```

In the example above, the displayed text will be:

**Example result:**

```
9
```

The number displayed is the number of words inside the context -- the number of words found depends on the delimiters you specify. If you specify spaces, commas, and periods as delimiters, then those characters will not be counted, and words will be defined as any text that is not a delimiter. Long runs of delimiters are ignored, so more than one space between words will not increase the word count.

If you want to count the number of lines in a multi-line string (such as the number of paragraphs in a story,) you can specify carriage return as the delimiter

**Example WebDNA code:**

```
[countwords delimiters=%0D]first line
second line
third line
[/countwords]
```

This will return 3, where the 'words' are actually defined as whole lines of text. Blank lines do not count. %0D is the hexadecimal equivalent to carriage return. You could type a carriage return in the Delimiters parameter, but for formatting reasons in this document we use the equivalent hexadecimal notation.

### \[listwords] WebDNA v3.0+ [context](https://docs.webdna.us/contexts-and-tags)

Breaks a string of text into separate words.

```
[listwords Words]Some Text[/listwords]
```

\


**Parameters**

| Parameter  | Description                                                                                                                                                                                                                              |
| ---------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| words      | (Required) string of text to break up.                                                                                                                                                                                                   |
| delimiters | (Optional) List of single characters which define word boundaries. Defaults to space, comma, and dot.                                                                                                                                    |
| tabs       | (Optional, Rare) Setting Tabs=T causes the list of words to break at tab boundaries only, and _runs of tabs_ are not collapsed. This assists in parsing special formats where two tabs in a row are important and should not be skipped. |

The following tags are available inside a \[listwords] context:

| Tag          | Description                                                                                                                                                                                                                                                |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \[index]     | A number from 1 to the total number of words in the text indicating the word's index position in the text.                                                                                                                                                 |
| \[word]      | Text of the word currently being displayed.                                                                                                                                                                                                                |
| \[delimiter] | Text of the delimiter(s) which caused this word to break.                                                                                                                                                                                                  |
| \[break]     | From version 8.1, if the \[listwords] context sees the \[break] tag while executing, it will stop, once it finishes the current loop. Thus the \[break] tag should only appear in a \[showif] statement that is evaluated at the end (bottom) of the loop. |

To display a list of all the separate words in a string, use a \[listwords] context. You may optionally specify the delimiters defining the boundaries of words (usually spaces, commas, and periods).

Example (normally you would put the following text into a .tpl file on your server and use a web browser to link to it):

**Example WebDNA code:**

```
[listwords Words=This is a big long sentence]
[index]: [word]
[/listwords]
```

**Example result:**

```
1: This
2: is
3: a
4: big
5: long
6: sentence
```

**Example WebDNA code:**

```
[listwords Words=Hello.      My name is Carl!&Delimiters= ,!]
[index]: [word]
[/listwords]
```

The example above yields the following. Notice the "." at the end of "Hello." is shown as part of the word, because "." was not specified in the list of delimiters. Also notice the long run of spaces in a row is collapsed and does not appear in the word list.

**Example result:**

```
1: Hello.
2: My
3: name
4: is
5: Carl
```

### \[findstring] WebDNA v6.0+ [tag](https://docs.webdna.us/contexts-and-tags)

You can use the \[findstring] global tag to find the location of a sub-string within a given source string.

```
[FindString Source=...&Find=...&MatchCase=...&Reverse=...&StartAt=...]
```

\


**Parameters**

| Parameter | Description                                                                                                                                                                                                                                                                                    |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Source    | (Required) The character string to search.                                                                                                                                                                                                                                                     |
| Find      | (Required) The character string to find.                                                                                                                                                                                                                                                       |
| MatchCase | (Optional) T/F - Default is 'F'. If set to 'T', the search will be case sensitive.                                                                                                                                                                                                             |
| Reverse   | (Optional) T/F - Default is 'F'. If set to 'T', the search is performed starting from the end of the 'source' string.                                                                                                                                                                          |
| StartAt   | (Optional) Sets the start position, in the 'source' string, from which to start the search. If not specified, the search will start from the beginning of the 'source' string. If the 'Reverse' parameter is set to 'T', the 'StartAt' position is relative to the end of the 'source' string. |

**Example WebDNA code:**

```
[text]mystring=WebDNA is a powerful, easy to learn, scripting language.[/text]
[text]found_at=[FindString source=[mystring]&find=powerful][/text]
[showif [found]>0]
Found 'powerful' in source string at character position [found_at]
[/showif][hideif [found]>0]
String 'powerful' not found.
[/hideif]
```

**Example result:**

```
Found 'powerful' in source string at character position 13
```

### \[middle] WebDNA v3.0+ [context](https://docs.webdna.us/contexts-and-tags)

Extracts middle portion of the text between any two strings.

```
[middle startafter=x&endbefore=y]Any Text[/middle]
```

To display a subsection of some text, put the text inside a \[middle] context.

**Parameters**

| Parameter   | Description                                                                                                                                                                                                                                                     |
| ----------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| name        | (optional) The name of the cookie to list.                                                                                                                                                                                                                      |
| startafter  | String of text characters to search for defining the beginning of the text to be returned. All preceding text (and the StartAfter text itself) will be ignored.                                                                                                 |
| endbefore   | String of text characters to search for defining the end of the text to be returned. If several occurence of the search string appear in the text, then the last one only will be relevant. All following text (and the EndBefore text itself) will be ignored. |
| startbefore | v8.2+ String of text characters to search for defining the beginning of the text to be returned. All preceding text will be ignored.                                                                                                                            |
| endafter    | v8.2+ String of text characters to search for defining the end of the text to be returned. All following text will be ignored.                                                                                                                                  |
| startcount  | v8.2+ Can be positive or negative. Provides a means of starting from the _nth_ occurrence of the startafter or endbefore delimiter. Default = 1.                                                                                                                |
| endcount    | v8.2+ Can be positive or negative. Provides a means of ending from the _nth_ occurrence of the startafter or endbefore delimiter. Default = 1.                                                                                                                  |

**Example WebDNA code:**

```
[middle startafter=<body>&endbefore=</body>]
  <html>
    <head>
    </head>
    <body>
      Hi There
    </body>
  </html>
[/middle]>
```

Results:\
The example above returns "Hi There," which is the sequence of letters between "\<body>" and the last occurence of "\</body>." Extracting sub-portions of text like this is useful when you need to remove the HTML header information from a file (or web page) containing HTML that you want to include inside the body of a web page.

**Example WebDNA code:**\
Breaking up a string of text

```
[text]myVAR=abc/def/ghi/jkl/&mno/pqr/>/stu/vwx/>yz?[/text][middle startafter=f/&endbefore=/j][myVAR][/middle]
[middle startafter=f/&endbefore=/][myVAR][/middle]
[middle startafter=f/&endbefore=[url]&[/url]][myVAR][/middle]
[middle startafter=f/&endbefore=&][myVAR][/middle]
[middle startafter=f/&endbefore=/>][myVAR][/middle]
```

Results:\
ghi\
ghi/jkl/\&mno/pqr/>/stu/vwx\
ghi/jkl/\
ghi/jkl/\&mno/pqr/>/stu/vwx/>yz?\
ghi/jkl/\&mno/pqr/>/stu/vwx

**From WebDNA v8.2**\
startcount and endcount provide a means of starting/ending from the _nth_ occurrence of the startafter or endbefore delimiter. The value can be positive or negative. A negative number counts from the opposite end it normally counts from. Positive numbers always count from the beginning. If you do not provide the startcount/endcount, the default is 1.\
A startcount of -2 means "find the startafter/startbefore that is 2nd from the end of the string". Likewise, giving endcount of -2 means "find the endbefore/endafter that is 2nd from the beginning of the string".\
When something isn't found, or if it isn't found _n_ times, then it is ignored.\
For example, \[middle startafter=q\&startcount=10]xyz\[/middle] will return xyz. This is the behavior of \[middle] in previous versions, to retains compatibility.

**Example WebDNA code:**

```
[middle startafter=Subject: &endbefore=%0A&endcount=-1]
Message-Id: <1874F5DF-301E-4560-8C37-63BD903EA7E4@webdna.us>
From: John Doe 
To: talk@webdna.us
Content-Type: text/plain; charset=US-ASCII; format=flowed
Content-Transfer-Encoding: 7bit
Mime-Version: 1.0 (Apple Message framework v924)
Subject: [WebDNA] test messageHello, this is a test
[/middle]
```

Results:\
\[WebDNA] test message\
(WebDNA counts from the first line ending after the Subject:)

**Example WebDNA code:**

```
1) [middle startbefore=f/&endafter=/j][myVar][/middle]
2) [middle startafter=/&startcount=2][myVar][/middle]
3) [middle endbefore=/&endcount=3][myVar][/middle]
4) [middle endbefore=/&endcount=-3][myVar][/middle]
5) [middle startbefore=/&endafter=/][myVar][/middle]
6) [middle startafter=f/][myVar][/middle]
7) [middle endbefore=/&startcount=5][myVar][/middle]
8) [middle startbefore=f/][myVar][/middle]
```

Results:\
1\) f/ghi/j\
2\) ghi/jkl/\&mno/pqr/>/stu/vwx/>yz?\
3\) abc/def/ghi/jkl/\&mno/pqr/>\
4\) abc/def/ghi\
5\) /def/ghi/jkl/\&mno/pqr/>/stu/vwx/\
6\) ghi/jkl/\&mno/pqr/>/stu/vwx/>yz?\
7\) abc/def/ghi/jkl/\&mno/pqr/>/stu/vwx\
8\) f/ghi/jkl/\&mno/pqr/>/stu/vwx/>yz?

Note result #7 the "startcount" is ignored because startcount is only used with "startbefore" and "startafter". Since no startbefore or startafter is specified, there is nothing to use startcount with. The default "endcount" of 1 is used instead since "endbefore" is specified.

### \[raw] [context](https://docs.webdna.us/contexts-and-tags)

Displays enclosed text without interpreting the \[xxx] tags in any way.

```
[raw] Any Text, including [xxx][ /raw]
```

To prevent WebDNA from interpreting \[xxx] tags, enclose the text inside a \[raw] context. The text inside the container is displayed unchanged until the \[ /raw] tag is found. Unlike most other WebDNA contexts, you cannot nest \[raw] contexts inside of each other.

**Example WebDNA code:**

```
[raw]Any text, including any [xxx] tags such as [date] or [delete] [ /raw]
```

In the example above, the displayed text will be

Any text, including any \[xxx] tags such as \[date] or \[delete]

This context is most often used within any HTML pages on your site that have many \[] characters may be misinterpreted by WebDNA, so surround those pages with \<!-- \[ Raw]-->...entire text of page...\<!-- \[ /Raw]-->. Notice the \<!-- --> HTML comments in this example: they are used so that if this page is ever displayed without being processed by WebDNA, the \[ raw] tags will be invisible.

### \[url] [context](https://docs.webdna.us/contexts-and-tags)

\[url], \[input], and \[convertchars] assure proper formatting and interpretation of text

```
[url][value][/url]
```

\


\[url], \[input], and \[convertchars] are used to prevent certain characters from being misinterpreted. You use each of them in specific cases, which are easily confused.

Here are the problematic characters:\
WebDNA: &, =, ^, \~, !,\\, / \[, ], and anything else used as code\
URLs: Only alphanumeric values, - and \_ are allowed\
Platform differences: Unix, Windows and Mac all have their own upper ASCII mapping, such as curly quotes, em dashes, degree signs, etc.

**\[url]**\[value]**\[/url]**\
URL converts characters to legal URL characters, such as a space to %20. When WebDNA works with \[url]'d values, it automatically decodes them, so no worries about ending up with a bunch of percent signs everywhere.

**Where to use:** \[search], \[replace], \[append], links, \[showif], \[hideif], \[listwords], or any place you are working with values that the _user inputs_ and you have no control over, or variables from databases where the value may contain problematic characters (e.g., item descriptions). If you are using guaranteed clean values, such as dates, Y/N values, numbers, or SKUs, you have nothing to worry about.

It is common in form submission validation to ask if a value is blank by using \[showif \[value]=] However, if \[value] would contain an !, the exclamation point would be interpreted to mean "does not equal", and therefore the expression would be true, exactly the opposite of what was intended. \[showif \[url]\[value]\[/url]=] prevents this.

**Example WebDNA code:**

```
[replace db=yourdb.db&eqrecordIDdata=[recordID]]title=[url][title][/url]
&desc=[url][desc][/url]&datestamp=[date][/replace]
```

(\[date] is not URL'd because we know it will be clean.)

```
[showif [url][subjectline][/url]=]Error! You must enter a subject line[/showif]
```

the \[text] does not need \[url]; however, if using \[text multi=t] then you can't use a value that could contain an &. You would set that variable in a single \[text] context.

### \[unurl] WebDNA v3.0+ [context](https://docs.webdna.us/contexts-and-tags)

Changes text from URL-compatible characters to plain text.

```
[unurl]Any URL Text[/unurl]
```

\


To automatically convert text containing URL characters such as %20, %3A, etc, put it inside a \[UnURL] context. Certain letters, such as spaces, colons, and the equals sign (=) are not allowed inside URLs unless they are first converted to hexadecimal form -- the UnURL context converts them back. This is the opposite of \[url].

Example (normally you would put the following text into a .tpl file on your server and use a web browser to link to it):

**Example WebDNA code:**

```
[unurl]Filename%20with%20spaces.gif[/unurl]
```

In the example above, the displayed text will be

**Example result:**

```
Filename with spaces.gif
```

By default, the \[unurl] context will not convert hex codes that contain lowercase letters. So, %3A will be converted to a colon while %3a will not. You can use the 'ignorecase' parameter to force the \[unurl] context to convert hex codes containing lowercase letters. So \[unurl ignorecase =T]%3a\[/unurl] will convert %3a to a colon.

This context is rarely needed, because most of the time WebDNA has already converted the text (in URL parameters, for instance) back to plain text. If you plan to store text with embedded unusual characters such as tabs or carriage returns into a field of a database, you might use \[url] to store them and \[unurl] to retrieve them.

### \[listpath] [context](https://docs.webdna.us/contexts-and-tags)

Breaks a path into separate foldernames and a filename.

```
[listpath Path]Path Tags[/listpath]
```

\


**Parameters**

| Parameter | Description                                                                                                      |
| --------- | ---------------------------------------------------------------------------------------------------------------- |
|           |                                                                                                                  |
| path      | (Required) Path text to break apart.                                                                             |
| FileOnly  | (Optional) Set to "T" if you only want to display the filename at the end of the path.                           |
| PathOnly  | (Optional) Set to "T" if you want to display only the folder names, but not the filename at the end of the path. |

| Tag         | Description                                                                                                                                                                                                                                                              |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
|  \[index]   | A number from 1 to the total number of names in the path text indicating the item's index position in the list.                                                                                                                                                          |
| \[name]     | Text of the path piece currently being displayed. This can either be a folder name or a filename, depending on the placement of the text in the path (the text after the last "/" is considered to be the filename).                                                     |
| \[filename] | Text of the filename, leaving off the folder names leading to it.                                                                                                                                                                                                        |
| \[break]    | From version 8.1, if the \[listpath] context sees the \[break] tag while executing a loop, it will stop looping, once it finishes the current loop. Thus the \[break] tag should only appear in a \[showif] statement that is evaluated at the end (bottom) of the loop. |

To display a list of all the separate folder names in a path (text separated by "/"), use a \[listpath] context. You may optionally specify that only the filename will be displayed (subtracting any path leading to it), or only the folder names (subtracting the filename from the end).

ListPath is only used for text-manipulation -- it does not actually look at your hard disk. If you want to find all the files in a folder on your hard disk, use \[listfiles] instead.

**Example WebDNA code:**

```
[listpath path=/folder1/folder2/folder3/filename.txt]
Path part #[index]: [name][/listpath]
```

**Example result:**

```
Path part #1: folder1
Path part #2: folder2
Path part #3: folder3
Path part #4: filename.txt
```

**Example WebDNA code:**

```
[listpath path=/folder1/folder2/folder3/filename.txt&FileOnly=T]
Path part #[index]: [name][/listpath]
```

**Example result:**\
Path part #1: filename.txt

```
[listpath path=/folder1/folder2/folder3/filename.txt&PathOnly=T]
Path part #[index]: [name]
[/listpath]
```

**Example result:**

```
Path part #1: folder1
Path part #2: folder2
Path part #3: folder3
```

### \[input] [context](https://docs.webdna.us/contexts-and-tags)

\[url], \[input], and \[convertchars] assure proper formatting and interpretation of text

<pre><code><strong>[input][value][/input]
</strong></code></pre>

INPUT is used to clean data like carriage returns and quote marks, and is used, as the name implies, in HTML input form tags.

1\. _Textarea_ form field. When carriage returns and tabs are written to a database, they are stored as special characters so as not to interfere with the tab-delimited structure of the database file itself. When bringing them out again into a textarea, \[input] will format them correctly back into tabs and returns.\
2\. Other _input tags_ in the value parameter. If your value contains a quote mark, it will close the form field data at that point, resulting in a chopped off value.\
3\. Other HTML tags (esp. alt tags), javascripts, anything in quotes; same idea as above.

**Example WebDNA code:**

```
<textarea>[input][desc][/input]</textarea>
<input type="text" name="desc" value="[input][desc][/input]">
<img source="yourimg.jpg" alt="[input][desc][/input]">
```

### \[removehtml] WebDNA v4.0+ [context](https://docs.webdna.us/contexts-and-tags)

Removes HTML or WebDNA tags from a string of text.

```
[removehtml]Any Text[/removehtml]
```

\


**Parameters**

| Parameter   | Description                                                                                                                                                          |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Source      | (Optional) T or F to indicate that you also want to remove uninterpreted WebDNA from the text. HTML is always removed from the text. Default is to remove HTML only. |
| ReplaceWith | Text with which you want to replace the HTML sequences. Defaults to nothing.                                                                                         |

To automatically strip out HTML or WebDNA tags from text, put the text inside a \[removehtml] context. Any HTML found in the text will be removed, leaving just the plain non-HTML text behind. Alternately you can specify that WebDNA be removed instead.

**Example WebDNA code:**

```
[removehtml]
Some Text that contains <br> <h3>or</h3> other HTML
[/removehtml]
```

**Example result:**

```
Some Text that contains or other HTML
```

Anywhere HTML sequences appear inside the context, they will be removed. You may put any \[xxx] tags inside the context--for instance, you may use fields from a database, or even \[include] the entire contents of another file. HTML is recognized as any text which begins with "<" followed by any number of non-space characters, followed by any number of characters, followed by ">".
