# Date & Time

### \[date] [tag](https://docs.webdna.us/contexts-and-tags)

* The default format of \[date] is MM/DD/YYYY, date format can be changed in WebDNA Preference Settings.
* Putting \[date] in a template displays the current date as defined by the **clock of the web server**
* Override the default \[date] format preference on a case-by-case basis by specifying a format inside the tag, as shown below.

**Parameters**

| Parameter | Description                     |
| --------- | ------------------------------- |
| format    | The required format of the date |

**Date Formats - case sensitive:**

| Format | Description                              |
| ------ | ---------------------------------------- |
| %a     | Abbreviated day name "Wed"               |
| %A     | Full day name "Wednesday"                |
| %b     | Abbreviated month name "Feb"             |
| %B     | Full month name "February"               |
| %c     | Date and time "Wed Sep 19 18:24:21 2010" |
| %d     | Day of month "01-31"                     |
| %j     | Day of year "001-365"                    |
| %m     | Month "01-12"                            |
| %U     | Week # of year, Sunday first day of week |
| %w     | Weekday "0" (Sunday) - "6" (Saturday)    |
| %W     | Week # of year, Monday first day of week |
| %x     | Date as "Jan 25 2022"                    |
| %y     | Year without century "00-99"             |
| %Y     | Year with century "1900-2199"            |

**Example WebDNA code:**

```
Formats in the [date] tag can be combined with ordinary characters, 
such as the comma, slash and space
[date] returns: 08/14/2022 
[date %A, %B %d, %Y] returns: Thursday, August 14, 2022
[date %m/%d/%Y] returns: 08/14/2022
[date %d/%m/%Y] returns: 14/08/2022
[date format=%A, %B %d, %Y] returns: Thursday, August 14, 2022
[date %Z] returns: UTC
[date %s] returns: 1670966773
```

The \[date] tag is sometimes confused with \[orderfile]'s \[date] tag, these are different. The \[date] tag inside the context of an \[orderfile] represents the date the order was created, and does not have the ability to be custom formatted. If today's date is required inside an \[orderfile], then a text variable must first be assigned outside the \[orderfile] context, and use that text variable.

**Example WebDNA code:**

```
[text]todaysdate=[date][/text]
[orderfile cart=[cart]]
  Order Date: [date]
  Today: [todaysdate]
[/orderfile]
```

Ambiguous dates/times\
If a date contain a decimal point "." then the \[math] context will interpret them as a time instead of a date. It is possible to force WebDNA to interpret text as a date by inserting a "D" in front of the text, example: \[math]{D10.01.2022}\[/math]. Alternately it is possible to force time interpretation by using "T".

**Mix time & date WebDNA code:**

```
[time] and [date] tags can be mixed by using the "%" formatting switches
[time %A, %I:%M %p] returns: Friday, 2:30 PM
[date %A, %I:%M %p] returns: Friday, 2:30 PM
```

### \[time] [tag](https://docs.webdna.us/contexts-and-tags)

* The default format of \[time] is HH:MM:SS (24-hour clock), time format can be changed in WebDNA Preference Settings
* Putting \[time] in a template displays the current time as defined by the clock of the web server
* Override the default \[time] format preference on a case-by-case basis by specifying a format inside the tag, as shown below

**Parameters**

| Parameter | Description                     |
| --------- | ------------------------------- |
| format    | The required format of the date |

**Time Formats - case sensitive:**

| Format | Description                                                                              |
| ------ | ---------------------------------------------------------------------------------------- |
| %H     | Hour "00-24"                                                                             |
| %-H    | Hour "1-12" (no leading 0 when adding - before the capital h)                            |
| %#H    | Hour "1-12" (no leading 0 when adding # before the capital h)                            |
| %I     | Hour "01-12" (capital i)                                                                 |
| %l     | Hour "1-12" (no leading 0 when using a lowercase L)                                      |
| %M     | Minute "00-59"                                                                           |
| %S     | Seconds "00-59"                                                                          |
| %p     | Am or PM                                                                                 |
| %X     | Time as "14:01:12"                                                                       |
| %c     | Date and time "Wed Jan 25 18:24:21 2022"                                                 |
| %s     | Unix time stamp "1630208860", the number of seconds since January 1st, 1970 00:00:00 UTC |
| %z     | Offset of server ie the difference in hours from GMT/UTC eg: +1100                       |
| %Z     | Time zone of the server eg: AEDT, this reads as _Australian Eastern Dalylight Time_      |

**Example WebDNA code:**

```
Formats in the [time] tag can be combined with ordinary characters, 
such as the comma, slash and space
[time] returns: 14:32:37
[time %I:%M %p] returns: 02:30 PM
[time %l:%M %p] returns: 2:30 PM
[time %H:%M %p] returns: 14:30 PM
[time %H:%M:%S] returns: 14:30:45
[time format=%H:%M:%S] returns: 14:30:45The leading zero can be eliminated on Linux & Apple servers using lowercase L 
[time %l:%M %p] returns: 2:08 PMor a hyphen "-" after the "%"
[time %-I:%M %p] returns: 2:08 PM
[time %-I:%-M %p] returns: 2:8 PM
```

**Mix time & date WebDNA code:**

```
[time] and [date] tags can be mixed by using the "%" formatting switches
[time %A, %I:%M %p] returns: Friday, 2:30 PM
[date %A, %I:%M %p] returns: Friday, 2:30 PM
```

Calculations on time are made simple using WebDNA. To calculate the time difference between 16:04:23 and 09:34:46 on the same day, use the following example. The result will be the number of seconds that have elapsed, there are 86,400 seconds in a day. Using \[format] will convert seconds into hours, minutes and seconds. This can be done using the expanded method of formatting the result, or the newer 'shorthand method of incorporating the formatting into the \[math] tag.

**Example WebDNA code:**

```
[math]{16:04:23}-{09:34:46}[/math]
[format seconds_to_time][math]{16:04:23}-{09:34:46}[/math][/format]
[math seconds_to_time]{16:04:23}-{09:34:46}[/math]
```

Converting a time calculation back to hours, minutes and seconds. Combining the 'time formats' in the table above with 'seconds\_to\_time' provides the ability to format the result in a desired form, in this case only hours and minutes are displayed.

**Example WebDNA code:**

```
[math seconds_to_time %H:%M]{16:04:23}-{09:34:46}[/math]
```

**Removing leading zeros.**\
On Linux and Apple platform servers, the leading zero can be removed using a hyphen "-" after "%"

**Example WebDNA code:**

```
[math seconds_to_time %I:%M:%S %p]{09:04:06}[/math]  << returns 09:04:06 AM
[math seconds_to_time %-I:%M:%S %p]{09:04:06}[/math]  << returns 9:04:06 AM
[math seconds_to_time %-I:%M:%S %p]{12:45:11}[/math]  << returns 12:45:11 PM
[math seconds_to_time %-I:%-M:%-S %p]{09:04:06}[/math]  << returns 9:4:6 AM
```
