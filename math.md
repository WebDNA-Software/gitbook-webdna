# Math

### \[format] [context](https://docs.webdna.us/contexts-and-tags)

To apply formats for Dates or Times **other than the current date and time**, use \[format]

Format is used for text and numbers. Format will also apply formatting to dates and times _provided they are converted to numbers first_. To convert a date or time into a number, enclose them with curly braces, and wrap them in a \[math] context. This will convert dates into number of days since 00/00/0000, and times into number of seconds since midnight. And then they can be wrapped in a format context for custom formatting.

To convert to a number:

```
[math]{08/14/2008}[/math] --> 733633
[math]{10:12:07}[/math] --> 36727
```

Now we can apply formatting using days\_to\_date, and formatting specifiers:

```
1. [format days_to_date %m/%d/%y][math]{08/14/2008}[/math][/format]
2. [format days_to_date %A, %B %d, %Y][math]{08/14/2008}[/math][/format]
3. [format days_to_date %A, %B %d, %Y]733633[/format]
4. [format days_to_date]733633[/format]
5. [format seconds_to_time %I:%M:%S %p][math]{10:12:07}[/math][/format]
6. [format seconds_to_time %I:%M:%S %p]36727[/format]
```

Results would be:

1\. 08/14/08\
2\. Thursday, August 14, 2008\
3\. Thursday, August 14, 2008\
4\. 08/14/2008\
5\. 10:12:07 AM\
6\. 10:12:07 AM

Note that the \[date] formats are the same, but work specifically on the \[date] tag, which resolves to the current date only. If you are retrieving dates from a database, use Format as above.

**TIP**\
The %d designator returns a 2 digit date (August 05, 2008), but for single digit dates, this looks bad. There's a workaround. Separate the components, and wrap the %d piece in \[math] tags, and it will get rid of the leading zero.

```
[math][format days_to_date %d][math]{[date]}[/math][/format][/math]
```

Another way would be using grep:

```
[grep search= 0&replace= ]
[format days_to_date %A, %B %d, %Y][math]{[date]}[/math][/format]
[/grep]
```

**\[format FormatSpec]Text or Number\[/format]**\
Formats text or numbers in various widths or currency formats.\
To display numbers with various decimal points or currency formats, surround the number or text with a \[format] context.

Numeric formatting works with comma separators for decimal point (non-US style, such as \[format 6,2f]\[math]39/7\[/math]\[/format] yields 7,46 instead of 7.46)

**Example WebDNA code:**:

```
[format 10.2f]99.5[/format] (f stands for floating-point number)
[format 10s]Hello[/format] (s stands for string of text)
[format days_to_date %m/%d/%y]195462[/format]
[format seconds_to_time]49768[/format]
[format seconds_to_time %I:%M:%S %p]49768[/format]
[format thousands 14.2f]394363210[/format]
[format thousands 14,2f]394363210[/format]
[format thousands .3d]7[/format] (d stands for decimal number)
```

The number displays right-justified with enough preceding spaces and digits after the decimal point to fill the exact width of the format specifier. Text is left-justified, with enough spaces after it to exactly fill the width specifier.

```
|     99.50| (10 wide, 2 after the decimal)
|Hello     | (10 wide, text)
|04/07/1997| (#days as a date)
|13:49:28|
|01:49:28 PM| 
|394,363,201.00| (14 wide, number with thousands separator)
|394.363.201,00| (14 wide, number with European thousands separator)
|007| (3 wide, integer part of number only, zeroes preceding)
```

Given a number 345.67, the following format specifiers will display as shown:

```
8.3f = | 345.670| (f stands for floating point)
8.2f = |  345.67|
8.1f = |   345.7| (notice rounding from .67 to .7)
8.0f = |     346| (notice rounding from .67 to next higher integer)
 .5d = |00345| (notice no rounding, and preceding 0s to fill 5 digits)
```

Optional date format -- to format a number as a date (the number must represent the number of days since Midnight, January 1st, 0000), use the optional date specifier and a date format, the same as from the \[date] tag. Also see date and time \[Math].

It is always advised to \[format] your math operations as WebDNA is working on a deep fractional part of the number, rounded by the server processor, which can sometimes yield strange results: instead of getting a "0", you might obtain a 0.0000000000000001

To specify the precision of a number, we must use like \[format .2f]\[math]25\*34.567\[/math]\[/format]. From version 8.6, you can use \[math .2f]25\*34.567\[/math]

\


### \[math] [context](https://docs.webdna.us/contexts-and-tags)

\[math] calculates equations using numbers, dates or time. It can also set variables to be used throughout the page.

**math variable names are limited to 15 characters.**

\


**New v8.6+**\
Previously, to specify the precision of a number, the result needed to be formatted using the \[format] context:\
\[format .2f]\[math]25\*34.567\[/math]\[/format]

Now it is possible to do the same in the \[math] context:\
\[math .2f]25\*34.567\[/math]

This will result in a number with 2 decimal places ie: 864.17

\


\[math] calculates the enclosed numerical, date, or time equation and displays the results.

Any \[xxx] variables are evaluated first, then the resulting equation is calculated.

Standard algebraic order of operations are followed when evaluating expressions. Use parentheses to clarify or force a specific order of operations.

To work with dates and times, put them inside curly-braces: {12/01/2025}. This converts them into numbers so they can be treated by WebDNA as numbers.

Setting math variables\
"Named" \[math] variables are limited to a maximum of 150 per template.\
These "named" variables can be used within any other \[math] context, or elsewhere in the template in the same manner as a text variable.

The name of a math variable is limited to 15 alphanumeric characters, and MUST begin with a letter.\
When setting a math variable, the value will display, unlike a text variable. To prevent this, add show=f to the \[math] context, \[math show=f].

Assign multiple math variables at the same time using a semicolon to separate the assignments.

```
[math show=f]var1=1;var2=2[/math]
```

**Example WebDNA code:**

An invoice template showing the line-item cost of each item:

```
[orderfile cart=[cart]]
  [lineitems]
    [quantity], [ref], [price], [math][price]*[quantity][/math]
  [/lineitems]
[/orderfile]
```

Using a math variable to create a counter

```
[math show=f]counter=0[/math]
[search paramaters]
  [founditems]
    [hideif [firstname]=jack]
      [math]counter=counter 1[/math]. [firstname] [lastname]<br>
    [/hideif]
  [/founditems]
[/search]
There were [counter] names found.
```

In this example, the first line sets the initial value of \[counter] to 0.\
Some of the founditems are eliminated via the \[hideif]. In this case, \[index] and \[numfound] normally used in \[search] would still include the eliminated items, and would not result in the correct final count.

Dates and times can not be mixed in one equation.\
Dates included in mathematical expressions must be enclosed in curly braces converting them to a number (the number of days since 01/01/0000) so math can be performed on the converted date.\
Easily add or subtract days, months, or years from dates by expressing them in date format within curly braces. Just use 0 for values you want ignored. For instance, in order to add 2 months to today's date:\


```
[math date]{[date]} {2/0/0000}[/math]
```

The result is displayed back into date format by adding the _date_ modifier to the context. **Note** The year must be expressed as 4 digits.

Decimals in date notation: Some countries specify dates with decimal points, as in {10.1.2008}, however WebDNA will interpret this as a time. Force WebDNA to interpret text as a Date by inserting a "D" in front of the text, as in \[math]{D10.01.1998}\[/math].

Use \[math] and \[date] to create a select list populated with the current year plus another 10 years using the \[math] context.

**Example WebDNA code:**

```
<select name="StartYear">
  [loop start=[date %Y]&end=[math][date %Y] + 10[/math]]
    <option[showif [date %Y]=[index]] selected[/showif]>[index]</option>
  [/loop]
</select>
```

To make a calculation without displaying the results, perhaps while calculating a running total use "show=F" into the math parameters, as in \[math show=F]total=total+\[subTotal]\[/math]. This allows a calculation in the middle of a web page without the intermediate numbers appearing to the visitor. At the end, show the value of the math variable with \[math]total\[/math] or \[math show=t]total\[/math].

**Example WebDNA \[math] code:**

<pre><code><strong>[math](4.5 6.2)/17*95-12[/math] 47.7941176470588
</strong></code></pre>

\


<pre><code><strong>[math]{4/7/1997} + 10[/math] 729496 (4/7/1997 + 10 days expressed as number of days since 00/00/0000)
</strong></code></pre>

\


<pre><code><strong>[math]{4/7/1997} + {02/00/0000}[/math] 729547 (4/7/1997 + 2 months expressed as days since 00/00/0000)
</strong></code></pre>

\


<pre><code><strong>[math date]{4/7/1997} + 10[/math] 04/17/1997 (4/07/1997 + 10 days expressed as date)
</strong></code></pre>

\


<pre><code><strong>[math date]{4/7/1997} + {02/00/0000}[/math] 06/07/1997 (4/17/1997 + 2 months expressed as date)
</strong></code></pre>

\


<pre><code><strong>[math date]{[date]}-{00/07/0000}[/math] 09/01/2008 (One week ago today)
</strong></code></pre>

\


<pre><code><strong>[math]{12:51:02}[/math] 46262 (number of seconds between midnight and 12:51:02 expressed as seconds)
</strong></code></pre>

\


<pre><code><strong>[math time]{12:51:02} + {01:00:05}[/math] 13:51:07 (12:51:02 pm plus 1 hour and 5 seconds expressed as time)
</strong></code></pre>

\


<pre><code><strong>[math]x=5/3[/math] 1.66666666666667
</strong></code></pre>

\


<pre><code><strong>[math]x=5%3[/Math](% = Modulo Operator) 2
</strong></code></pre>

**Example WebDNA Scientific Function code:**

```
[math]ceil(1.5)[/math]
[math]sin([formvalue])*cos(3.1415)[/math]
```

\


| Function | Description                                       |
| -------- | ------------------------------------------------- |
| sin(x)   | Returns sine of x                                 |
| cos(x)   | Returns cosine of x                               |
| tan(x)   | Returns tangent of x                              |
| asin(x)  | Returns arcsine of x                              |
| acos(x)  | Returns arccosine of x                            |
| atan(x)  | Returns arctangent of x                           |
| sinh(x)  | Returns hyperbolic sine of x                      |
| cosh(x)  | Returns hyperbolic cosine of x                    |
| tanh(x)  | Returns hyperbolic tangent of x                   |
| log(x)   | return natural log of x                           |
| log10(x) | Returns log base 10 of x                          |
| sqrt(x)  | Returns square root of x: sqrt(16) = 4            |
| floor(x) | rounds down to next-lower integer: floor(2.9) = 2 |
| ceil(x)  | rounds up to next-higher integer: ceil(3.1) = 4   |
| abs(x)   | Returns absolute value of x: abs(-3.4) = 3.4      |
| deg(x)   | converts radians to degrees                       |
| rad(x)   | converts degrees to radians                       |

**Historical behavior:**\
Originally, a math variable had to be retrieved by using the following format: \[math]variablename\[/math], as opposed to \[variablename]. It will still work this way. In fact, that is why seemingly incorrect code like \[math]counter=counter 1\[/math] works.

**Time:**\
The principles are the same for time, except the number represents seconds since midnight. Curly braces convert times into seconds since midnight, and using \[math time] will display the result in time format.\
Time may be included in mathematical expressions by enclosing the time in braces ie: {10:24:33} . It is easy to add or subtract hours, minutes, or seconds from times by expressing them as a complete time. Use 0 for values that you want ignored. That is, in order to add 2 minutes to the current time, write an expression such as

```
[math time]{[time]} + {00:02:00}[/math]
```

It is a good idea to group math expressions involving time together by using parentheses.

When using time mixed with integers, the final result is a value respresenting a number of seconds

```
[math]{10:15:31} + 10[/math] adds 10 seconds to the time
```

In fact, the result of a math expression with time is always the number of seconds. To display the output of the math expression as a time, add the Time modifier to the \[math] context: \[math time]...\[/math].

Use \[format] to convert an integer number to a date or time. Use \[format days\_to\_date] and \[format seconds\_to\_time] to convert integer numbers to their equivalent dates/times. The integer number represents the number of days since January 1, 0000 and for time, the number of seconds since midnight.

**Example WebDNA code:**

```
[format days_to_date]729496[/format] yields 4/17/1997
[format seconds_to_time]46262[/format] yields 12:51:02
```

\[math] variable names allow 15 characters. Avoid using a hyphen (-) in a \[math] variable name.\
\[text] variable names have no limit.
