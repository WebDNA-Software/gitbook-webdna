# Searching

### \[search] [context](https://docs.webdna.us/contexts-and-tags)

Use the \[search] context with \[founditems] to easily retrieve records from a database.

The following tags are available inside a \[search] context:

| Tag                                          | Description                                                                                                                                            |
| -------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| \[numfound]                                  | A number indicating how many records matched the search request.                                                                                       |
| \[sum field=_fieldName_]                     | Calculates the numerical sum of all the found records, using the _fieldName_ column.                                                                   |
| \[avg field=_fieldName_]                     | Calculates the numerical average of all the found records, using the _fieldName_ column.                                                               |
| \[min field=_fieldName_]                     | Calculates the numerical minimum of all the found records, using the _fieldName_ column.                                                               |
| \[max field=_fieldName_]                     | Calculates the numerical maximum of all the found records, using the _fieldName_ column.                                                               |
| \[founditems]...\[/founditems]               | Place a \[founditems] loop inside a \[search], to display all the matching records. Any database field names may be used inside the \[founditems] loop |
| \[replacefounditems]...\[/replacefounditems] | Loops though all the found items and replaces text in each record with the specified new data.                                                         |
| \[shownext]...\[/shownext]                   | Is used to build pagination                                                                                                                            |

Fundamental Search Structure

```
[search criteria]
    [founditems]Search results[/founditems]
[/search]
```

It really is that simple. Any search criteria may be specified, and as many \[search] contexts may be included in a template as required, \[search] may be 'nested' them inside of each other to create relational searches among multiple databases.

**Example WebDNA code:**

```
[search db=xx.db&eqCITYdata=Chicago]
  Search returned [numfound] items
  [founditems]
    <img src="[picture]">
    [name], [address], [city]
  [/founditems]
  [shownext]
    Pagination links may be placed here
  [/shownext]
[/search]
```

To display results (using simple fieldname tags), place a \[founditems]...\[/founditems] context inside the \[search] context. \[founditems] will loop through the matching records.

In addition to \[founditems], three other special tags are valid within the search, but NOT WITHIN the \[founditems] tags.

1\) \[numfound] will report the number of records matching the search criteria.

2\) \[shownext] \[/shownext] will manage pagination.

3\) \[replacefounditems] \[/replacefounditems] will replace specified fields with new values as each matching record is found.

SQL/ODBC Note: To search through an ODBC-compliant database, use the \[SQL] context, not \[search].

**Search Critera**\
Search criteria are strung together in the opening \[search] tag, and separated with ampersands (&).

```
[search db=yourdatabase.db&eqCITYdata=Chicago&ZIPCODEsumm=t&ZIPCODEsort=1&max=25&startat=[starthere]]
```

**Database Source** (required)

```
db=yourdatabase.db
  OR
table=tablename
```

The source of your data can be either a database file on the server or a table (temporary 'in line' database table that is local to the template and not part of the global database cache.)

**Data to look for** (required)

```
eqCITYdata=Chicago
```

The data to search for must be indicated by specifying the field to look in, and the data to look for. See "Search Data" for how to refine this search with comparisons, partial strings, multiple fields, ranges, plus other options.

**Sort Order**

```
LASTNAMEsort=1
```

Define the sort order with sort. See sort for how to refine the sort using random, ascending, descending, no sorting, etc. There are two formats for defining sort.

**Summary**

```
ZIPCODEsumm=t
```

Summary will return just one record from each matching set of values. Use Summary to create headings and initiate subsearches. Sorting on the field summarized is also possible.

The source of your data can be either a database file on the server or a table (temporary 'in line' database table that is local to the template and not part of the global database cache.)

**Data to look for** (required)

```
eqCITYdata=Chicago
```

The data to search for must be indicated by specifying the field to look in, and the data to look for. See "Search Data" for how to refine this search with comparisons, partial strings, multiple fields, ranges, plus other options.

**Sort Order**

```
LASTNAMEsort=1
```

Define the sort order with sort. See sort for how to refine the sort using random, ascending, descending, no sorting, etc. There are two formats for defining sort.

**Summary**

```
ZIPCODEsumm=t
```

Summary will return just one record from each matching set of values. Use Summary to create headings and initiate subsearches. Sorting on the field summarized is also possible.

**Data Format**

```
DEADLINEtype=date
CLASSTIMEtype=time
INVENTORYtype=num
```

Unless otherwise specified, data will be treated alphabetically. This will cause 10 to appear before 2, and 11/30/2008 to appear before 02/28/2008. This applies to both sorting and to finding data using ranges, greater than, less than, etc. In the case of numbers of fixed length, such as zip codes and phone numbers, they will sort properly alphabetically, so there's no need to specify that field as a number.

**Maximum to display on each page**

```
max=25
```

Contrary to what this looks like, maximum only specifies how many results to display per page. The search will still find all the records requested, but when used with \[shownext] context to manage pagination.

### Search comparisons <a href="#background14" id="background14"></a>

**Multiple Fields**\
You may search on more than one field.

```
eqStateData=New York&geCountyPopData=20000
```

**Required Match**\
Add **rq** to the end to require the data be found. Otherwise, in the above example you would get other states with counties of over 20,000 population, and all New York counties, not just the big ones. Using **rq** makes this an AND comparison.

<pre><code><strong>eqStateDatarq=New York&#x26;gecountiesDatarq=20
</strong></code></pre>

Sometimes you want to match one field **and** another field at the same time -- this if often called "anding fields together". Examples are "Find all people whose first name is Grant **and** last name is Hulbert" or perhaps "Find all houses that have more than 5 bedrooms **and** a fireplace **and** 2 bathrooms".

By default, if you do not specify certain fields are required to match, they are "or-ed" together, which in the above examples would read like "Find all people whose first name is Grant **or** last name is Hulbert" or perhaps "Find all houses that have more than 5 bedrooms **or** a fireplace **or** 2 bathrooms".

**Important**: WebDNA automatically 'ranks' the best-match results to the top of the list of found items. In the "or-ed" example above, all the houses that have more than 5 bedrooms and a fireplace and 2 bathrooms are shown at the top of the list. Houses that match 2 out of the 3 criteria are listed just below those, and finally houses that only match one of the criteria are listed last. You can override this standard ranking by forcing a different sort order.

typically "and" will find fewer matches in a database, because it is more restrictive -- the chances of finding a house that matches all 3 criteria simultaneously is far lower than finding a house that matches any one of the criterion individually.

To require particular fields to match during a search, put the letters "**rq**" after the **data** specifier, as in the following examples:

| FirstName is Grant **and** LastName is Hulbert            | eqFirstNamedata**rq**=Grant\&eqLastNamedata**rq**=Hulbert        |
| --------------------------------------------------------- | ---------------------------------------------------------------- |
| Bedrooms > 5 **and** fireplace is True and Bathrooms is 2 | grBedsdata**rq**=5\&eqFireplacedata**rq**=T\&eqBathsdata**rq**=2 |

You can make all fields required in one simple step by putting "**AllReqd=T**" into the form.

The results file would contain a \[founditems] loop that fills with all the matching records, and the resulting HTML would be sent back to the browser.

you can also embed a \[search] context in a template. Any browser accessing that HTML page (no special commands required - just link to the page) will see the 'live' search results. It might look like the following

**Here's an embedded search:**

```
[search db=People.db&eqFirstNameDatarq=Grant&eqLastNamedatarq=Hulbert]
[founditems]
[FirstName], [Address], [City], [State], [Zip]<br>
[/founditems]
[/search]
```

**Search one field two different ways**\
**Search two fields at once**\
Both of these conditions are handled by **grouping fields** and it warrants its own section. Please refer to "Searching multiple fields" below.

**Comparisons**\
You tell WebDNA how to compare by putting the two-letter comparison code in front. For instance, using **gr** means "Field value must be 'greater than' your search word"

<pre><code><strong>grCountyPopData=20000
</strong></code></pre>

| eq                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | equal to                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| ne                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | not equal to                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| gr                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | greater than.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| ge                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | greater than or equal to                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| ls                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | less than                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| le                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | less than or equal to                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| rn                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | <p>range. Specify two values separated by spaces (or any other delimiter specified by <em>wbrk</em>; i.e., ...&#x26;wbrk=,&#x26;...). The values may be dates, numbers, or text. If date, time or number, you must specify what type of data by using fieldnametype=xxx (where xxx is num, date or time).</p><p>Example: rnBirthdaydata=02/28/2008 08/31/2008&#x26;Birthdaytype=date<br>Note: When specifying a range, the smaller value must precede the larger value, i.e. rnZipCodedata=92069 93090, not rnZipCodedata=93090 92069.</p> |
| mr                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | minimum range value: if no maximum then ge is used instead                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| mx                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | maximum range value: if no minimum than le is used instead                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| cl                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | close to (numeric only). clZipCodedata=92069\&clZipCodedata=10 finds all records whose ZipCode field is within 10 of 92069 (92059 - 92079)                                                                                                                                                                                                                                                                                                                                                                                                 |
| bw                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | begins with                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| ew                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | ends with (from 8.6)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ws                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Interpret the words as a single string to be matched (including spaces etc.) This lets you find entire phrases, like "Joe enjoys butter" only if those 3 words are next to each other in that order, including spaces (unlike wa and wo, below)                                                                                                                                                                                                                                                                                            |
| wa                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Separate the words and "and" them together (all must match). Searching for 3 words using wa will match only if all 3 are in the field, but not necessarily next to each other.                                                                                                                                                                                                                                                                                                                                                             |
| wo                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Separate the words and "or" them together (at least one must match - default option for text). Search for 3 words using wo will match if any one of them matches.                                                                                                                                                                                                                                                                                                                                                                          |
| wn                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | word not equal - none of the words match text in the specified field                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| **Word breaks**: Use **Wbrk** to define what constitutes a word. By default, a space is a word delimiter, but you can define your own delimiter(s) using **wbrk**; example: _fieldname_**Wbrk**=,-;.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| **Word position in field**: Use **Word** to define where you want the word to be found. Example: _fieldname_**Word**=ww                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ss                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | SubString match. Looks for word anywhere, even inside larger words. Finds "I like typo**graphic** conventions" and "This is a **graphic**" and "**Graphic**al user interfaces are cool."                                                                                                                                                                                                                                                                                                                                                   |
| ww                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Whole Word match. Looks for whole word; ignores if found inside a larger word. Finds "This is a **graphic**"                                                                                                                                                                                                                                                                                                                                                                                                                               |
| sw                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Start of Word. Looks for word at the beginning of words. Finds "This is a **graphic**" and "**Graphic**al user interfaces are cool."                                                                                                                                                                                                                                                                                                                                                                                                       |
| <p><strong>Case sensitivity</strong>: Conducts case sensitive searches.<br>Examples:<br><em>fieldname</em><strong>Case</strong>=t<br><strong>AllCase</strong>=t (apply to all fields)</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| <p><strong>Finding Empty Fields</strong> and <strong>Find All</strong><br>[blank] is a special WebDNA tag available in searches to explicitly find empty records. In practice, it is more often used to find records which are NOT empty, which is one way to "find all". Here are two popular ways to <strong>Find All</strong>:</p><p><strong>ne</strong><em>fieldname</em>Data<strong>rq</strong>=[blank]<br><strong>ne</strong><em>fieldname</em>Data<strong>rq</strong>=FindAll</p><p>The second example, of course, assumes you don't have a field value of FindAll.</p><p>Alternately, you can use the form <strong>FieldNameBLNK=T</strong>, which makes it possible to use radio buttons or checkboxes to let the visitor decide how they want to search for blank fields. If you want to apply this to all the fields in your search at once, then set <strong>allBLNK=T</strong>.</p> |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |

### Searching multiple fields <a href="#background15" id="background15"></a>

One case you would need to do this would be in a general database search. If you want your users to search a catalog, you can let them search across the title, description, keywords, and any other field, such as author, manufacturer, etc. here is how you would do this:

<pre><code><strong>group1field=title+description+keywords&#x26;wogroup1data=Bird
</strong></code></pre>

"group1" becomes the name of the combined field, and is used just like a regular fieldname with the data parameter. In this case, all three fields are searched and if Bird is found in any of them, the record will be retrieved.

**WARNING:** Grouping WILL NOT work if your field names contain a "-" (hyphen).

**Searching the same field in two different ways**\
Sometimes you need to compare a value two different ways, but the search parameter will only accept a fieldname once. In this case, create two group fields like this:

<pre><code><strong>group1field=zipcode&#x26;group2field=zipcode
</strong></code></pre>

and do what you need to do in your search; for example:

<pre><code><strong>negroup1datarq=08055&#x26;rngroup2datarq=08001 08099
</strong></code></pre>

In this case, all the zip codes in one area are found, but one of them is excluded (perhaps the zip code that is currently being highlighted elsewhere on the page.)

When assigning groups, you must name them sequentially, without skipping numbers.

\


If you plan to work on several groups, the groupfield definitions must be placed at the beginning of the search string. For instance

group1field=title+description\&wogroup1data=Bird\&group2field=title+keyword\&wogroup2data=Bird

has to be written in this order:

group1field=title+description\&group2field=title+keyword\&wogroup1data=Bird\&wogroup2data=Bird

### Sorting the results <a href="#background16" id="background16"></a>

A WebDNA search query allows you to sort and subsort your records in many ways. You may also summarize your records, and by using nested searches, create subheads.

**Hierarchy**\
You can subsort on as many fields as necessary. By default, sort order is ascending, alphabetically. By using a prefix (see table below), you can change this default. By using _fieldname_**type=num/date/time,** you tell WebDNA to sort either numerically, by date, or by time.

Assuming your database contains state, city, and citypop fields:

```
StateSort=1&CitySort=2
```

(Cities appear alphabetically, grouped by State)

<pre><code><strong>StateSort=1&#x26;deCityPopSort=2&#x26;CityPopType=num&#x26;CitySort=3
</strong></code></pre>

(Cities appear in order of largest first, grouped by State. In the rare case that two cities have the exact same population, then they would be sorted alphabetically. Note the use of type=num to tell WebDNA to treat that field numerically. This is important. )

You cannot skip a number in the sorting hierarchy.

**Summaries**\
See the Summary section for ways to use Sorting and Summaries together

| as                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | ascending (the default, if nothing specified) |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------- |
| de                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | descending                                    |
| ra                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | random                                        |
| <p><strong>Maintain randomness</strong>: Sometimes you need a series of random sorts to always present the exact same 'random' order. You can force the randomizer to return items in the same order if you add another parameter "RandSeed=879" (or any other integer number). This is most helpful when you use [ShowNext] and you need each successive set of links to predictably return the results in the same order.<br><strong>RandSeed</strong>=<em>integer</em></p> |                                               |
| <p><strong>No sorting at all</strong>: Results are displayed in exactly the same order they appear in the database file.<br><strong>Rank</strong>=off</p>                                                                                                                                                                                                                                                                                                                     |                                               |

### Summaries <a href="#background17" id="background17"></a>

**Create Headings**\
If we do two searches on a database of cities and states, we can create a heading for each state, and list the cities below. The first search will sort and summarize by state. Within the \[FoundItems] of that search, we perform a second search based on field values from the first search.

```
[search db=yourdatabase.db&neStatedata=[blank]&StateSort=1&StateSumm=t]
[founditems]
<b>[State]</b> <p>
[search db=yourdatabase.db&eqStatedata=[state]&CitySort=1]
[founditems]
[city], population [citypop] <br>
[/founditems]
[/search]
[/founditems]
[/search]
```

When this plays out on the server, WebDNA will sort by state, but only return the first record of each state. As it finds each one, it displays the state name in bold, inserts a paragraph tag, then performs a new search for that state's cities (inserting the known state name in the \[search]), then lists the cites with a line break tag after each one. Then it goes to the next state and repeats the process until the last state is taken care of.

**Counting Unique Values**\
Working from the above example, maybe all we want is to know how many cites are in each state. We will use almost the same code to determine this, but won't display the cites at all.

```
[search db=yourdatabase.db&neStatedata=[blank]&StateSort=1&StateSumm=t]
[founditems]
<b>[State]</b> 
[search db=yourdatabase.db&eqStatedata=[state]]
has [numfound] cities.<p>
[/search]
[/founditems]
[/search]
```

This time, WebDNA finds one record for each state as before, but this time, the second search doesn't care about sorting the cities, nor do we need the \[foundItems] to display them. However, WebDNA still went to all the effort to find the cities that go with each state, and we can use \[numfound] to get that value.

Want to grab the population of each state, by adding the populations of the cities within? No problem with WebDNA. It is easy using the \[math] context.

### Word Breaks and Field Breaks <a href="#background18" id="background18"></a>

**Searches for whole words**

Normally WebDNA searches for any text in a field, regardless of where it is in a sentence or even if it is in the middle of a word. You can tell WebDNA to search for whole words (letters surround by spaces, commas, etc.) or only the beginning of a word by setting word-break options.

You can define your own custom list of word break characters with wbrk=. By adding Descriptionwbrk=.,- to your search causes user-entered text surrounded by those characters in the HTML form field Description to be considered as individual words.

If your database has a field called "Description," and you want to look for records containing "graph" only at the beginning of words in the Description field, the URL would look like this:

<pre><code><strong>[search db=somebase.db&#x26;woDescriptiondata=graph&#x26;Descriptionword=sw&#x26;]
</strong></code></pre>

| word |                                                                                                                                                                                                                        |
| ---- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ss   | Description**word**=**ss** looks for "graphic" and finds it anywhere inside phrases such as: "I like typo**graphic** conventions" and "This is a **graphic**" and "**Graphic**al user interfaces are cool."            |
| ww   | Description**word**=**ww** looks for "graphic" and will **not** find it inside phrases such as: "I like typographic conventions". It **will** be found inside "This is a **graphic.**"                                 |
| sw   | Description**word**=**sw** looks for "graphic" at the beginning of words, like "This is a **graphic**" and "**Graphic**al user interfaces are good", but will **not** find it inside "I like typographic conventions." |

**FBRK (field break) option for WW, and SW field comparisons.**

Specifies the delimiters used when doing a word compare in the field data itself. Thus:

```
woDESCRIPTIONdatarq=2&DESCRIPTIONword=WW&DESCRIPTIONfbrk=[url],[/url]
```

in the search string would not find a result if the field data is "400.2", whereas it would if the FBRK option was not specified.

Basically, "wbrk" tells WebDNA how to break up the values you pass in your search context, while "fbrk" tells WebDNA how to break up the values stored in the database.

### \[founditems] [context](https://docs.webdna.us/contexts-and-tags)

\[founditems], the companion context to \[search], loops through the records retrieved by the search, and provides the means to display data from each record.

The \[founditems] context goes between the search tags. If the search specified a maximum number of matches, then only that many will be displayed in the \[founditems] loop.

Example:

```
[search db=xx.db&eqCITYdata=Chicago]
[founditems]
[index]. <b>[Name]</b>, [Address], [City]<hr>
[/founditems]
[/search]
```

This will display a numbered list of people with a horizontal rule separating each.

When you are inside a search, and inside the \[founditems] tags, you can retrieve all fields from the current record. These values can be easily intermixed with your html, as in the sample above. In addition, \[index] is a counter that has many practical uses beyond the simple numbering seen here.

| \[fieldname] | Enclose any fieldname in square brackets to display the value                                                                                                                                                                                                                                                                                     |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \[index]     | A number indicating this record's placement in the list. Note that this number is not taken from the database, but is purely a counter as the records are retrieved. When records are split into chunks with the \[shownext] context, subsequent pages will start with the index true to the total search, not just the group found on that page. |
| \[break]     | From version 8.1, if the \[founditems] context sees the \[break] tag while executing a loop, it will stop looping, once it finishes the current loop. Thus the \[break] tag should only appear in a \[showif] statement that is evaluated at the end (bottom) of the loop.                                                                        |

You also use the \[founditems] within a \[SQL] context, to list the records resulting from a successfull ODBC/SQL query.

The \[founditems] context is also used within a \[SQLresult] context, to list the records resulting from a successfull Native SQL query.\
When used within \[SQLresult], the new 'Iterate' parameter can be specified to control the behavior of the \[founditems] context.

In setting the 'iterate' parameter to 'manual', the \[founditems] context behaves more like a collection. The WebDNA programmer can retrieve the found records in any manner they wish.

| Parameter                                                               | Description                                                                                                                                                                                                                                                                                                                                                                                                         |
| ----------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><strong>Iterate</strong></p><p>Only available within [SQLresult]</p> | (Optional) - Manual/Auto - When set to MANUAL, the \[founditems] context switches to 'manual' mode. The new \[Seek] tag must then be used to 'move' the record set cursor. The cursor is always initialized to the first record position. When set to Auto - \[founditems] reverts to default behavior, iterating the record set one record at a time, starting from the first item, and ending with the last item. |

The following tags are available inside a \[founditems] context:

| Tag                                                                                       | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| ----------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| The following tags are only valid when \[founditems] is used with a \[SQLresult] context. |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| \[Field _Params_]                                                                         | <p>The [field] tag is used within [founditems] to return information about a particular field of the current record.<br>Parameters:<br><em><strong>seek</strong></em> (required) - The seek parameter uses a prefix-qualified naming semantic to select a specific field of the current record. You can use one of two possible prefixes for the value of the seek parameter... either named or ordinal; follow this with a ":" and the actual value which identifies the field (according to the prefix-qualifier).<br><strong>For example:</strong><br><strong>seek=ordinal:1</strong> - would select the first field<br><strong>seek=named:employeeID</strong> - would select the field named "employeeID"</p><p><em><strong>get</strong></em> (optional) - The get parameter accepts one of seven possible enumerated values:<br>NAME - Retrieves the field name.<br>ORDINAL - Retrives the field position.<br>DATATYPE - Retrievs the field data type.<br>WIDTH - Retrieves the max data length.<br>ALLOWNULL - T/F Resolves to 'T' if the field will accept a NULL value.<br>ISNULL - T/F Resolves to 'T' if the field value is NULL.<br>VALUE - Retrieves the field value.</p><p><em><strong>raw</strong></em> (optional) - When retrieving field data where the field data type is: DATE, TIME, or DATETIME , WebDNA will 'interpret' the data based on the user defined WebDNA preferences for displaying Dates and Times.</p><p>However, if you wish to display the data in its 'raw' format, you can set the 'raw' parameter to 'T'.</p> |

**For example:**

```
[founditems]
[dateacquired]
[field seek=named:dateAcquired&get=value&raw=T]
[/founditems]
```

This would result in something like...\
04/23/2010\
2010-04-23

You can see that the second line displays the data value in its 'native' format (MySQL stores dates in YYYYY-MM-DD).

| \[Seek _Params_] | <p>When the 'iterate' parameter for [founditems] is set to 'manual'. The [seek] tag is used to position the result set 'cursor' to a particular record.</p><p><br>Parameters:<br><em><strong>To</strong></em> (required) - Can be one of four values:<br>First - set the cursor to the first record.<br>Last - set the cursor to the last record.<br>Prev - set the cursor back one record.<br>Next - set the cursor ahead one record.</p> |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

**Example WebDNA code:**

```
[SQLresult result_ref=rs1]
List records in reverse order:
<table>
<tr><th>ID</th><th>Date Acquired</th><th>Comments</th></tr>
[founditems iterate=manual]
[seek to=last]
[loop start=1&end=[numfound]]
<tr><td>[inventoryID]</td><td>[dateAcquired]</td><td>[comments]</td></tr>
[hideif [index]=[numfound]][seek to=prev][/hideif]
[/loop]
[/founditems]
</table>
[/SQLresult]
```

### \[replacefounditems] WebDNA v4.0 [context](https://docs.webdna.us/contexts-and-tags)

Replaces each found record in a database with the new field values.

\[replacefounditems]field1=value1\&field2=value2\[/replacefounditems]

To replace records in an ODBC-compliant table controlled by a SQL server, use the \[SQL] context.

To replace field values of records in a database, put a \[replacefounditems] context into a template inside a \[search] context. As each matching record is found, that record's fields inside the \[ReplaceFoundItems] context are replaced with new values.

| \[index] | A number indicating this record's placement in the list. Note that this number is not taken from the database, but is purely a counter as the records are retrieved. |
| -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

**Example WebDNA code:**

```
[search db=base.db&neRefdata=0&Refsort=1]
[replacefounditems]Ref=[index][/replacefounditems]
[/search]
```

this context is much faster than the old technique of nesting a \[replace] context inside a \[founditems] context. For example: if you currently use something like this to modify many records in a database...

\


```
[search db=xx.db&neSKUdata=0]
[founditems]
[replace db=xx.db&eqSKUdata=[sku]]value=[math][value]+1[/math][/replace]
[/founditems]
[/search]
```

...then you can change it to the following in order to speed it up considerably:

```
[search db=xx.db&neSKUdata=0]
[replacefounditems]value=[math][value]+1[/math][/replacefounditems]
[/search]
```

**Example WebDNA code:**

```
[search db=products.db&neSKUdata=0]
[replacefounditems]price=[math][price]*1.1[/math][/replacefounditems]
[/search]
```

In the example above, the database "products.db" opens, all records whose sku field is not "0" found, and each of those found record's price fields incremented by 10%. As each found record is visited, that record's field values are available inside the context so you can use them to compute new values.

This behavior is very different from the simpler \[replace] context, which replaces all found items with the same value.

Any fieldnames that do not exist in the database are ignored, and if you leave some existing fieldnames out of the replace context, they will remain unchanged in the database. Certain letters are illegal, such as

or , so they are converted to and before being added to the database. Some computers use the two-character sequence to indicate a single end of line, which is automatically converted to a single character before being added to the database. These 'soft' characters are automatically converted back to 'hard' versions (the originals) whenever you retrieve fields from a search of the database.

You may specify an absolute or relative path to the database file, as in "/WebCatalog/GeneralStore/somebase.db" or "../somebase.db". You may also place "^" in front of the database path to indicate that the file can be found in a global root folder called "Globals" inside the WebCatalogEngine folder.

\


| Contrast between \[replacefoundItems] and \[replace]                                                                                          |                                                                            |             |     |              |             |
| --------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------- | ----------- | --- | ------------ | ----------- |
| <p>[search db=products.db&#x26;neSKUdata=0]<br>[replacefoundItems]<br>price=[math][price]*1.1[/math]<br>[/replacefoundItems]<br>[/search]</p> | <p>[replace db=products.db&#x26;neSKUdata=0]<br>price=10<br>[/replace]</p> |             |     |              |             |
| SKU                                                                                                                                           | price before                                                               | price after | SKU | price before | price after |
| 1                                                                                                                                             | 5                                                                          | 5.5         | 1   | 5            | 10          |
| 2                                                                                                                                             | 10                                                                         | 11          | 2   | 10           | 10          |
| 3                                                                                                                                             | 15                                                                         | 16.5        | 3   | 15           | 10          |
| 4                                                                                                                                             | 20                                                                         | 22          | 4   | 20           | 10          |
| 5                                                                                                                                             | 35                                                                         | 38.5        | 5   | 35           | 10          |

### \[shownext] [context](https://docs.webdna.us/contexts-and-tags)

\[shownext] is a special context used to create links (or more accurately, the sets of ranges of found items - it's up to you to make them into links) to the next or previous sets of results.

Put \[shownext] inside the search context, but not inside \[founditems]. To sucessfully use \[shownext], you need to add some matching parameters in the \[search] context, as shown in the example.

\[shownext] is a looping type of context. It will repeat until all the sets of ranges are accounted for. In this example, we've used a pipe character to separate the sets. Each repeat of the \[shownext] will end in the pipe character, separating the links from each other. There are three different \[shownext] contexts to handle the before, current, and after sets of ranges.

**Example for a file named "states.dna"**

```
<!--HAS_WEBDNA_TAGS-->[search db=states.db&geSTATEdata=1&refsort=1&max=10&startat=[starthere]][shownext position=begin]
<a href="states.dna?state=[url][state][/url]&starthere=[start]"> [start]-[end]</a> |
[/shownext]
[shownext position=middle]
<b>[start]-[end]</b> |
[/shownext]
[shownext position=end]
<a href="states.dna?state=[url][state][/url]&starthere=[start]"> [start]-[end]</a> |
[/shownext]
<br>
[founditems]-[state]<br>[/founditems][/search]
```

your database "states.db" should look like

```
ref    state
1      Alabama
2      Alaska
3      American Samoa
4      Arizona
5      Arkansas
6      California
7      Colorado
8      Connecticut
9      Delaware
10     District of Columbia
11     Florida
...
```

**Explanation of the Example**\
The \[search] context uses a variable for **\[state]**. Imagine that on the previous page the user had a choice of states to click on. If they click on Texas, then we need to remember Texas and carry it through to the rest of the pages, because each page performs the search all over again. This is why we include **state=\[state]** in the Shownext links. It will resolve to state=Texas, and the search for the next pages will be correct.

\[search] also uses **startat=\[starthere]**, and the shownext links use **starthere=\[start]**. This is perhaps the most confusing part of the shownext context. **startat=** is part of the search context and must be entered as such. **\[start]** is a special tag used in the \[shownext] context, and reveals the starting index for a given range of records. In our example, _\[starthere]_ is our own, made-up name for a variable we will use to pass the value of \[start] on to the next page, where it lands in the search. We could just have easily used \[elephant], or \[start] or \[startat]. The choice is yours, but the idea is to not add more confusion.

| **Shownext-specific parameters for the Search Context** |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| ------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| startat                                                 | This specifies where, in the total search that is performed, the current page's worth of found items will begin. If it is missing or invalid (i.e., the raw code \[starthere] appears), it will start at the beginning.                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| max                                                     | (not to be confused with the max in the shownext context) This specifies how many in each set of records.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| **Parameters specific to the ShowNext context**         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| position                                                | <p><strong>begin</strong> - Displays the ranges ([start]-[end]) for records before the ones currently displayed inside the [founditems] loop<br><strong>middle</strong> - Displays the ranges for records currently displayed inside the [foundItems] loop (usually not as links, but perhaps bolded or styled in some way to show the user that this is the range they are currently seeing.)<br><strong>end</strong> - Displays the ranges for records after the ones currently displayed inside the [foundItems] loop<br><strong>parameter not used</strong> - Defaults to show the entire range of before and after, skipping the current (middle) range.</p> |
| max                                                     | (not to be confused with the max in the search context) this specifies how many sets of ranges to display. If set to 1, then you can show a simple "next" link.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| **Tags available inside the ShowNext context**          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| \[start]                                                | The index position of the first item that will be displayed in this range of found items                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| \[end]                                                  | The index position of the last item that will be displayed in this range of found items                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| \[searchstring]                                         | <p>All search parameters used to find this "chunk" of items in the range [Start] - [End]. However, this tag is not compatible with the search context. It is a tag from early versions of WebDNA, when using the less secure URL string or formvariables to perform a search, and for security reasons, is not a recommended technique today.</p><p>In modern usage, you will construct your URL in the shownext context to contain only those variables necessary to perform the search, plus the starting value. See example above.</p>                                                                                                                         |

You will find sophisticated examples at "a \[shownext] example", "another \[shownext] example" and "an example of application for \[shownext]"

You can also use the WebDNA flexibility to find an alternative solution, like this alternative to \[shownext] code.

### \[lookup] [tag](https://docs.webdna.us/contexts-and-tags)

```
[lookup db=databasePath&[!]
[/!]value=searchValue&[!]
[/!]lookInField=searchField&[!]
[/!]returnField=fieldName&[!]
[/!]notFound=TextIfNotFound]
```

There is no need to close this tag: it will be directly replaced by the result

Using \[lookup] in your template performs an extremely fast search through the specified database and returns either the value of the returnField in the found record, or the literal text of the notFound value. The search is a case-sensitive, exact match, so "Grant" does not equal "grant." If you want more control over the search criteria, use a \[search] context instead.

You can specifiy a WebDNA table, in place of a db file.\
For example: \[Lookup table=TableName&...].

\[lookup] is about 40 times faster than \[search]. Using several \[lookup] to recover data from your bases might be faster than a single \[search]
