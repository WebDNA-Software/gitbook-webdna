# JSON

### \[JSONstore] WebDNA v8.2+ [context](https://docs.webdna.us/contexts-and-tags)

\[JSONstore] stores a multidimentional JSON object into a WebDNA database

JSON standard is language-independent and its data structures, arrays and objects, are universally recognized. These structures are supported in some way by nearly all modern programming languages and are familiar to nearly all programmers. These qualities make it an ideal format for data interchange on the web. It handles different types of data: numbers, strings, boolean, arrays... The problem relies in JSON objects structure that can be complicated to handle as it can include other nested objects, arrays, arrays with objects, objects with nested arrays, objects with nested objects, object with nested arrays and objects... a number of structures that are difficult to recognize and parse and even more complicated to store.

The new tag is a "cell" \[JSONstore] that can analyze, recognize, parse and store JSON object into a WebDNA database. It is solid and stucture-independant.

Let's see an example of a JSON Object with a nested array; this should not be too difficult for a human to read

**Example WebDNA code:**

```
{
"first": "John",
"last": "Doe",
"age": 39,
"interests": [ "Reading", "Mountain Biking", "Hacking" ],
"favorites": {
  "color": "Blue",
  "sport": "Soccer",
  "food": "Spaghetti"
},
"skills": [
  {
    "category": "JavaScript",
    "tests": [
      { "name": "One", "score": 90 },
      { "name": "Two", "score": 96 }
    ]
  },
  {
    "category": "CouchDB",
    "tests": [
      { "name": "One", "score": 79 },
      { "name": "Two", "score": 84 }
    ]
  },
  {
    "category": "WebDNA",
    "tests": [
      { "name": "One", "score": 97 },
      { "name": "Two", "score": 93 }
    ]
  }
]
}
```

We will make WebDNA store it in a database: simple data go the usual way, like

**Example WebDNA code:**

```
first=John
last=Doe
age= 39
```

The arrays will have a field with ":array" appended to the field name so WebDNA knows it is an array and the content should not be parsed; in this example, we have two arrays, that will be store into fields interests:array and skills:array:

**Example WebDNA code:**

```
interests:array=["Reading","Mountain Biking","Hacking"]skills:array=[{"category":"JavaScript","tests":[{"name":"One", "score":90},{"name": "Two", "score": 96 }]},{"category":"CouchDB","tests":[{"name":"One","score":79},{"name":"Two","score":84 }]}, {"category":"WebDNA","tests":[{"name":"One","score":97},{"name":"Two","score":93}]
```

Nested objects will be parsed and stored by appending the second level field name to the first one, separated by a colon

**Example WebDNA code:**

```
favorites:color=blue
favorite:sport=Soccer
favorite:food=Spaghetti
```

First, prepare your database to receive the JSON Object; it will include these fields

**Example WebDNA code:**

```
first - last - age - interests:array - favorites:color - favorite:sport - favorite:food - skills:array
```

If the database does not exist, then WebDNA will automatically create it with the proper fields

Then, using the new cell \[JSONstore] and the database name

**Example WebDNA code:**

```
[JSONstore db=whatever.db]
JSONobject
[/JSONstore]
```

From version 8.2, it is possible to use a table with \[JSONstore table=whatever.db]

A debug=on option is available to know exactly what and how it is stored

by replacing JSONobject by the actual object, the cell \[JSONstore] will populate the database like this, automatically

**Example WebDNA code:**

```
first=John
last=Doe
age= 39
interests:array=["Reading", "Mountain Biking", "Hacking"]
favorites:color=Blue
favorites:sport=Soccer
favorites:food=Spaghetti
skills:array=[{"category": "JavaScript", "tests": [{ "name": "One", "score": 90 }, { "name": "Two", "score": 96 }]}, { "category": "CouchDB", "tests": [ { "name": "One", "score": 79 }, { "name": "Two", "score": 84 }]}, { "category": "WebDNA", "tests": [ { "name": "One", "score": 97 }, { "name": "Two", "score": 93 }]}]
```

Sorting the data at a later time to recompose the original JSON Object is straightforward

**Example WebDNA code:**

```
{
"first": "[first]",
"last": "[last]",
"interests": [interest:array],
"favorites": {
  "color": "[favorite:color]",
  "food": "[favorite:food]"
  "sport": "[favorite:sport]"
              }
  "skills": [skills:array]
}
```

You can then use this data to feed JQUERY.

### \[JSONstore2] WebDNA v8.6.4+ [context](https://docs.webdna.us/contexts-and-tags)

\[jsonstore2] flattens the array and saves it in the database with the full path name for each node.\
The code allows JSON with colons - or any character - in the node names : everything is allowed except tabs/linefeeds/etc.
