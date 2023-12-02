# Technical features

### \[calcfilecrc32] WebDNA v5.0+ [context](https://docs.webdna.us/contexts-and-tags)

To ensure that a downloaded file has not been changed during transmission use \[calcfilecrc32] to calculate the CRC32 value the file.

```
[calcfilecrc32 file=checkthisfile.txt]
```

The result is a value in bits.

CRC32 is a 32-bit Cyclic Redundancy Check code, used mainly as an error detection method during data transmission. If the computed CRC bits are different from the original (transmitted) CRC bits, then there has been an error in the transmission. If they are identical, it can be assume that no error occurred (there is one chance in 4 billion that two different bit streams have the same CRC32). The idea is that the data bits are treated as a data polynomial and the CRC bits represent the remainder of the division of the data polynomial by a fixed, known polynomial (called the CRC polynomial).

From WebDNA v8.1, to calculate the CRC32 of a string, use \[calcfilecrc32]\[string]\[/calcfilecrc32]

### \[ddeconnect] [context](https://docs.webdna.us/contexts-and-tags)

\[ddeconnect Parameters]DDESend Contexts\[/ddeconnect]\
Connects to a DDE server program on the local machine.\
To embed the results of a DDE command into one of your pages, put a DDEConnect context into a template, and put \[DDESend] contexts inside of that. The DDESend steps contained inside the context are executed, and any returned value is displayed in place of the context. Any \[xxx] tags inside the context are first substituted for their real values before the DDE is executed.

DDEConnect does nothing by itself. You must insert one or more \[ddesend] contexts inside it for it to work. DDEConnect establishes a connection to the DDE server program, and provides an environment for the DDESend contexts to do their work and return text results.

**Example WebDNA code:**

```
[ddeconnect program=PCAuthorize&topic=GetBatches]
[ddesend]Tela00001031 1[/ddesend]
[/ddeconnect]
```

In this example, the DDE command "GetBatches from PCAuthorize" is executed, and the results (a listing of all the batch information for batch #1) is displayed. Notice that the whitespace between "Tela00001031" and "1" is actually a tab character, which is part of the PCAuthorize specification.

DDE has complete access to all DDE-aware programs on your web server. You can write a DDE context that can do damage to the machine or retrieve secret information. However, remote visitors to your web site have no way of executing their own DDE contexts remotely. They can only execute DDE commands inside a template file you have saved on your web server's hard disk.

\


| Parameter | Description                                                                                                                                                                    |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| program   | (Required) Name of the DDE server program to connect to, such as PCAuthorize                                                                                                   |
| topic     | (Required) Name of the DDE topic on which to send messages, such as GetBatches. Defined by the DDE server program; refer to that program's documentation for more information. |

### \[ddesend] [context](https://docs.webdna.us/contexts-and-tags)

Sends text to a DDE server program on the local machine.

\[ddesend]Text of DDE command\[/ddesend]

To embed the results of a DDE command into one of your pages, put a \[DDEConnect] context into a template, and put \[ddesend] contexts inside of that. The DDESend steps contained inside the context execute, and any returned value displays in place of the context. Any \[xxx] tags inside the context are first substituted for their real values before the DDE executes.

DDESend must be inside a containing \[ddeconnect] context, otherwise it will not know which DDE server program to send its text to.

**Example WebDNA code:**

```
[ddeconnect program=PCAuthorize&topic=GetBatches]
[ddesend]Tela00001031 1[/ddesend]
[ddesend]Tela00001031 2[/ddesend]
[/ddeconnect]
```

In this example, the DDE command "GetBatches from PCAuthorize" is executed, and the results (a listing of all the batch information for batch #1 and batch #2) is displayed. Notice that the whitespace between "Tela00001031" and "1" is actually a tab character, which is part of the PCAuthorize specification.

DDE has complete access to all DDE-aware programs on your web server. You can write a DDE context that can do damage to the machine or retrieve secret information. However, remote visitors to your web site have no way of executing their own DDE contexts remotely. They can only execute DDE commands inside a template file you have saved on your web server's hard disk.

### \[elapsedtime] [tag](https://docs.webdna.us/contexts-and-tags)

Putting \[elapsedtime] in your template displays the elapsed time (in 60ths of a second) since the beginning of processing this page. Put one at the top of a page, and another at the bottom to see how long the entire page takes to process (subtract the 2 numbers to get the answer).

### \[flushcache] [tag](https://docs.webdna.us/contexts-and-tags)

Putting \[flushcache] in your template causes all 'memorized' pages in memory to be immediately removed. Newer versions of any HTML or template files that have been modified on disk will be used the next time a browser links to those pages.

### \[freememory] [tag](https://docs.webdna.us/contexts-and-tags)

Putting \[freememory] in your template displays the amount of memory available to WebDNA. This number is reduced whenever templates are cached, or databases are opened.

This tag will not work on the OS X or FreeBSD platforms.

### \[interpret] [context](https://docs.webdna.us/contexts-and-tags)

Interprets the enclosed \[xxx] values using the WebDNA interpreter.

\[interpret]Any Text and \[xxx]\[/interpret]

To interpret \[xxx] values stored in a database field, enclose them in an \[interpret] context. For example, if you put the text "\[date]" into a field in a database (text1, for instance), then when you display \[text1] in a template it only shows the literal text "\[date]" -- it will not try to interpret the contents of the field. \[interpret]\[text1]\[/interpret], however, first substitutes "\[date]" for text1, then interprets the result as "1/29/10."

**Example WebDNA code:**

\[interpret]Some Text that contains \[xxx] fields\[/interpret]\
Any WebDNA \[xxx] tags are first substituted for their real values, which may contain more \[xxx] tags. Then the resulting text and tags are interpreted as though they were typed into the template directly.

Why? A good example is a banner advertisement database: let's say you want to create a list of advertisements to display randomly at the top of every page in your site. This works well if the ads do not contain any \[xxx] tags. But if you want to put \[cart] information into the banner ad, you must use the \[interpret] context to cause the \[cart] variable to be substituted for its real value.

```
-----BannerAds.db------
AdNumber    BannerHTML
1           <a href="Invoice.tpl?cart=[cart]">Invoice</a>
2           <img src="[random].gif">
-----------------------
```

Now let's look at a sample template that incorrectly uses the database above to randomly choose 1 item from the list and display the results at the top of a page:

```
<html><body>
[search db=BannerAds.db&neAdNumberdata=0&max=1&AdNumbersdir=ra&AdNumbersort=1]
[founditems][BannerHTML][/founditems]
[/search]
</body></html>
```

The problem here is that the \[BannerHTML] is indeed evaluated and displayed, but it is incorrect because the \[random] tag inside the field has not been evaluated to its true value:

```
<img src="[random].gif">
```

Now let's look at a sample template that correctly uses the database above to randomly choose 1 item from the list and display the results at the top of a page:

```
<html><body>
[search db=BannerAds.db&neAdNumberdata=0&max=1&AdNumbersdir=ra&AdNumbersort=1]
[founditems][interpret][BannerHTML][/interpret][/founditems]
[/search]
</body></html>
```

The results here are what we expected:

```
<img src="47.gif">
```

### \[object] WebDNA v4.0+ [context](https://docs.webdna.us/contexts-and-tags)

Embeds the results of an external function

\[object parameters]Main Parameter\[/object]

Executes the external Windows ActiveX control or Java class file, and displays the text of the result.

To embed the results of an external function (which could be a Windows ActiveX DLL or Java class file on any platform) into one of your pages, put an Object context with appropriate parameters into a template. The parameters are sent to the external module, and the result of the external call are displayed as text in place of the context. Any \[xxx] tags inside the context are first substituted for their real values before the Object is executed.

**Example WebDNA code:**

```
[object objname=MS.CUSTOM&[!]
[/!]call=MyFunction&[!]
[/!]param1=Hello&[!]
[/!]param1type=text&[!]
[/!]param2=2000&[!]
[/!]param2type=num] 
[/object]
```

The ActiveX control DLL "MS.CUSTOM" will be loaded and "MyFunction" will be executed with the following parameters:\
Parameter 1: Hello Type: text\
Parameter 2: 2000 Type: num

| Parameter                     | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| ----------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| objname                       | <p>ActiveX: The name of the ActiveX control<br>Java: The name of the Java class file</p>                                                                                                                                                                                                                                                                                                                                                                                                                              |
| <p><br>call</p>               | <p>ActiveX: Name of function to call.<br>Java: Name of the method to call(NOTE: the method must be be<br>able to receive "java/lang/String" and return<br>"java/lang/String")</p>                                                                                                                                                                                                                                                                                                                                     |
| type                          | <p>(Optional) The type of module to execute.<br>0 - ActiveX (Default)<br>1 ­ Java .class file</p>                                                                                                                                                                                                                                                                                                                                                                                                                     |
| classpath                     | <p>(Required for Java) Location of the .class file and all the<br>supporting .jar files.Can contain multiple locations separated<br>by semicolons ";".Only java modules require this<br>parameter.<strong>Note:</strong> This is only used on Mac OS platforms and is ignored in all the other platforms. If you want to set the classpath for the other platforms, you have to manually change the JavaClassPath preference in the "WebCatalog Prefs" file, but make sure you shutdown WebDNA before doing this.</p> |
| <p><br>Param1</p>             | <p>(Optional) Value of the first parameter to be passed into<br>the ActiveX or Java function.<br>For boolean values, use 0 for FALSE and 1 for TRUE</p>                                                                                                                                                                                                                                                                                                                                                               |
| <p><br>Param1Type</p>         | <p>(Required for each parameter) Data type of the first parameter.<br>Choose from <strong>bool</strong>, <strong>num</strong>, or <strong>text</strong></p>                                                                                                                                                                                                                                                                                                                                                           |
| Param2                        | <p>(Optional) Value of the second parameter to be passed into<br>the ActiveX or Java function</p>                                                                                                                                                                                                                                                                                                                                                                                                                     |
| Param2Type                    | (Required for each parameter) Data type of the second parameter                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| <p><br>...Param<em>N</em></p> | <p>(Optional) As many parameters as are necessary may be passed<br>into the ActiveX/Java function</p>                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ...Param_N_Type               | <p>(Required for each parameter) For each value passed, you<br>must define its data type</p>                                                                                                                                                                                                                                                                                                                                                                                                                          |

### \[platform] [tag](https://docs.webdna.us/contexts-and-tags)

Putting \[platform] in your template displays the computer platform (Windows or Macintosh or Unix) that WebDNA is running on.

### \[rawpost] WebDNA v8.6.5 [tag](https://docs.webdna.us/contexts-and-tags)

\[rawpost] displays contents of the HTTP POST data as sent to the web server, the value will be empty if no POST data was sent.

### \[redirect] [tag](https://docs.webdna.us/contexts-and-tags)

\[redirect url]

Putting \[redirect url=http://www.webdna.us/] in your template forces the remote browser to **immediately** 'jump' to the new location specified, **before** displaying anything in the template ofter the \[redirect url], other text in the template will be ignored.

Using this method will completely evaluate before anything of the client side will evaluate:\
`<META HTTP-EQUIV="Refresh" CONTENT="0;URL=http://www.webdna.us/">`

This will perform browser authentication, on the server, since you're running WebDNA, you can use \[username] and \[password] to check for logged in status (as opposed to setting up a cookie scheme).\
`[redirect http://DomainUser:DomainPass@www.domain.com/whatever.html]`

To pass a value within \[redirect]\
\[redirect page.dna?var1=\[value1]\&var2=\[value2]]

In some cases, you will need to \[url]...\[/url] the value:

\[redirect page.dna\[url]?var1=\[value1]\&var2=\[value2]\[/ur]]

\


### \[permredirect] WebDNA v8.6.4+ [tag](https://docs.webdna.us/contexts-and-tags)

### \[return] WebDNA v5.0+ [context](https://docs.webdna.us/contexts-and-tags)

Explicitly identify what text is returned from a function call

\[return]

\[/return]

The textual output generated as a result of a WebDNA function call includes whatever text remains after the function code is executed. This may include unwanted spaces, carriage returns, and other 'white space' characters. The \[return] context can be used to explicitly identify what text is returned from the function call, thereby avoiding unwanted characters.

The \[return] context is optional and can only be used from within the \[function] context. The \[return] context does NOT 'break out' of a function call, so it is possible to use one or more \[return] contexts to 'tailor' the functions output.

**Example without \[return]**

Below is a simple function that does not include a \[return] context. This function simply adds the first ten positive numbers. We will execute the function, then wrap the execution in \[url]\[/url] tags to 'reveal' the extra white space that can accumulate from a function call (much as it would when using the WebDNA \[include] tag.)

Here is the code:

```
[function name=add_em_up]
[text]result=0[/text]
[loop start=1&end=10]
[text]result=[math][result]+[index][/math][/text]
[/loop]
[result]
[/function]
```

\
Executing the function, we get: " 55 " (note the extra spaces)

Now, lets 'wrap' the function result with the \[url] context to uncover the 'extra' stuff we accumulated a result of the function call.\
Here is the result:

"%0D%0A%0D%0A%0D%0A%0D%0A%0D%0A%0D%0A%0D%0A%0D%0A%0D%0A\
%0D%0A%0D%0A%0D%0A%0D%0A%0D%0A%0D%0A%0D%0A%0D%0A%0D%0A\
%0D%0A%0D%0A%0D%0A%0D%0A%0D%0A55%0D%0A"

Note all the extra white space, in this case, carriage returns and line feeds.

**The 'old' Solution**

One way that WebDNA programmers have dealt with unwanted return characters, is to wrap line-endings, or other unwanted white space, with WebDNA comments, i.e. \[!]...\[/!]. So the function definition on the previous page would look like...

```
[function name=add_em_up][!]
[/!][text]result=0[/text][!]
[/!][loop start=1&end=10][!]
[/!][text]result=[math][result]+[index][/math][/text][!]
[/!][/loop][!]
[/!][result][!]
[/!][/function]
```

\
Executing the above function, and wrapping the result with URL tags, we get: "55"

The extra 'garbage' is gone, but using all those \[!]\[/!] pairs is cumbersome, and does add some extra parsing overhead.

**A Better Solution**

The \[return] context can now be used to target exactly what we want the function to return. So our example function now looks like...

```
[function name=add_em_up]
[text]result=0[/text]
[loop start=1&end=10]
[text]result=[math][result]+[index][/math][/text]
[/loop]
[return][result][/return]
[/function]"[url][add_em_up][/url]"
```

\
Executing the above code, we get: "55"

The extra 'garbage' is gone, and we did not have to use all those \[!]\[/!] contexts.

Even if the explicit results of a function call are not significant, for example, when the function assigns the result to some global text variable. It is still a good idea to use the \[return] context in order to cut down on the amount of white space that my be returned to the client browser.

**For example:**

```
[function name=add_em_up]
[text]result=0[/text]
[loop start=1&end=10]
[text]result=[math][result]+[index][/math][/text]
[/loop]
[text scope=global]result=[result][/text]
[return][/return] [!] return nothing [/!]
[/function][add_em_up]
result="[result]"
```

\
Executing the above code, we get:

result="55"

As mentioned in the first page of this tutorial, the \[return] context does not actually 'return' or 'break out' of the function call. So, it is possible to have multiple \[return] contexts in a given function definition. For example:

```
[function name=add_em_up]
[text]result=0[/text]
[loop start=1&end=10]
[text]result=[math][result]+[index][/math][/text]
[showif [index]
```

\
Results in...\
"1+2+3+4+5+6+7+8+9+10=55"

The \[return] context is also very useful when creating 'recursive' functions (functions that call them selves until a terminating 'base case' is reached).

Here is a sample recursive function that calculates the factorial for a given integer.

```
[function name=factorial]
[showif [num]>1]
[return][math][num]*[factorial num=[math][num]-1[/math]][/math][/return]
[/showif]
[hideif [num]>1]
[return]1[/return]
[/hideif]
[/function]6! = [factorial num=6]
```

\
The results...

6! = 720

### \[scope] WebDNA v5.0+ [context](https://docs.webdna.us/contexts-and-tags)

\[scope] explicitly defines a block of WebDNA code that has a separate variable space.\
Meaning that variables defined within a \[scope], only exist for the duration of WebDNA within that \[scope] context.

```
[scope name=scopeTest]
[text]variable1=abc[/text]
The value of variable1 = [variable1]
[/scope]The value of variable1 = [variable1]
```

**The result of the above example will return:**\
The value of variable1 = abc\
The value of variable1 = \[variable1]\
variable1 remains 'raw' because the text variable "variable1" is not available outside the scope.

**Parameters**

| Parameter | Description                                                                                                                                                                                                     |
| --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| name      | (optional) The name assigned to the scope. This name can be used with the scope resolution operator to 'access' variables stored in the 'named' variables space but only for the duration of the scope context. |

**Example WebDNA code:**

```
[scope name=mytempvars]
    [text]a=11[/text]
    [text]b=22[/text]
    [text]c=33[/text]    Access the variables inside the scope:
    [listvariables scope=mytempvars][name]=[value]
    [/listvariables]
[/scope]Access the variables outside the scope:
a = [a]
b = [b]
c = [c]Access the variables outside the scope using the scope name:
[listvariables scope=mytempvars][name]=[value]
[/listvariables]
```

**Results:**

```
Access the variables inside the scope:
a=11
b=22
c=33Access the variables outside the scope:
a = [a]
b = [b]
c = [c]Access the variables outside the scope using the scope name:
a=11
b=22
c=33
```

The 'local' scope variables; 'a','b','c', only exist between the \[scope] tags or when called using the scope name.

This is useful when you need to create several temporary variables for a specific block of WebDNA code, but do not want the variables 'cluttering' the global template variable space.

**Scope and Functions:**\
WebDNA functions have their own implied scope. Meaning that when variables are created inside of a function definition, the variables are local to that function. The 'name' of the variable space in the function, is the function name itself.

**Example WebDNA code:**

```
[function name=test_function]
   [loop start=1&end=10]
      [text]local_[index]=[index][/text]
   [/loop]
   [listvariables scope=test_function][name]=[value]
   [/listvariables]
[/function][test_function]
```

**Results:**\
local\_1=1\
local\_2=2\
local\_3=3\
local\_4=4\
local\_5=5\
local\_6=6\
local\_7=7\
local\_8=8\
local\_9=9\
local\_10=10\
_By default, text variables created within a function are discarded after the function has finished._

**Text Variables with Global Scope**\
A text variable with scope set to global will exist outside the function or scope.

**Example WebDNA code:**

```
[function name=myfunction]
   [text scope=global]var1=foo[/text]
   [text]var2=bar[/text]
   Anything entered here as text within the function will not display, 
   if you want it to display on the rendered page then put it inside a [return] context:
   [return]This text will display on the rendered page:[/return]
[/function][myfunction] produces [var1] and [var2]
```

**Result:**\
\[myfunction] \[var1] and \[var2] will render as:\
This text will display on the rendered page: foo and bar

**Reserved Scope Names:**\
WebDNA has a number of 'reserved' scope names:\
**"global"** : Refers to the 'normal/secure' template variable space.\
**"local"** : When used inside of a function or scope context, refers to the variable space associated with the current function or scope.\
**"insecure"** : Refers to the 'insecure' template variable space (this space also includes HTML form variables).

### \[spawn] WebDNA v3.0+ [context](https://docs.webdna.us/contexts-and-tags)

Creates a new thread to execute WebDNA simultaneously with the current template.

\[spawn]Some Complex WebDNA\[/spawn]

To perform several WebDNA actions simultaneously, place them inside a Spawn context. All WebDNA inside the Spawn context begins to execute immediately, and the remainder of the template is returned to the visiting browser immediately.

The HTML output from within a Spawn context is never displayed to the browser. While this may seem unhelpful at first, realize that the purpose of Spawn is to allow you to execute very lengthy operations without forcing the visitor to wait for them. The WebDNA in the spawned context could update a database several minutes later, wait for a 15 second credit card operation, create a WebDelivery file, flush a database to disk or many other useful things.

Not all WebDNA information is available inside a \[spawn] context, because often the outer template that created the spawn is already gone from memory, and thus its context information is gone as well. Only the following values are available inside the spawn, because a copy of them is made before creating the spawn context:

\- MIME headers from the browser.\
\- Cookies from the browser.\
\- Form variables (or URL parameters from HREF hyperlink).\
\- Math variables created in this template.\
\- Text variables created in this template.

**Example WebDNA code:**

```
Before the spawn [elapsedtime][spawn]
-- Some WebDNA that takes a long time to finish
[loop start=1&end=5000][showif 1=1][/showif][/loop]
[/spawn]
After the spawn [elapsedtime]
```

The example above yields:

Before the spawn 1\
After the spawn 3

the elapsedtime is very small, even though the loop inside the spawn could take several seconds. This is because your web browser sees the results of the template before the spawned WebDNA is finished.

Here are some common **mistakes** you should avoid:

\


| Incorrect                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Correct                                                                                                                                                                              |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| <pre><code>[loop start=1&#x26;end=10]
[spawn]
[showif [index]=5]
stuff
[/showif]
[/spawn]
[/loop]
</code></pre>                                                                                                                                                                                                                                                                                                                                                                                                 | <pre><code>[loop start=1&#x26;end=10]
<strong>[Math]loopindex=[index][/Math]
</strong>[spawn]
<strong>[showif [loopindex]=5]
</strong>stuff
[/showif]
[/spawn]
[/loop]
</code></pre> |
| Why                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |                                                                                                                                                                                      |
| Remember spawn might start executing long **after** the original template that was created has gone away. spawn has no idea what the value of \[index] is, because that comes from the outer \[loop] context, which really 'belongs' to the now-gone exterior template. The correct method is to create a math variable to hold the \[index] value, because spawn **does** keep a copy of all the math variables in existence when it was created.                                                              |                                                                                                                                                                                      |
| Incorrect                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Correct                                                                                                                                                                              |
| <pre><code>[search blah]
[founditems]
[spawn]
[FieldFromDB]
[/spawn]
[/founditems]
[/search]
</code></pre>                                                                                                                                                                                                                                                                                                                                                                                                      | <pre><code><strong>[search blah&#x26;max=10]
</strong>[founditems]
<strong>[Text]value=[FieldFromDB][/Text]
</strong>[spawn]
[value]
[/spawn]
[/founditems]
[/search]
</code></pre>  |
| Why                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |                                                                                                                                                                                      |
| This is bad for two reasons. Similar to the first example, spawn has no idea what the database field values are, because it is not truly inside the \[founditems] context. Second, be very careful you do no create too many spawns -- they can use a lot of memory, and in this case if the \[founditems] is more than a dozen or so; web server performance can degrade considerably. The correct example limits the number of spawns, and also uses a text variable to hold the value of the database field. |                                                                                                                                                                                      |

Nothing within a SPAWN context will be delivered to the browser, nor will any HTML within the SPAWN be interpreted. SPAWN is intended to run time-consuming server-side tasks (like large database updates) without making the browser wait for it to complete.

**Example WebDNA code:**\
The following code send one email every 30 seconds. If you're only sending 500 emails occasionally this will send them in less than 5 hours.

```
[spawn]
[search db=xxx.db&neemaildatarq=[blank]]
[founditems]
[text]wffSuccess=no[/text]
[waitforfile thisFileWillNeverExist.txt][/waitforfile]
[showif [wffSuccess]=no]
[sendmail from=xxx@xxx.xxx&to=[email]&subject=not spam]
Blah blah blah ...
[/sendmail]
[text]wffSuccess=yes[/text]
[/showif] 
[/founditems]
[/search]
[/spawn]
```

\


### \[store] WebDNA v8.1+ [context](https://docs.webdna.us/contexts-and-tags)

\[store] is a simple context that stores variables permanently

The process would use \[store] and \[recall] the name of the variable the variable itself

The variable and the name are as long as requested: it save the hassle of writing the code to store those in a database. However, to avoid "overheating", we can have only one name per variable, meaning that if we store name3 as "pink", and later store name3 as "black", then only the last value will remain and "pink" will be overwritten by "black"

**Example WebDNA code:**\
\[store]var1=Joe\[/store]\
The data is recorded inside a database "reserved.db" in /WebDNA (FastCGI version), in /globals/ (server version, no sandboxing) or in /SandBoxes/domain.com/Globals/ (server version with sandboxing), one column for the variable name and the other one for the string.

If reserved.db does not exist, it will be created automatically and transparently.

| Parameter | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| show      | <p>(optional) "T" or "F". Default behavior is to hide the value when assigning to a variable. If we want the value to be shown at the same time it is assigned to a variable, we may set Show=T (There is no reason to ever use show=f, as this is default behavior.)<br><strong>Example WebDNA code:</strong><br>[store show=T]var1=Joe[/store]</p>                                                                                                                                                                                                             |
| multi     | <p>(optional) "T" or "F". Allows to assign more than one text variable in a single context.<br><strong>Example WebDNA code:</strong><br>[store multi=T]var1=Joe&#x26;var2=Fred[/store] simultaneously assigns the two variables. (There is no need to use multi=f for single variables.)</p>                                                                                                                                                                                                                                                                     |
| path      | <p>(optional) path of the database, if a user wants to use another database than the default one.<br><strong>Example WebDNA code:</strong><br>[store path=../../specific.db]var1=Joe[/store]<br>Look for "specific.db" two level up</p>                                                                                                                                                                                                                                                                                                                          |
| parse     | <p>(optional) "T" or "F". Interpret o no the tags that would be in the data to be stored.<br><strong>Example WebDNA code:</strong><br></p><pre><code>[store parse=T]var1=Today is [date][/store]
</code></pre><p><br>would give<br></p><pre><code>
var1 - Today is 01/16/2015
var2 - Fred
</code></pre><p><br><strong>Example WebDNA code:</strong><br></p><pre><code>[store parse=F]var1=Today is [date][/store]
</code></pre><p><br>would give<br></p><pre><code>
var1 - Today is [date]
var2 - Fred
</code></pre><p><br>The defaut behavior being parse=F</p> |

To recover your data, you can use either

```
[recall var1] and get the tag replaced by the variable.
[recall var1&path=../../specific.db] or [recall path=../../specific.db&var1]
```

The data is usable anytime, anywhere: you can store data with one browser and someone else can recall it with another browser.

You can also use \[convertwords] \[/convertwords]

```
[convertwords path=../../specific.db]
var2 says var1
[/convertwords]
```

Fred says Today is 04/17/2015

### \[version] [tag](https://docs.webdna.us/contexts-and-tags)

Putting \[version] in your template will display the version of the currently-running WebDNA program or plugin.

### \[waitforfile] WebDNA v3.0+ [context](https://docs.webdna.us/contexts-and-tags)

The server waits until the specified file appears on disk, then executes the WebDNA inside the context.

```
[waitforfile file=newFile.txt]
        Some WebDNA to parse once newFile.txt is present
[/waitforfile]
```

To perform an action as soon as a file appears on disk, put WebDNA inside of a \[waitforfile] context. The server waits until the specified file appears on disk, then executes the WebDNA inside the context.

**Parameters**

| Parameter | Description                                                                                                                                                                  |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| file      | (Required) Path to the file that will appear in the future. If the file exists on disk already, there is no waiting.                                                         |
| timeout   | (Optional) Number of seconds to wait before cancelling the operation. No WebDNA inside the \[waitforfile] context will be executed. Defaults to 30 seconds if not specified. |

WaitForFile will wait a specified number of seconds (default 30) until the file appears. The template will not finish executing and the HTML will not be returned to the visiting browser until the WaitForFile finishes.

You can write WebDNA that learns if the WaitForFile timed out:

**Example WebDNA code:**

```
[text]succeeded=F[/text]
[waitforfile file=FileToWaitFor.txt]
Some WebDNA to execute if the file is found
[text]succeeded=T[/text]
[/waitforfile]
[showif [succeeded]=T] File was found! [/showif]
[showif [succeeded]=F] File was not found! [/showif]
```

See also \[wait]

### \[wait] WebDNA v8.6+ [tag](https://docs.webdna.us/contexts-and-tags)

\[wait] just does what it says

**Parameters**

| Parameter         | Description                                          |
| ----------------- | ---------------------------------------------------- |
| a numerical value | The number of milliseconds to wait. 3000 = 3 seconds |

When executed on a thread, it will only affect the one thread

### \[webserver] [tag](https://docs.webdna.us/contexts-and-tags)

### \[shell] WebDNA v3.0+ [context](https://docs.webdna.us/contexts-and-tags)

\[shell] is a way to use the command line with your webserver. This allows you to tap into other applications on your server.

\[shell]command line commands here\[/shell]

Using shell, you can manipulate images with Imagemagick, zip and unzip files, check directory listings, change permissions, create symlinks... the list goes on.

Since this is, after all, the command line, it is also very powerful, and not to be taken casually. For this reason, in a Sandbox environment where the server does not belong to you, shell scripts are handled a little differently. You need to submit your script to the administrator first. If it is approved as safe, the admin will enter the script into a sandbox database and give you the ID of the record. Then you run the script via its ID value, with nothing between the tags.

**ImageMagick Example:**\
(assuming you have set the text variables already)

<pre><code><strong>[shell]/sw/bin/convert -size [maxwidth]x[maxheight] uploads/[tempfile] -scale [maxwidth]x[maxheight] uploads/[thisname] -format jpg /images/[thisname][/shell]
</strong></code></pre>

This script calls the 'convert' portion of the ImageMagick program, resizes the file in the uploads folder, and writes a new image in .jpg format to the images folder.

In a Sandbox environment, you would give the above information to your administrator, and they'd set up a script so you'd use simply:

```
[shell id=uniqueID][/shell]
```

Something specific like the above is fine if this is all you do with images. However, if you are adventuresome and want more control, you can submit this:

```
[shell]/sw/bin/convert [IMstuff][/shell]
```

and call it like this:

```
[text]IMstuff=-size [maxwidth]x[maxheight] uploads/[tempfile] -scale [maxwidth]x[maxheight] uploads/[thisname] -format jpg /images/[thisname][/text]
[shell id=uniqueID][/shell]
```

Now you can do whatever creative or specific operations you need, using only one sandbox shell script, and changing the \[IMstuff] variable as needed.

**ZIP example:**

<pre><code>[text]zippath=your/path/to/folder[/text]
<strong>[shell]zip -rq [zippath] [zippath][/shell]
</strong></code></pre>

This takes the specified folder and creates your/path/to/folder.zip

Find out more about the commands for ImageMagick, zip, and other server applications by searching the web.

```
[shell]ls -l[/shell]
```

In this example, the shell command "ls -l" is executed, and the results (a listing of all the files in the current directory) is displayed. The user privileges are the same as the WebDNA program itself (which is typically logged on as user nobody).

Shell has access to your hard disk and programs on your web server, depending on the permissions your administrator gave you. However, remote visitors to your web site have no way of executing their own Shell contexts remotely. They can only execute shell commands inside a template file that you have saved on your web server's hard disk.

### \[DOS] [context](https://docs.webdna.us/contexts-and-tags)

Executes the DOS batch file commands contained in the context and displays the results.

\[DOS]DOS Batch Commands\[/DOS]

To embed the results of a DOS Batch file into one of your pages, put a DOS context into a template. The DOS program contained inside the context is executed, and any returned value is displayed in place of the context. Any \[xxx] tags inside the context are substituted first for their real values before the batch file is executed.

**Example WebDNA code:**

```
[DOS]dir c:[/DOS]
```

In this example, the DOS command "dir c:" is executed, and the results (a listing of all the files at the root of C: drive) is displayed.

Caution! DOS has complete access to your hard disk and all programs on your web server. You can write a DOS context to erase the contents of your hard disk. However, remote visitors to your web site have no way of executing their own DOS contexts remotely. They can only execute DOS commands inside a template file that you have saved on your web server's hard disk.

### \[sendmail] [context](https://docs.webdna.us/contexts-and-tags)

There is hardly a website that doesn't at some point need to send an email, whether through a simple contact form, or as notifications to the buyer and seller when somebody buys something. WebDNA makes this simple and straightforward.

\[sendmail _Parameters_]Body\[/sendmail]

To send an email, use a SendMail context with the body of the email message inside. Specify "to," "subject," and "from" information in parameters of the SendMail context. WebDNA does not actually send the email; it writes a special file into the EmailFolder which the separate Emailer program scans nearly continuously, looking for mail to send. If the Emailer program is not running, the emails will sit there unsent. The Emailer program does not erase old outgoing email files until it has successfully completed sending the email. Then it moves them to the SentMail folder. If any of the emails are malformed and can't be sent, they are moved to the ProblemEmail folder.

```
[sendmail to=you@xxx.com&from=me@xxx.com&subject=Hello]
This is the body of the email.
The date is [date][/sendmail]
```

Any WebDNA inside the \[sendmail] is interpreted first, such as the \[date] tag above. HTML is not relevant here. The body of the \[sendmail] is written to a file just as it is coded on your page, returns, running spaces and all. Do not use paragraph tags or table code or any other html. If you insert a URL, just write the URL, including the http:// portion without the \<a> tags. Most email software will recognize it as a URL and make it clickable.

| **Parameter** | **Description**                                                                                                                                                                                                                                      |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| to            | recipient(s). Can be comma separated as in address@domain.com,address2@domain2.com                                                                                                                                                                   |
| from          | sender's email address, which should be a legitimate address allowed to send mail through your mail server                                                                                                                                           |
| reply-to      | (Optional) Address the recipient's email client will insert when they reply to the mail. This can be any email address, and is useful for assuring that the recipients reply to the right person, since you must use a local address for the sender. |
| subject       | the subject line                                                                                                                                                                                                                                     |
| any header    | (Optional) CC, BCC, or any extra MIME header information. For example, if you want to force the date of the email to something other than the server's default, add "Date=Mon, 05 Sep 2008 15:59:33 -0500" to the list of parameters.                |
| saveonsuccess | T or F (Optional) instructs WebDNA to save or not save the email files after successful transmission                                                                                                                                                 |
| saveonfail    | T or F (Optional) instructs WebDNA to save or not save the email files after failed transmission.                                                                                                                                                    |

WebDNA does not actually send the email; it writes a special file into an EmailFolder which the separate Emailer program uses to send the email. If the Emailer program is not running, no emails will be sent. The Emailer program does not erase old outgoing email files until it has successfully completed sending the email.

The email file format conforms to Unix SendMail native mail format. You can add any extra MIME headers at the top of the file, followed by a blank line and then the body of the message. The format is basically what you see in a Eudora email message.\
If you wish to send emails from your own CGIs or programs (without using WebDNA's built-in \[sendmail] context), you can write a text file into the EMailFolder specified in the EMailer preferences. The file must have the following format (only the text between the dashed lines goes into the email file):

An example:

```
------- /WebCatalogEngine/EMailFolder/SomeFileName.txt -----
to: support@webdna.us, WebDNA@webdna.us
from: WebMaster@YourDomain.com
subject: This is the Subject Line
Date: Mon, 05 Jan 2010 15:59:33 -0500line 1
line 2
line 3
line 4
------------------------------------------------------
```

**Sendmail and binary files**\
You can also send binary files (zip files, images...) using \[sendmail].

**Example WebDNA code:**

```
[sendmail to=whoever@whereever.com&from=whoever@whereever.com&subject=testing zip attachment&Content-Type=multipart/mixed; boundary="1234abcd"&MIME-version=1.0][!]
[/!]--1234abcd
Content-Type: text/plainThis is the first part : just plain text.The next part contains a zip file.--1234abcd
Content-Type: application/x-zip-compressed; name="test.zip"
Content-Transfer-Encoding: base64
Content-Disposition: inline; filename="test.zip"[encrypt method=base64&file=test.zip&EMAILFORMAT=T][/encrypt]--1234abcd--
[/sendmail]
```

**Mailing a PDF** (by Stuart Tremain)\
Send a very simple email with a .pdf attached

```
[SENDMAIL to=recipient@webdna.us&from=sender@webdna.us&SUBJECT=Email with attachment&Content-Type=multipart/mixed; boundary="1234abcd"&MIME-version=1.0]--1234abcd
Content-Type: text/plainThe attached PDF file will open easily into Acrobat.Go here to get Adobe Acrobat: http://www.adobe.com/products/acrobat/readstep2.html--1234abcd
Content-Type: text/plain; name="[_DocNo].pdf"
Content-Transfer-Encoding: base64
Content-Disposition: attachment; filename="[_DocNo].pdf"[ENCRYPT method=base64&file=/orderdocs/[_DocNo].pdf&EMAILFORMAT=T][/ENCRYPT]--1234abcd--
[/SENDMAIL]
```

**Mailing using html encoding** (by Stuart Tremain)

```
[Sendmail to=[EMAILTO]&from=[EMAILFROM]&subject=[EMAILSUBJECT]&MIME-Version=1.0&Content-Type=multipart/alternative; charset=utf-8; boundary="this_is_the_boundary"&Content-Transfer-Encoding=7bit]--this_is_the_boundary
Content-Type: text/plain; charset=us-ascii
Content-Transfer-Encoding: 7bit[unurl]%0D%0A[/unurl]Dear [Capitalize][CUST_FIRSTNAME][/Capitalize]We have added your details to the express checkout system.If you would like to change or delete your credit card, please login as a member and enter the details in Payment Options.
https://[ListMIMEHeaders name=HOST][value][/ListMIMEHeaders]/membershipThank you for supporting original designIf you have any questions, please feel free to respond to this email or call 123456789 during business hours.Warm regards,
membership co-ordinatorYou can read our terms and conditions at any time:
http://[ListMIMEHeaders name=HOST][value][/ListMIMEHeaders]/terms-and-conditions[unurl]%0D%0A[/unurl]--this_is_the_boundary
Content-Type: text/html; charset=us-ascii
Content-Transfer-Encoding: 7bit[unurl]%0D%0A[/unurl]<!DOCTYPE html>
<html lang="en">
<head><meta charset="utf-8" />
<style>
body {margin:0;padding:20px 0px 20px 0px;}
table {width:600px;border-spacing:0;border-collapse:separate;padding:0;margin:0px auto;background:white;border:0;}
table.emailer {border:1px solid #666666;}
td.emailhead {height:138px;background:url(http://mydomain.com/emailer/emailer-head.png) no-repeat 0 0;padding:0;border:0;vertical-align:top;}
td.emailpromo {height:100px;padding:0px 0px 15px 0px;border:0px;}
td.emailpromotext {padding:15px;border-bottom:1px solid #666666;}
td.emailbody {color:#222222;font:12px/18px Arial, Helvetica, sans-serif;padding:0px 30px 30px 30px;}
td.emailfooter {color:#222222;font:12px/18px Arial, Helvetica, sans-serif;padding:10px 0px 20px 30px;}
td.emailfooter a {color:#222222;font:12px/18px Arial, Helvetica, sans-serif;text-decoration:none;}
td.baseicon {width:19px;vertical-align:top;padding:6px 0px 0px 30px;}
td.basetext {color:#222222;font:12px/18px Arial, Helvetica, sans-serif;padding:6px 0px 0px 12px;vertical-align:top;}
td.basetext a {color:#49A3FD;font:12px/18px Arial, Helvetica, sans-serif;text-decoration:none;}
img {border:0px;}
table.social {width:136px;}
table.social tr td {width:29px;height:29px;padding:10px 5px 0px 0px;}
table.inner {width:540px;}
</style>
</head>
<body>PUT ALL YOUR HTML HERE</body>
</html>--this_is_the_boundary--[/sendmail]
```

\
