---
description: Show & hide fundamentals of dynamic page creation
---

# Show & Hide

### \[showif] [context](https://docs.webdna.us/contexts-and-tags)

\[showif] is one of the fundamental tools in WebDNA, it is used to show anything required if the comparison is met. Having a good understanding of creating these comparisons will give the WebDNA developer a strong ability to create efficient, meaningful code.

**Example WebDNA code:**

```
[showif comparison]Show this content or further WebDNA code[/showif]
```

\


**WebDNA Comparisons:**

| Comparison   | Modulo Operator | Example                                                                                                                                                                                                                                                 |
| ------------ | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| equal        | =               | \[showif \[username]=SAGEHEN]Welcome Mr. Sagehen\[/showif]                                                                                                                                                                                              |
| not equal    | !               | \[showif \[random]!45]......\[/showif]                                                                                                                                                                                                                  |
| contains     | ^               | \[showif \[browsername]^Mozilla]......\[/showif]                                                                                                                                                                                                        |
| begins with  | \~              | <p>[showif [ipaddress]~245.078.013]......[/showif].<br><strong>Notice the IP address has been defined with 3 digits in each portion of the address</strong>.<br>This is very important for making comparisons with [ipaddress] to work as expected.</p> |
| less than    |  <              | \[showif \[random]<50]...\[/showif]                                                                                                                                                                                                                     |
| greater than |  >              | \[showif \[lastrandom]>25]...\[/showif]                                                                                                                                                                                                                 |
| divisible by | \\              | \[ShowIf \[index]\3]......\[/ShowIf]                                                                                                                                                                                                                    |

Comparisons are always case-insensitive so "oranges" equals "ORANGES".

If both side of the equation are numbers, then the comparison for greater than, less than, and equals is performed numerically. If either side is not a number, then the comparison is performed alphabetically.

**Using WebDNA comparisons:**\
Ensure neither side of the comparison equation contains any of the special comparison characters listed in the Comparisons Table above.

```
[showif [question]=I'm Friendly! Are you?]...[/showif]
```

This example contains an exclamation point inside the sentence being compared, WebDNA sees this as "I'm Friendly" is not equal to " Are you?=\[question]", which is not what the developer meant to compare.\
The solution is to wrap each side of the comparison in a \[url] context:

```
[showif [url][question][/url]=[url]I'm Friendly! Are you?[/url]]......[/showif]
```

This causes any embedded ! = > < symbols to be converted to their URL equivalents, which can then be correctly compared.

### \[hideif] [context](https://docs.webdna.us/contexts-and-tags)

\[hideif] along with its counterpart \[showif] is one of the fundamental tools in WebDNA, it is used to show anything required if the comparison is met. Having a good understanding of creating these comparisons will give the WebDNA developer a strong ability to create efficient, meaningful code.

**Example WebDNA code:**

```
[hideif comparison]Hide this content or further WebDNA code[/hideif]
```

\[hideif] works in exactly the same way as \[showif] but in reverse.

**WebDNA Comparisons:**

| Comparison   | Modulo Operator | Example                                                                                                                                                                                                                                                 |
| ------------ | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| equal        | =               | \[hideif \[username]=SAGEHEN]You are not Mr. Sagehen\[/hideif]                                                                                                                                                                                          |
| not equal    | !               | \[hideif \[random]!45]......\[/hideif]                                                                                                                                                                                                                  |
| contains     | ^               | \[hideif \[browsername]^Mozilla]......\[/hideif]                                                                                                                                                                                                        |
| begins with  | \~              | <p>[hideif [ipaddress]~245.078.013]......[/hideif].<br><strong>Notice the IP address has been defined with 3 digits in each portion of the address</strong>.<br>This is very important for making comparisons with [ipaddress] to work as expected.</p> |
| less than    |  <              | \[hideif \[random]<50]...\[/hideif]                                                                                                                                                                                                                     |
| greater than |  >              | \[hideif \[lastrandom]>25]...\[/hideif]                                                                                                                                                                                                                 |
| divisible by | \\              | \[hideif \[index]\3]......\[/hideif]                                                                                                                                                                                                                    |

### \[if] WebDNA v4.0+ [context](https://docs.webdna.us/contexts-and-tags)

The \[if] context displays HTML or executes WebDNA conditionally if the comparisons are true and also an option to display a different output _if_ false . \[if] can have multiple comparisons, singularly or as groups.

**Example WebDNA code:**

```
[if [qty]=1]
  [then]only one required[/then]
  [else]more than one required[/else]
[/if]
```

\


**WebDNA \[if] Comparisons:**

| Comparison   | Modulo Operator | Example                                                                                                                                                                                                                                                               |
| ------------ | --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| equal        | =               | \[if "\[username]" = "SAGEHEN"] variable \[username] is equal to SAGEHEN                                                                                                                                                                                              |
| not equal    | !               | \[if \[random] ! 45] random number is not 45                                                                                                                                                                                                                          |
| contains     | ^               | \[if "\[browsername]" ^ "Mozilla"] variable \[browsername] contains the text Mozilla                                                                                                                                                                                  |
| begins with  | \~              | <p>[if "[ipaddress]" ~ "245.078.013"] variable [ipaddress] begins with 245.078.013</p><p><strong>Notice the IP address has been typed with 3 digits in each portion of the address</strong>, this is very important for making these comparison work as expected.</p> |
| less than    | <               | \[if \[random] < 50] random number is less than 50                                                                                                                                                                                                                    |
| greater than | >               | \[if \[lastrandom] > 25] last random number is greater than 25                                                                                                                                                                                                        |
| divisible by | \\              | \[if \[index] \ 3] variable \[index] is divisble by 3                                                                                                                                                                                                                 |
| or           | \|              | \[if (5>4) \| (1<3)] Boolean comparison: if either side of the operator is true, then the comparison is true                                                                                                                                                          |
| and          | &               | \[if (5>4) & (1<3)] Boolean comparison: if both sides of theoperator are true, then the comparison is true                                                                                                                                                            |

**WebDNA \[if] Delimiters:**

| Delimiter   | Operator          | Example                                                                                                                                                                                                            |
| ----------- | ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Quoted Text | "......"          | \[if "Hello" ^ "hell"] All text must be surrounded by quotes                                                                                                                                                       |
| Numbers     |                   | \[if 12.5 < 13.2] Numbers do not need to be delimited; they function the same as in a [\[math\]](https://docs.webdna.us/MathContext.html) context                                                                  |
| Dates       | \[math]{}\[/math] | \[if \[math]{\[date]}\[/math] > \[math]{9/7/1963}\[/math]] Dates must be enclosed in curly braces to distinguish them from regular numbers and have the \[math] operator applied to convert the date to a number   |
| Times       | \[math]{}\[/math] | \[if \[math]{\[time]}\[/math] > \[math]{12:31:00PM}\[/math]] Times must be enclosed in curly braces to distinguish them from regular numbers and have the \[math] operator applied to convert the date to a number |
| Parentheses | (...)             | \[if (3>1) & ("a"<"b")] You may collect groups of items in parentheses in order to force the order of evaluation                                                                                                   |

A common mistake is to omit the "" around text variables

\[if _comparison_]\[then]do this\[/then]\[else]otherwise this\[/else]\[/if]

**Example WebDNA code:**

```
[if (("[username]"="Grant") | ([grandTotal]<100)) & ([math]{[date]}[/math]<[math]{2/15/2030}[/math])]
    [then]either username was Grant or grandTotal was < $100 and it's not Feb 15, 2030 yet[/then]
    [else]The complex expression wasn't true[/else]
[/if]
```

**Example WebDNA code:**

```
[if (("apples"="red")&("bananas"="yellow"))|(200>100)]
[then]This is correct[/then]
[else]This is wrong[/else]
[/if]
```

Comparisons are always case-insensitive so "grant" equals "GRANT". The expression is evaluated as a mathematical boolean equation, where each sub-expression evaluates to either 0 or 1 (meaning true or false). If the entire evaluated expression is true, then the WebDNA inside the \[then] context is executed, otherwise the \[else] context is executed.

The \[math] context has been extended to allow for quoted text and boolean operators, and is actually what is used by \[if] to perform the work of evaluating the expression. A side-effect of this allows you to use these operators inside a \[math] equation: \[math]1<3\[/math] evaluates to "1", because the equation is true. Conversely, \[math]3<1\[/math] evaluates to "0" because the equation is false. Similarly, \[math]1&1\[/math] evaluates to "1", and \[math]1&0\[/math] evaluates to "0".

| Comparison   |                   | Example                                                                                                                                                                                                                                                             |
| ------------ | ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| equal        | =                 | \[if "\[username]" = "SAGEHEN"] variable \[username] is equal to SAGEHEN                                                                                                                                                                                            |
| not equal    | !                 | \[if \[random] ! 45] random number is not 45                                                                                                                                                                                                                        |
| contains     | ^                 | \[if "\[browsername]" ^ "Mozilla"] variable \[browsername] contains the text Mozilla                                                                                                                                                                                |
| begins with  | \~                | <p>[if "[ipaddress]" ~ "245.078.013"] variable [ipaddress] begins with 245.078.013</p><p><strong>Notice the IP address has been typed with 3 digits in each portion ofthe address</strong>. this is very important for making these comparison workas expected.</p> |
| less than    | <                 | \[if \[random] < 50] random number is less than 50                                                                                                                                                                                                                  |
| greater than | >                 | \[if \[lastrandom] > 25] last random number is greater than 25                                                                                                                                                                                                      |
| divisible by | \\                | \[if \[index] \ 3] variable \[index] is divisble by 3                                                                                                                                                                                                               |
| or           | \|                | \[if (5>4) \| (1<3)] Boolean comparison: if either side of the operator is true, then the comparison is true                                                                                                                                                        |
| and          | &                 | \[if (5>4) & (1<3)] Boolean comparison: if both sides of theoperator are true, then the comparison is true                                                                                                                                                          |
| Delimiter    |                   | Example                                                                                                                                                                                                                                                             |
| Quoted Text  | "..."             | \[if "Hello" ^ "hell"] All text must be surrounded by quotes                                                                                                                                                                                                        |
| Numbers      |                   | \[if 12.5 < 13.2] Numbers do not need to be delimited; they function the same as in a [\[Math\]](https://docs.webdna.us/MathContext.html) context                                                                                                                   |
| Dates        | \[math]{}\[/math] | \[if \[math]{\[date]}\[/math] > \[math]{9/7/1963}\[/math]] Dates must be enclosed in curly braces to distinguish them from regular numbers and have the \[math] operator applied to convert the date to a number.                                                   |
| Times        | \[math]{}\[/math] | \[if \[math]{\[time]}\[/math] > \[math]{12:31:00PM}\[/math] Times must be enclosed in curly braces to distinguish them from regular numbers and have the \[math] operator applied to convert the date to a number                                                   |
| Parentheses  | (...)             | \[if (3>1) & ("a"<"b")] You may collect groupsof items in parentheses in order to force the order of evaluation                                                                                                                                                     |

An example to detect mobilephones:

**Example WebDNA code:**

```
[if ("[browsername]"="Mozilla/5.0 (iPhone; U; CPU iPhone OS 2_2_1 like Mac OS X; en-us) AppleWebKit/525.18.1 (KHTML, like Gecko) Version/3.1.1 Mobile/5H11 Safari/525.20") | ("[browsername]"="BlackBerry8330/4.3.0 Profile/MIDP-2.0 Configuration/CLDC-1.1 VendorID/126 UP.Browser/5.0.3.3")]
[then][redirect url=mobile/index.html][/then]
[else][/else]
[/if]
```

An example with some handy techniques to use for dealing with boolean values, by Brian Fries:

I define two global text variables in my header include files:\
**Example WebDNA code:**

```
[text]true=1=1[/text]
[text]false=1=0[/text]
```

Then I use these to set my "boolean" variables within my code:\
**Example WebDNA code:**

```
[text]needToDoThis=[true][/text]
[text]alreadyDidThis=[true][/text]
```

Then in my _if, showif and hideif_ statements, I can use the following:\
**Example WebDNA code:**

```
[if [needToDoThis]][then]
do this
[/then][else]
don't do this
[/else][/if][showif [needToDoThis]]
do this
[text]alreadyDidThis=[true][/text]
[/showif][hideif [alreadyDidThis]]
gotta do this
[/hideif]
```

When using some comparisons such as ">" or "<", make sure\
you have a valid string or the statement will always issue the exception (ie \[else]\[/else]).\
In the example below, because the first comparison now has something to compare, the statement is not "nulled" out and the second comparison is allowed to make the whole statement true.

**Example WebDNA code:**

```
This won't work:When "balance" = nothing
[text]balance=[/text][if ([balance]>0) | ("[balance]" = "")]
  [then]
    should do this
  [/then]
[/if]This will work:
When "balance" = nothing
[text]balance=[/text][if (0[balance]>0) | ("[balance]" = "")]
  [then]
    should do this
  [/then]
[/if]
```

**Example WebDNA code:**

```
This won't work:
[if [number]=11|16]
[then]do this[/then]
[else]do that[/else]
[/if]this will work
[if ([number]=11)|([number]=16)]
[then]do this[/then]
[else]do that[/else]
[/if]
```

### \[then] WebDNA v4.0+ [context](https://docs.webdna.us/contexts-and-tags)

Must be used inside \[if].....\[/if]

### \[else] WebDNA v4.0+ [context](https://docs.webdna.us/contexts-and-tags)

Must be used inside \[if].....\[/if]

### \[switch] WebDNA v4.0+ [context](https://docs.webdna.us/contexts-and-tags)

Executes the WebDNA inside the only \[case] context which matches the given value.

```
[switch Value]Series of [case]...[/case] contexts[/switch]
```

To display some HTML (or execute some WebDNA) from a list of known text options, put the text value inside a \[switch] context. For each possible option, put a \[case] context inside the \[switch]. You may optionally specify a default case by inserting a \[default] context. The \[default] context must be the very last context inside the \[switch].

**Example WebDNA code:**

```
[text]x=5[/text]
[switch value=[x]]
  [case value=1]
    The value of x was 1
  [/case]
  [case value=2]
    The value of x was 2
  [/case]
  [default]
    The value of x was neither 1 nor 2; it was [x]
  [/default]
[/switch][text]title=Mrs[/text]
[switch value=[title]]
  [case value=Mr]
    You're a male
  [/case]
  [case value=Mrs]
    You're a female
  [/case]
[/switch]
```

In the first example above, the text "The value of x was neither 1 nor 2; it was 5" will display, because the two cases for "1" and "2" did not match the actual value of x, which was "5." In the second example above, the text "You're a female" will display. Any WebDNA inside the other \[case] contexts will not execut, and any text inside those contexts will not display.

The values are compared as case-insensitive text only. This means the number "1.0" is not the same as the number "1" when determining which of the \[case] contexts to execute.

From WebDNA version 8.1, \[switch] accepts the whole set of comparisons:

| Comparison   |       | Example                                                                                                                                                                                                                                                             |
| ------------ | ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| equal        | =     | \[If "\[username]" = "SAGEHEN"] variable \[username] is equal to SAGEHEN                                                                                                                                                                                            |
| not equal    | !     | \[If \[random] ! 45] random number is not 45                                                                                                                                                                                                                        |
| contains     | ^     | \[If "\[browsername]" ^ "Mozilla"] variable \[browsername] contains the text Mozilla                                                                                                                                                                                |
| begins with  | \~    | <p>[If "[ipaddress]" ~ "245.078.013"] variable [ipaddress] begins with 245.078.013</p><p><strong>Notice the IP address has been typed with 3 digits in each portion ofthe address</strong>. this is very important for making these comparison workas expected.</p> |
| less than    | <     | \[If \[random] < 50] random number is less than 50                                                                                                                                                                                                                  |
| greater than | >     | \[If \[lastrandom] > 25] last random number is greater than 25                                                                                                                                                                                                      |
| divisible by | \\    | \[If \[index] \ 3] variable \[index] is divisble by 3                                                                                                                                                                                                               |
| or           | \|    | \[If (5>4) \| (1<3)] Boolean comparison: if either side of the operatoris true, then the comparison is true                                                                                                                                                         |
| and          | &     | \[If (5>4) & (1<3)] Boolean comparison: if both sides of theoperator are true, then the comparison is true                                                                                                                                                          |
| Delimiter    |       | Example                                                                                                                                                                                                                                                             |
| Quoted Text  | "..." | \[If "Hello" ^ "hell"] All text must be surrounded by quotes                                                                                                                                                                                                        |
| Numbers      |       | \[If 12.5 < 13.2] Numbers do not need to be delimited; they function the same as in a [\[Math\]](https://docs.webdna.us/MathContext.html) context                                                                                                                   |
| Dates        | {}    | \[If {\[date]} > {9/7/1963}] Dates must be enclosed in curly braces to distinguish them from regular numbers                                                                                                                                                        |
| Times        | {}    | \[If {\[time]} > {12:31:00PM} Times must be enclosed in curly braces to distinguish them from regular numbers                                                                                                                                                       |
| Parentheses  | (...) | \[If (3>1) & ("a"<"b")] You may collect groupsof items in parentheses in order to force the order of evaluation                                                                                                                                                     |

To maintain compatibility with previous versions, if the \[case] statement sets the parameter "value" as in this example:\
\[case value=13]\
Then it assumes they are the old-style switch statements.

However, if it doesn't see "value=", it assumes there is an equation to evaluate, just like with \[if]. You can mix/match both types in the same switch block.

**Example WebDNA code:**

```
[text]x=3[/text]
[switch value=[x]]
 [case [x]=1]
   The value of x was 1
 [/case]
 [case [x]>2]
   The value of x was > 2
 [/case]
 [case value=3]
   The value of x was 3
 [/case]
 [default]
   The value of x was neither 1 nor 3 nor greater than 2; it was [x]
 [/default]
[/switch]
```

This code outputs:\
The value of x was > 2\
The value of x was 3

If you set value= in the \[switch] statement, then that is used only for the \[case] statements where you also set value=. The value= you set in the \[switch] statement is completely ignored for any \[case] statements where you don't set value=.

So as an example, this does NOT work at all:\
\[switch value=\[x]]\
\[case value>23]\
Because you are literally comparing the text "value" to the number 23.

Also, this does NOT work either:\
\[switch value=\[x]]\
\[case \[value]>23]\
value is a parameter, it is not defined as a variable. So \[value] doesn't evaluate as anything in the equation.

However, this DOES work:\
\[text]value=24\[/text]\
\[switch value=\[x]]\
\[case \[value]>23]\
This works because value was defined as a variable outside of the switch.

This works too:\
\[text]value=24\[/text]\
\[switch]\
\[case \[value]>23]\
If you don't use value= statements in the \[case] statements, you don't have to set it in the \[switch] statement.

**Example WebDNA code:**

```
[text]x=3[/text]
[text]y=1[/text][switch value=[x]] >>>>>>> value=[x] to maintains compatibility
 [case [x]=1]
   The value of x was 1
 [/case]
 [case [y]>2]
   The value of y was > 2
 [/case]
 [case value=4] >>>>>>> this is to maintain compatibility
   The value of x was 4
 [/case]
 [default]
   The value of x was not 1 or 4 and the value of y was not greater than 2
 [/default]
[/switch]
```

works perfectly fine. The old compatible style statements still work and the new ones do too. For example, x=4 and y=3 results in: The value of y was > 2 The value of x was 4

### \[case] WebDNA v4.0+ [context](https://docs.webdna.us/contexts-and-tags)

### \[default] WebDNA v4.0+ [context](https://docs.webdna.us/contexts-and-tags)

### \[hide] WebDNA v8.5+ [context](https://docs.webdna.us/contexts-and-tags)
