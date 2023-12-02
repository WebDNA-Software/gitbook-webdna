# How to use MISCELLANEOUS features of WebDNA

### \[! ] WebDNA v3.0+ [context](https://docs.webdna.us/contexts-and-tags)

\[!] is WebDNA for a "comment" inside a template. Anything inside the context will not be processed or displayed in the rendered document.

```
[!]This text can only be seen by someone that can open the document in an editor[/!]
```

Often notes are required for developers in a WebDNA template, yet these notes are not to be displayed to a visitor. Unlike HTML comments, which are sent to the browser even though they are hidden from view and can be seen with a "View Source" in the browser, WebDNA comments never get sent to the remote browser. This shortens download times, and provides a degree of protection against prying eyes.

Do not be confused with the html comment, if you put WebDNA inside an html comment the code will still render and could effect your result and will be able to be seen by visitors using a "View Source" command in the browser.

### \[formvariables] [context](https://docs.webdna.us/contexts-and-tags)

To list all the variables that are passed in a form or url, \[formvariables] will display them all in name-value pairs.

```
[formvariables][name] = [value][/formvariables]
```

\


**Parameters**

| Parameter | Description                                                                                                                                                |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Name      | (Optional) The name of the field to list.                                                                                                                  |
|  Exact    | (Optional) T(rue) or F(alse) whether to exactly match the name of the parameter or match any name that contains the "name" value. (Default value is true). |
| Form      | (Optional) Use form=include to retrieve the list of form variables from the \[include file=xxx\&var1=yyy\&var2=zzz] tag in this template.                  |

**Tags available inside a \[formvariables] context:**

| Tag      | Description                                                                                                                                                                                                                                                            |
| -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \[index] | A number indicating this variable's placement in the list                                                                                                                                                                                                              |
| \[name]  | The name of the variable                                                                                                                                                                                                                                               |
| \[value] | The value associated with the variable                                                                                                                                                                                                                                 |
| \[break] | From v8.1, if the \[formvariables] context sees the \[break] tag while executing a loop, it will stop looping, once it finishes the current loop. Thus the \[break] tag should only appear in a \[showif] statement that is evaluated at the end (bottom) of the loop. |

This context is extremely useful to debug a form.\
To display a list of all the form variables available, put a \[formvariables] context into a template.

**Example WebDNA code:**

```
The following will list all the form variables available to a page:
[formvariables]
  [index], [name] = [value]<br>
[/formvariables]
```

The \[formvariables] context has optional parameters in order to modify the list of form variables produced.\
**Example WebDNA code:**

```
The following are the values of all the form variables with the name "text":
[formvariables name=text&exact=T]
  [index], [name] = [value]<br>
[/formvariables]
```

### \[listvariables] WebDNA v4.0+ [context](https://docs.webdna.us/contexts-and-tags)

To list all the text and/or math variables which have been set earlier on a page, \[listvariables] will display them all in name-value pairs.

```
[listvariables][name] = [value][/listvariables]
```

**Parameters**

| Parameter | Description                                                                                                                                                |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Name      | (Optional) The name of the variable to list.                                                                                                               |
|  Exact    | (Optional) T(rue) or F(alse) whether to exactly match the name of the parameter or match any name that contains the "name" value. (Default value is true). |
| Type      | (Optional) Text or Math. Default is to list variables of both types. Specify Text for Text variables only, Math for Math variables only.                   |

**The following tags are available inside a \[listvariables] context:**

| Tag       | Description                                                                                                                                                                                                                                                            |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \[name]   | The name of the variable.                                                                                                                                                                                                                                              |
| \[value]  | The value associated with the variable.                                                                                                                                                                                                                                |
| \[index]  | A number from 1 to the total number of variables, indicating this field's index position in the list                                                                                                                                                                   |
| \[secure] | Displays "T" if the variable is secure, "F" otherwise.                                                                                                                                                                                                                 |
| \[break]  | From v8.1, if the \[listvariables] context sees the \[break] tag while executing a loop, it will stop looping, once it finishes the current loop. Thus the \[break] tag should only appear in a \[showif] statement that is evaluated at the end (bottom) of the loop. |

This context is extremely useful to debug a page.\
To display a list of all the text/math variables available, put a \[listvariables] context into a template.

**Example WebDNA code:**

```
The following will list all the text/math variables available to a page:
[listvariables]
  [index], [name] = [value]<br>
[/listvariables]
```

The \[listvariables] context has optional parameters in order to modify the list of text/math variables produced.\
**Example WebDNA code:**

```
The following are the values of all the text/math variables with the name "shipping":
[listvariables name=shipping&exact=T]
  [index], [name] = [value]<br>
[/listvariables]
```

**Example WebDNA code:**

```
The following are the values of all the math variables:
[listvariables type=math]
  [index], [name] = [value]<br>
[/listvariables]
```

### \[loop] [context](https://docs.webdna.us/contexts-and-tags)

\[loop] is designed to repeat html or WebDNA code. The \[loop] context requires two parameters: start and end. Looping occurs between, and including, the start and end value; advancing by 1 each time. An "advance" value may optionally tell the loop context to advance by a specified amount, other than 1, by using the advance parameter.

**Parameters**

| Parameter | Description                                                                                                                           |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| start     | The starting index value to begin looping. This value is required and may be positive or negative.                                    |
| end       | The ending index value when looping. This value is required and may be positive or negative.                                          |
| advance   | (Optional) The amount to advance by when looping. This parameter is optional and may be positive or negative, the default value is 1. |

**The following tags are available inside a \[loop] context:**

| Tag      | Description                                                                                                                                                                                                                                        |
| -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \[index] | Displays the current index number between or including **start** and **end**.                                                                                                                                                                      |
| \[break] | If the \[loop] context sees the \[break] tag while executing a loop, it will stop looping, once it finishes the current loop. Thus the \[break] tag should only appear in a \[showif] statement that is evaluated at the end (bottom) of the loop. |

**Example WebDNA code:**

```
[loop start=1&end=4&advance=1]
  [append db=some.db]...[/append]Record [index] added.<br>
[/loop]The result would be:
Record 1 added.
Record 2 added.
Record 3 added.
```

### \[include] [tag](https://docs.webdna.us/contexts-and-tags)

Including the \[include] tag in a template will include the contents of the specified file.

**Parameters**

| Parameter     | Description                                                                                                                              |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| file          | The path to the file to be included                                                                                                      |
| raw           | (optional) T/F raw=T means the file should be included unchanged, without performing any \[xxx] substitutions.                           |
| fromcache     | (optional) T/F fromcache=F means a more-recent version of the file should be read from disk, instead of using the cached version in RAM. |
| variable name | Passes any variable names and their values into the included file, which can then use the passed varaibles.                              |

Normally all file paths are relative to the local template. To ensure that the path to the file is correct, the file path can begin with "/" so that the file is referenced relative to the web server's virtual host root.

**Example WebDNA code:**

```
[include file=../inc/footer.inc]
This results with the footer file being included in the current template using the relative path to the included file.[include file=/inc/footer.inc]
This results with the footer file being included in the current template using the path to the included file
from the web server's virtual host root.
```

**Example WebDNA code:**

```
[include file=../inc/footer.inc&var1=a&var2=b]
This results with the footer file being included with the two variables var1 and var2 being passed into the file.
```

Do not use \&raw=T and \&fromcache=F together in the same \[include]\
This will error: \[include file=page.inc\&raw=T\&fromcache=F]

### \[function] WebDNA v5.0+ [context](https://docs.webdna.us/contexts-and-tags)

This context enables the WebDNA programmer to call a previously defined block of WebDNA code.

\[function] This context enables the WebDNA programmer to call a previously defined block of WebDNA code. Functions allow you to create your own tags to use just as you would any other predefined WebDNA tag. A function will override a native WebDNA tag, so you could, for instance, redefine the date and time tags to match a different time zone, if your server is not local to you. A function is a handy way to perform a complicated series of instructions so you need only maintain one piece of code, but use it a number of times in your page.

**Example WebDNA code:**

```
[function name=citytime]
[format seconds_to_time %I:%M %p][math]{[time]}+{[more]:00:00}-{[less]:00:00}[/math][/format]
[/function]<b>Current time around the world</b>
[citytime more=0&less=0] - New York City<br>
[citytime more=0&less=3] - Seattle<br>
[citytime more=7&less=0] - Paris
```

In this example, we feed the function two variables, \[more] and \[less]. The code within the page is clean and clear, and we use only one function to return multiple values. If we decide to change the way the times are formatted, we only need to change the single chunk of code. (The server in the example is in New York City, so we define \[more] and \[less] relative to the server time.)

| name     | User defined name for the function. The name is then used like any other WebDNA tag, as in our example.                                                                                                                                                                                                                                                                                                                                                                                             |
| -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| preparse | T/F Optional. By default, the WebDNA code that makes up the function is stored 'raw' and executed later when the function is called, as in our citytime example above, because the variables are, well, variable. But if you need to use WebDNA to create the function in the first place, then you can set 'preparse' to 'T'. This will force the WebDNA engine to first parse the WebDNA in the function definition before storing it for later use. Usage: \[function name=somename\&preparse=t] |

This new example creates a function named 'backwards' that will take a variable named 'instring' and display the characters of the string in reverse order. We use the following code...

```
[function name=Backwards]
[text]length=[countchars][instring][/countchars][/text]
[loop start=[length]&end=1&advance=-1][getchars start=[index]&end=[index]][instring][/getchars][/loop]
[/function]
```

Now the function is defined and stored for later use in the template.\
To execute the new function, we use...

```
[Backwards instring=abcdef_12345]
```

### The power of \[function] <a href="#background5" id="background5"></a>

Using the \[function ]\[/function] context, one can extend WebDNA's power to near limitless potential.

A "Function" could be something as simple as outputing static info:

```
[!] ** create primary numbers function ** [/!]
[function name=PrimeNums]
[return]2,3,5,7,11,13,17,19,23,29[/return][/function]
```

then

```
[!] ** Display primary numbers function ** [/!]
[PrimeNums]
```

Or, a function could be a something more logical.\
For example, have the option of returning a random prime number:

```
[!] ** create primary numbers function ** [/!]
[!] ** adding a param called fmode ** [/!]
[function name=PrimeNums]
[!] ** table of primes (could be a .db file, or MySQL database as well) ** [/!]
[table name=primes&fields=pnum]
2
3
5
7
11
13
17
19
23
29
[/table]
  [switch value=[fmode]]
    [case value=random]
      [search table=primes&[!]
        [/!]nePNUMdatarq=find_all&[!]
        [/!]raPNUMsort=1&[!]
        [/!]PNUMtype=num[!]
        [/!]&max=1]        [return][founditems][PNUM][/founditems][/return]
      [/search]
    [/case]
    [default]
      [!] ** find all primes ** [/!]
      [search table=primes&nePNUMdatarq=find_all]
        [return][founditems][PNUM],[/founditems][/return]
      [/search]
    [/default]
  [/switch]
[/function]
```

Now, from the above, you can call this function with either:

```
[!] ** Find all Prime Numbers ** [/!]
[PrimeNums]
```

or:

```
[!] ** Find a random prime number ** [/!]
[PrimeNums fmode=random]
```

The above is untested, but you get the idea! Create a WebDNA Lab (in the admin prefs of WebDNA) to find a great tutorial on the \[function]\[/function] context. There are many other great ways to use it.

What is a Function Library?:\
Libraries are usually a collection of functions that may have a themed purpose. A library theme could be as focused as, for example, functions to deal with image manipulation, or as loosely focused, for example, as "Donovan's commonly used functions". However, the idea is that you end up with a library that can be easily "loaded" into your site for extensible use with your WebDNA site. This is the open source and limitless area of WebDNA. Anyone can create and share a function library.

What format is a function library?:\
There are a couple/few "formats" that could be created for your library.. but the traditional method is the best. This method is a simple text file with a specific ending suffix such as: "mylibrary.dnalib" or "mylibrary.inc"

if you use a custom suffix such as "dnalib", you should add this mapping within your WebDNA prefs so that WebDNA knows to process this file type. First, find your "WebCatalogEngine" directory and edit the file "webdna.conf" so that "dnalib" is added along with all the other mappings (towards the end of the file). Then, in your admin prefs, add .dnalib as an allowable suffix. (you can avoid the extra mapping stuff by using ".inc"

So, a very small sample function library might look like:\
mylibrary.inc

```
[!] ** donovan's sample library ** 
    ** File: mylibrary.inc **     ** created Mar 23 2009 **
    ** function prefix convention "ml_"
[/!][!]    ** NAME: "ml_list"
    ** DESCRIPTION: Returns a list of available functions in this library ** 
    ** INPUT: none
    ** OUTPUT: List of function names[/!][function name=ml_list]
[return]Ml_List,Ml_PrimNums[/return]
[/function][!]    ** NAME: "ML_PrimeNums"
    ** DESCRIPTION: Library for dealing with Prime Numbers ** 
    ** INPUT: "fmode"
               params:
                 "random"
               (leave blank to find all prime numbers)
    ** OUTPUT: Prime Numbers[/!][function name=Ml_PrimeNums]
[!] ** table of primes (could be a .db file, or MySQL database as well) ** [/!]
[table name=primes&fields=pnum]
2
3
5
7
11
13
17
19
23
29
[/table]
  [switch value=[fmode]]
    [case value=random]
      [search table=primes&[!]
        [/!]nePNUMdatarq=find_all&[!]
        [/!]raPNUMsort=1&[!]
        [/!]PNUMtype=num[!]
        [/!]&max=1]        [return][founditems][PNUM][/founditems][/return]
      [/search]
    [/case]
    [default]
      [!] ** find all primes ** [/!]
      [search table=primes&nePNUMdatarq=find_all]
        [return][founditems][PNUM],[/founditems][/return]
      [/search]
    [/default]
  [/switch]
[/function]
```

(again, the above is untested and is just to give you an idea of the format of things)

How To Load a Function Library:\
There are several ways to load a library, but the two best ways are to simply it include it on the page where you want to use it:

```
[include file=mylibrary.inc]
```

Or, you can drop the file in your "Globals/FunctionDefs/" directory, and then turn on your "pre-parse" function within the WebDNA Preferences, to have your library of functions available to you on all your sites and pages at all times!

With the module version of WebDNA, the Globals directory is in your "WebCatalogEngine" directory.. however, if you want access to your library from within a WebDNA Sandbox, you will have to find the "Globals" directory for that particular sandbox. By default, this would be "WebCatalogEngine/SandBoxes//Globals"

Then, call your functions!:

```
[!] List of available functions in mylibrary.inc [/!]
[Ml_list][!] List all prime numbers [/!]
[Ml_PrimeNums]
```

Happy function library creating!



### \[random] [tag](https://docs.webdna.us/contexts-and-tags)

will display a random number

Putting \[random] in your template displays a random number between 0-99. An optional format is a floating-point number between 0.0 and 1.0, with a granularity of 32768 intermediate values. \[random format=float] will generate this alternate floating-point number.

Putting \[lastrandom] in your template displays the same value as the last \[random] number displayed. The format of the number will match whatever format was used in the previous \[random] use.

### \[arrayset] WebDNA v5.0+ [context](https://docs.webdna.us/contexts-and-tags)

Array Operation

\[arrayset name=...\&dim=....]

\[/arrayset]

You can't code PHP without knowing about array's because array's are just about the basis of PHP. WebDNA does not need active use of array's like other languages because of its flat-file database system: with WebDNA, array's can easily and advantageously be replaced by databases to serve as virtual unlimited permanent array's.

So far you have been using variables quite a lot. You have put numbers into \[math ] variables, and you have put text into \[text] variables. But you have only done this one at a time: you've put one number into a variable, or one string of text. So one variable was holding one piece of information. An array is a variable that can hold more than one piece of information at a time.\
An array allows the programmer to store more than one value in a variable, whilst retaining a single reference. If we think of a variable as a single slot in memory (or a box) that can contain data of a certain type - number, character, etc. - then an array is the equivalent of a box divided into partitions, each containing a piece of data.

\
The array context will allow the WebDNA programmer to create an array data object with up to five dimensions.

A multi-dimensional array is an array of arrays. If we think of a one-dimensional array as a row of slots in a box, then a two dimensional array is a grid of slots, and a three dimensional array is a cube of slots. Beyond three dimensions it becomes difficult to conceptualize, but theoretically at least, arrays can have any dimension. WebDNA allows up to 5 dimensions

\
**Parameters**

\-Name - The variable name you specify to represent the array object.\
\-Dim - Comma delimited series of numbers representing the array dimension sizes, i.e. '3,4,5', which would represent a 3x4x5 dimensioned array:

![](https://docs.webdna.us/images/3dimarray.gif)\
**Here is a simple example of a single dimension array:**

```
[arrayset name=DOW&dim=7](1)=Sun&(2)=Mon&(3)=Tue&(4)=Wed&(5)=Thu&(6)=Fri&(7)=Sat[/arrayset]
```

\
...the tag '\[DOW(7)]' would resolve to 'Sat'

You can replace array values with new ones:

```
[arrayset name=DOW](7)=joker[/arrayset]
```

\
...the tag '\[DOW(7)]' would resolve to 'joker'

and add new ones, only if the original arrayset context specifies a size allowing to add new values:

```
[arrayset name=DOW&dim=8](1)=Sun&(2)=Mon&(3)=Tue&(4)=Wed&(5)=Thu&(6)=Fri&(7)=Sat[/arrayset]
[arrayset name=DOW](8)=Next week[/arrayset]
```

\
...the tag '\[DOW(8)]' would resolve to 'Next week'

**Here an example of a three-dimensional array:**

```
[arrayset name=myarray&dim=2,2,2]
(1,1,1)=value1&(1,1,2)=value2&(1,2,1)=value3&(1,2,2)=value4
[/arrayset]
```

\
This creates an array, called 'myarray', that will persist for the duration of the template. Note that the 'index=value' pairs within the \[arrayset] tags are optional when creating a new array. Array indices can be assigned at any time.

When the 'dim' parameter is supplied with the arrayset context, it implies that this will be a new array object. Subsequent calls to arrayset, using the same variable name (and without using the �dim� parameter), imply new index assignments to an existing array object.

So after we have created the 'myarray' object using the line shown above, we can assign index value at a later time using:

```
[arrayset name=myarray](2,1,2)=value6&(2,2,2)=value7[/arrayset]
```

\
Once you have created an array, you can access the 'elements' of the array using the following methods:

You can use a 'global tag' reference to retrieve a single array element. Here is the syntax for the global method:

```
[<arrayname>(<index>)]
```

\


```
[myarray(1,1,1)]
[myarray(1,1,2)]
[myarray(1,2,2)]
[myarray(1,2,1)]
[myarray(2,1,1)]
[myarray(2,1,2)]
[myarray(2,2,2)]
[myarray(2,2,1)]
```

\
would show

value1\
value2\
value4\
value3

value6\
value7

Another way would be to use the \[arrayget] context to retrieve several index values. **Here we will discuss using the \[arrayget] contex**

```
[arrayget name=myarray]
[loop start=1&end=2]
[loop start=1&end=2]
[loop start=1&end=2]
Index [::::index],[::index],[index] = ([::::index],[::index],[index])<br>
[/loop]
[/loop]
[/loop]
[/arrayget]
```

\[arrayget] takes just one parameter; 'name', in which you place an existing array variable name. The (a,b,c,d,e) patterns evaluate to the corresponding index values. These patterns can be intermingled with other text, as shown.

Index 1,1,1 = value1\
Index 1,1,2 = value2\
Index 1,2,1 = value3\
Index 1,2,2 = value4\
Index 2,1,1 =\
Index 2,1,2 = value6\
Index 2,2,1 =\
Index 2,2,2 = value7

**\[numdims] Tag**\
You can use the \[numdims] tag to retrieve the number of dimensions for an array. For example:

```
[arrayset name=array_1&dim=3,3,10][/arrayset]
array_1 contains [arrayget name=array_1][numdims][/arrayget] dimensions.
```

\
Results:\
array\_1 contains 3 dimensions.

You can also use the 'array name' global tag to retrieve the number of dimension in a named array object. Here is the code:

```
[arrayset name=array_1&dim=3,3,10][/arrayset]
array_1 contains [array_1(numdims)] dimensions.
```

\
Results:\
array\_1 contains 3 dimensions.

**\[dimsize\_] Tag**\
The \[dimsize\_] tag is used to retrive the size of a given array dimension.\
Example:

```
[arrayset name=array_1&dim=3,4,5][/arrayset]
[arrayget name=array_1]
array_1 has [numdims] dimensions:
[loop start=1&end=[numdims]]
Dimension [index] has [interpret][dimsize_[index]][/interpret] indexes.
[/loop]
[/arrayget]
```

\
Results:\
array\_1 has 3 dimensions:\
Dimension 1 has 3 indexes.\
Dimension 2 has 4 indexes.\
Dimension 3 has 5 indexes.

You can also access the dimension sizes using the global '**array name**' tag, as follows:

```
[arrayset name=array_1&dim=3,4,5][/arrayset]
array_1 has [array_1(NumDims)] dimensions:
[loop start=1&end=[array_1(NumDims)]]
Dimension [index] has [interpret][array_1(DimSize_[index])][/interpret] indexes.
[/loop]
```

\
Results:\
array\_1 has 3 dimensions:\
Dimension 1 has 3 indexes.\
Dimension 2 has 4 indexes.\
Dimension 3 has 5 indexes.

### \[arrayget] WebDNA v5.0+ [context](https://docs.webdna.us/contexts-and-tags)
