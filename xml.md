# XML

### \[xmlparse] WebDNA v5.0+ [context](https://docs.webdna.us/contexts-and-tags)

\[xmlparse] enables the input of XML data into a WebDNA variable, after which any part of the XML structure can be readily examined using the WebDNA XML contexts. \[xmlparse] is the first step in interpreting XML so that its contents can be used within WebDNA.

**Parameters**

| Parameter | Description                                                                                                                                  |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| var       | The variable name specified to represent the XML parsed object                                                                               |
| source    | A URL reference to an external XML file. If 'source' is provided, the content within the \[xmlparse...] and \[/xmlparse] context is ignored. |

**Example WebDNA code:**\
This example uses an example file that you can view/download here [example-file.xml](https://docs.webdna.us/files/example-file.xml)

```
[xmlparse var=xml_var1][include file=example-file.xml][/xmlparse]
```

Internally, WebDNA 'saves' this parsed instance as an internal variable named 'xml\_var1'. This saved instance can then be referenced in the other WebDNA XML contexts on the same page.

In the example, the \[include] tag is used to place the xml file contents within the \[xmlparse] context. The complete XML data can be used in place of an included file.

If the source url in the \[xmlparse] context contains an ampersand then it must be url'd \[url]...\[/url] so that any '&' in the query portion of the URL is not interpreted as a WebDNA parameter delimiter.

**Example WebDNA code:**

```
[xmlparse var=xml_var1&source=[url]http://www.domain.com?parameter1=a&parameter2=b[/url]][/xmlparse]
```

\


### \[xmlnodes] WebDNA v5.0+ [context](https://docs.webdna.us/contexts-and-tags)

The \[xmlnodes] context is to iterate the child XML nodes of a parent node. The location of the parent node, in the xml 'tree', is determined by the 'path' parameter. If a path parameter is not provided, then the child nodes of the ref 's root are iterated.

**Parameters**

| Parameter | Description                                                                                                                                                                                                                                                                                                         |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ref       | Reference to an xml parsed object variable. If this parameter is not provided, then it is assumed that there is an 'outer' \[xmlnodes] context from which to reference a particular XML node. (This is explained further on in this section).                                                                       |
| path      | Depending on which path method is used (see below), this will either be a path to the parent xml element (node) from which to iterate the child elements (nodes), or a path 'expression' representing a collection of XML nodes. If a path is not provided, then the child nodes of the 'ref' node will be iterated |
| name      | A string used to filter the resulting xml nodes. Only the xml nodes that match the 'name' string will be iterated.                                                                                                                                                                                                  |
| exact     | Used with the 'name' parameter. Either 'T' or 'F'. Specifies whether the 'name' parameter is a 'whole' string match or a 'sub-string' match.                                                                                                                                                                        |

**Tags available within a \[xmlnodes] context:**

| Tag         | Description                                                                                                                 |
| ----------- | --------------------------------------------------------------------------------------------------------------------------- |
| index       | The 'count' of the current iterated node.                                                                                   |
| name        | The name of the current iterated XML node.                                                                                  |
| value       | The value of the current iterated XML node. Will be empty if the node is a 'container' node, i.e. contains other XML nodes. |
| numfound    | The total 'count' of iterated nodes.                                                                                        |
| content     | The 'raw' xml content of the current node.                                                                                  |
| iscontainer | 'T' if the current node contains other XML nodes.                                                                           |

The first step to obtaining the XML nodes is to first parse the XML content using the WebDNA \[xmlparse] context.

Obtaining the path parameter from the parsed XML can use three different methods: named, indexed, or xpath.

The named method expresses a literal path to the parent node, i.e. path=named:CATALOG/CD(n). If there are more than one similarly named 'sibling' nodes, then the '(n)' part specifies which node to select as part of the path.

The indexed method expresses a numerical "stepped" value path to the parent node, i.e. path=indexed:1/2/3. This example could be expressed as: _'The third child node of the second child node of the first child node of the xml root'_.

The xpath method is an XPath 'expression' that evaluates to a collection of nodes in the XML tree. In this case, the iterated nodes are those of the resulting 'collection' of nodes. This is a bit different from the 'named' and 'indexed' method in that the collection of node are not the 'child' nodes of a given 'parent' node. This is the most powerful method for selecting XML nodes. There are several online 'xpath' tutorials that you can visit that will help you develop your XPath skills.

Example WebDNA code:\
This example uses an example file that you can view/download here [example-file.xml](https://docs.webdna.us/files/example-file.xml)

```
[xmlparse var=xml_var1][include file=example-file.xml][/xmlparse]
[xmlnodes ref=xml_var1]
   [name] = [value]<br>
[/xmlnodes]
```

Results: This example finds the first node or the root of the XML document.\
CATALOG =\
The node named 'CATALOG' is the only child node from the root of the XML file. Notice that the 'value' is empty. This is because the 'CATALOG' node has no value, and is actually a 'container' node for other XML nodes. A 'value' will only be displayed for a 'leaf' XML node.

Expanding on the method of a named path to delve deeper into the file.\
**Example WebDNA code:**\
This example uses an example file that you can view/download here [example-file.xml](https://docs.webdna.us/files/example-file.xml)

```
[xmlparse var=xml_var1][include file=example-file.xml][/xmlparse]
[xmlnodes ref=xml_var1&path=named:Catalog]
  [name] =  [value]<br>
[/xmlnodes]
```

Results: All the names of the child nodes of the parent 'CATALOG' are displayed. Again, none of the resulting child nodes contain a value as they are all 'container' nodes.\
CD=\
CD=\
CD=\
CD=\
CD=

\[xmlnodes] contexts can be embedded within \[xmlnodes] contexts. This method will iterate the child nodes of all the 'CD' nodes of the example file.

**Example WebDNA code:**\
This example uses an example file that you can view/download here [example-file.xml](https://docs.webdna.us/files/example-file.xml)

```
[xmlparse var=xml_var1][include file=example-file.xml][/xmlparse]
[xmlnodes ref=xml_var1&path=named:Catalog]
  <b>[name] - [index]</b><br>
  [xmlnodes]
    [name] = [value]<br>
  [/xmlnodes]<br>
[/xmlnodes]
```

Results: (only the first 4 are displayed here, there are may more in the file) Note that the 'inner' xmlnodes context did not need a 'ref' parameter. This is because the inner xmlnodes context inherited the outer xmlnodes' current iterated node. Also notice that the inner xmlnodes context did not use a 'path' parameter. So it uses the 'root' path of the outer xmlnodes' current iterated node. As with most 'iterative' WebDNA contexts, the \[index] tag resolves to the current iteration 'count'.\
CD - 1\
&#x20; TITLE = Empire Burlesque\
&#x20; ARTIST = Bob Dylan\
&#x20; COUNTRY = USA\
&#x20; COMPANY = Columbia\
&#x20; PRICE = 10.90\
&#x20; YEAR = 1985\
CD - 2\
&#x20; TITLE = Hide your heart\
&#x20; ARTIST = Bonnie Tylor\
&#x20; COUNTRY = UK\
&#x20; COMPANY = CBS Records\
&#x20; PRICE = 9.90\
&#x20; YEAR = 1988\
CD - 3\
&#x20; TITLE = Greatest Hits\
&#x20; ARTIST = Dolly Parton\
&#x20; COUNTRY = USA\
&#x20; COMPANY = RCA\
&#x20; PRICE = 9.90\
&#x20; YEAR = 1982\
CD - 4\
&#x20; TITLE = Still got the blues\
&#x20; ARTIST = Gary More\
&#x20; COUNTRY = UK\
&#x20; COMPANY = Virgin records\
&#x20; PRICE = 10.20\
&#x20; YEAR = 1990

With a minor change to the 'path' parameter, we can retrieve all the child nodes of the fifth 'CD' node of example-file.xml, still using the named method.

**Example WebDNA code:**\
This example uses an example file that you can view/download here [example-file.xml](https://docs.webdna.us/files/example-file.xml)

```
[xmlparse var=xml_var1][include file=example-file.xml][/xmlparse]
[xmlnodes ref=xml_var1&path=named:Catalog/CD(5)]
  [name] = [value]
[/xmlnodes]
```

Results:\
TITLE = Eros\
ARTIST = Eros Ramazzotti\
COUNTRY = EU\
COMPANY = BMG\
PRICE = 9.90\
YEAR = 1997

Once again using the named method the results can be filtered to display only the 'TITLE' node of the fifth 'CD' node of example-file.xml using the name parameter.

**Example WebDNA code:**\
This example uses an example file that you can view/download here [example-file.xml](https://docs.webdna.us/files/example-file.xml)

```
[xmlparse var=xml_var1][include file=example-file.xml][/xmlparse]
[xmlnodes ref=xml_var1&path=named:Catalog/CD(5)&name=TITLE]
  [name] = [value]
[/xmlnodes]
```

Results:\
TITLE = Eros

Further filtering is possible using the name and exact parameters, we can filter the results to display only those nodes, of the fifth 'CD' node, where the node name matches a given sub-string, 'CO' of example-file.xml

**Example WebDNA code:**\
This example uses an example file that you can view/download here [example-file.xml](https://docs.webdna.us/files/example-file.xml)

```
[xmlparse var=xml_var1][include file=example-file.xml][/xmlparse]
[xmlnodes ref=xml_var1&path=named:Catalog/CD(5)&name=CO&exact=F]
  [name] = [value]
[/xmlnodes]
```

Results: Both node names start with CO, so that satisfies the exact=F\
COUNTRY = EU\
COMPANY = BMG

### \[xmlnode] WebDNA v5.0+ [context](https://docs.webdna.us/contexts-and-tags)

The \[xmlnode] is used to retrieve the contents of the specific node, where as \[xmlnodes] is used to list all nodes within a path.

**Parameters**

| Parameter | Description                                                                                                                                                                                          |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ref       | Reference to an xml object variable. If this parameter is not provided, then it is assumed that there is an 'outer' \[xmlnode] or \[xmlnodes] context from which to reference a particular XML node. |
| var       | User defined name for this persisted node instance. If this parameter is not provided, then this node instance will not be persisted                                                                 |
| path      | Path to the desired XML node. If an XPath expression is used, it should evaluate to a single node.                                                                                                   |

**Tags available within a \[xmlnode] context:**

| Tag         | Description                                                                                                |
| ----------- | ---------------------------------------------------------------------------------------------------------- |
| name        | The name of the XML node.                                                                                  |
| value       | The value of the XML node. Will be empty if the node is a 'container' node, i.e. contains other XML nodes. |
| content     | The 'raw' XML content of the node.                                                                         |
| iscontainer | 'T' if the current node contains other XML nodes.                                                          |

The 'path' parameter is used to locate the node. As with the \[xmlnodes] context, the 'path' parameter has three modes; named, indexed, and xpath.

\[xmlnode] can also be used to persist a 'pointer' to a specific node. This reference can then be used in subsequent calls to other XML contexts.

From example-file.xml the \[xmlnode] context can be used to retrieve the TITLE information of the third CD node.

**Example WebDNA code:**\
This example uses an example file that you can view/download here [example-file.xml](https://docs.webdna.us/files/example-file.xml)

```
[xmlparse var=xml_var1][include file=example-file.xml][/xmlparse]
[xmlnode ref=xml_var1&path=indexed:1/3/1]
  [name] = [value]
[/xmlnode]
```

Results: Note that that the indexed path method is used. This assumes that the desired XML node is already known.\
TITLE=Greatest Hits

If the XML has been parsed previously on the page then there is no need to parse it again if performing subsequent operations on the same XML data. In the example below the var 'xml\_var1' is referenced as previously parsed XML data. Using \[xmlnode] to get all of the XML data within the node, the XML data within that node can be accessed using \[xmlnodes].

**Example WebDNA code:**\
This example uses an example file that you can view/download here [example-file.xml](https://docs.webdna.us/files/example-file.xml)

```
[xmlnode ref=xml_var1&path=indexed:1/3&var=xml_CD3][/xmlnode]
[xmlnodes ref=xml_CD3]
  [name] = [value]
[/xmlnodes]
```

Results: The content of the 3rd node is stored in memory by WebDNA with the reference 'xml\_CD3', this reference is then used to get the content of the referenced node.\
TITLE = Greatest Hits\
ARTIST = Dolly Parton\
COUNTRY = USA\
COMPANY = RCA\
PRICE = 9.90\
YEAR = 1982

### \[xmlnodesattributes] WebDNA v5.0+ [context](https://docs.webdna.us/contexts-and-tags)

The \[xmlnodesattributes] context is used to iterate the attributes of a specific XML node.

**Parameters**

| Parameter | Description                                                                                                                                                                                          |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ref       | Reference to an xml object variable. If this parameter is not provided, then it is assumed that there is an 'outer' \[xmlnode] or \[xmlnodes] context from which to reference a particular XML node. |
| path      | Path to the desired XML node. If an XPath expression is used, it should evaluate to a single node.                                                                                                   |

**Tags available within a \[xmlnodesattributes] context:**

| Tag      | Description                                           |
| -------- | ----------------------------------------------------- |
| name     | The name of the current iterated XML node attribute.  |
| value    | The value of the current iterated XML node attribute. |
| index    | The 'count' of the current iterated attribute.        |
| numfound | The total 'count' of iterated attributes.             |

The path parameter is used to locate the node. As with the \[xmlnodes] context and the \[xmlnode] context, the path parameter has three modes; named, indexed and xpath.

The \[xmlnodesattributes] context is used to retrieve the attributes of an XML node, the attributes are the parameters of the XML node container.

**Example WebDNA code:**\
This example uses an example file that you can view/download here [example-file.xml](https://docs.webdna.us/files/example-file.xml)

```
<b>Parse the xml file:</b><br>
[xmlparse var=xml_var1][include file=example-file.xml][/xmlparse]
<b>Get the contents of the indexed node:</b><br>
[xmlnode ref=xml_var1&path=indexed:1/1]
  [content]
[/xmlnode]<br>
<b>Display the attributes of the node:</b><br>
[xmlnodeattributes ref=xml_var1&path=indexed:1/1]
  [name] = [value]<br>
[/xmlnodeattributes]
```

Results:\
Parse the xml file:\
Get the contents of the indexed node:\
\<CD status="instock" id="123">\
\<TITLE>Empire Burlesque\</TITLE>\
\<ARTIST>Bob Dylan\</ARTIST>\
\<COUNTRY>USA\</COUNTRY>\
\<COMPANY>Columbia\</COMPANY>\
\<PRICE>10.90\</PRICE>\
\<YEAR>1985\</YEAR>\
\</CD>\
Display the attributes of the node:\
id = 123\
status = instock

To simplify the code, place the \[xmlnodesattributes] context 'inside' of the \[xmlnode] context. In this case, the 'path' or 'ref' parameters are not required \[xmlnodesattributes], as the 'implied' XML node in the outer \[xmlnode] context is used.

**Example WebDNA code:**\
This example uses an example file that you can view/download here [example-file.xml](https://docs.webdna.us/files/example-file.xml)

```
[xmlparse var=xml_var1][include file=example-file.xml][/xmlparse]
[xmlnode ref=xml_var1&path=indexed:1/1]
  [xmlnodeattributes]
    [name] = [value]<br>
  [/xmlnodeattributes]
[/xmlnode]
```

Results:\
id = 123\
status = instock

### \[xsl] WebDNA v5.0+ [context](https://docs.webdna.us/contexts-and-tags)

\[xsl] enables the application of style sheets to XML data, which allows the querying and rendering of the XML data as html. Use of the \[xsl] context requires familiarity with the XPath and XSLT languages. XSLT is a language for transforming XML documents into other XML documents. XPath is a language for defining parts of an XML document.

**Parameters**

| Parameter | Description                                                                                                         |
| --------- | ------------------------------------------------------------------------------------------------------------------- |
| name      | The variable name used to represent the XSL parsed object.                                                          |
| source    | A URL reference to an external XSL file. If 'source' is provided, the content within the \[xsl] context is ignored. |

These new contexts enable the WebDNA programmer to compile and apply XSL style sheets to XML data, allowing the programmer to query and render XML data as they see fit.

**Example WebDNA code:**

```
[xsl var=xsl_var1]
  <xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
    <xsl:template match="/">
      <html>
        <body>
          <h2>My CD Collection</h2>
          <table border="1">
            <tr bgcolor="#9acd32">
              <th>Title</th>
              <th>Artist</th>
            </tr>
            <xsl:for-each select="CATALOG/CD">
              <tr>
                <td><xsl:value-of select="TITLE"/></td>
                <td><xsl:value-of select="ARTIST"/></td>
              </tr>
            </xsl:for-each>
          </table>
        </body>
      </html>
    </xsl:template>
  </xsl:stylesheet>
[/xsl]
```

Results: WebDNA 'saves' this compiled instance as an internal variable in memory named 'xsl\_var1'. This saved instance can then be referenced when using the \[xslt] context on the same page.

The \[xsl] styles can also be called from a file.

**Example WebDNA code:**\
This example uses an example file that you can view/download here [example-xml-styles.xsl](https://docs.webdna.us/files/example-xml-styles.xsl)

```
[xsl var=xsl_var1][include file=example-xml-styles.xsl][/xsl]
```

### \[xslt] WebDNA v5.0+ [context](https://docs.webdna.us/contexts-and-tags)

The \[xslt] context allows the application of an XSL style sheet to an XML document and thus 'transform' the XML data into any format required (usually HTML).

**Parameters**

| Parameter | Description                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| xslref    | Named reference to a previously compiled block of XSL source.                                                                                                                                                                                                                                                                                                                                                                                         |
| xslsource | URL path to an external XSL document. This is an alternative to using the xslref parameter. Meaning that it is possible to use the \[xslt] context without having used the \[xsl] context to pre-compile XSL source code. However, if you plan to use the same XSL code several times throughout the template, then it would be more efficient to compile the XSL source code once, using \[xsl], then just use the XSL object reference when needed. |
| xmlsource | URL reference to an external XML document on which to apply the XSL transformation. If this parameter is supplied, then any in-line XML code that exists between the \[xslt] tags will be ignored.                                                                                                                                                                                                                                                    |

Select all the 'CD' nodes in example-file.xml and transform the results into an HTML table, with the 'text' data of each CD node tree displayed in the table rows.

**Example WebDNA code:**\
This example uses an example file that you can view/download here [example-file.xml](https://docs.webdna.us/files/example-file.xml)

```
[xsl var=xsl_var1]
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
  <xsl:template match="/">
    <html>
      <body>
        <h2>My CD Collection</h2>
        <table border="1">
          <tr bgcolor="#9acd32">
            <th>Title</th>
            <th>Artist</th>
          </tr>
          <xsl:for-each select="CATALOG/CD">
            <tr>
              <td><xsl:value-of select="TITLE"/></td>
              <td><xsl:value-of select="ARTIST"/></td>
            </tr>
          </xsl:for-each>
        </table>
      </body>
    </html>
  </xsl:template>
</xsl:stylesheet>
[/xsl]
[xslt xslref=xsl_var1][include file=example-file.xml][/xslt]
```

Or using a link to an external XML stylesheet.

**Example WebDNA code:**\
This example uses an example file that you can view/download here [example-file.xml](https://docs.webdna.us/files/example-file.xml) and the external style sheet [example-xml-styles.inc](https://docs.webdna.us/files/example-xml-styles.inc)

```
[xslt xslsource=example-xml-styles.xsl&xmlsource=example-file.xml][/xslt]
```

Results: Using either method, the results are the same

My CD Collection

| Title                    | Artist            |
| ------------------------ | ----------------- |
| Empire Burlesque         | Bob Dylan         |
| Hide your heart          | Bonnie Tylor      |
| Greatest Hits            | Dolly Parton      |
| Still got the blues      | Gary More         |
| Eros                     | Eros Ramazzotti   |
| One night only           | Bee Gees          |
| Sylvias Mother           | Dr.Hook           |
| Maggie May               | Rod Stewart       |
| Romanza                  | Andrea Bocelli    |
| When a man loves a woman | Percy Sledge      |
| Black angel              | Savage Rose       |
| 1999 Grammy Nominees     | Many              |
| For the good times       | Kenny Rogers      |
| Big Willie style         | Will Smith        |
| Tupelo Honey             | Van Morrison      |
| Soulsville               | Jorn Hoel         |
| The very best of         | Cat Stevens       |
| Stop                     | Sam Brown         |
| Bridge of Spies          | T\`Pau            |
| Private Dancer           | Tina Turner       |
| Midt om natten           | Kim Larsen        |
| Pavarotti Gala Concert   | Luciano Pavarotti |
| The dock of the bay      | Otis Redding      |
| Picture book             | Simply Red        |
| Red                      | The Communards    |
| Unchain my heart         | Joe Cocker        |

Download some further XSL styling examples: [more-example-xml-styles.xsl](https://docs.webdna.us/files/more-example-xml-styles.xsl)

**Appending XML to a WebDNA database**

This example uses XSLT to generate WebDNA \[append] code to append the XML data to a WebDNA database.

**Example WebDNA code:**\
This example uses an example file that you can view/download here [example-file.xml](https://docs.webdna.us/files/example-file.xml)

```
[xsl var=xsl_var1][!]
[/!]%5Breplace db=music.db[url]&[/url]eqTITLEdata=[url]&[/url]append=T%5D[!]
[/!]TITLE=[url]&[/url][!]
[/!]ARTIST=
%5B/replace%5D
[/xsl]
[text]result=[unurl][xslt xslref=xsl_var1][include file=example-file.xml][/xslt][/unurl][/text]
```

Results: The \[text] variable 'results' contains the following:\
\[replace db=music.db\&eqTITLEdata=Empire Burlesque\&append=T]TITLE=Empire Burlesque\&ARTIST=Bob Dylan\[/replace]\
\[replace db=music.db\&eqTITLEdata=Hide your heart\&append=T]TITLE=Hide your heart\&ARTIST=Bonnie Tylor\[/replace]\
\[replace db=music.db\&eqTITLEdata=Greatest Hits\&append=T]TITLE=Greatest Hits\&ARTIST=Dolly Parton\[/replace]\
\[replace db=music.db\&eqTITLEdata=Still got the blues\&append=T]TITLE=Still got the blues\&ARTIST=Gary More\[/replace]\
\[replace db=music.db\&eqTITLEdata=Eros\&append=T]TITLE=Eros\&ARTIST=Eros Ramazzotti\[/replace]\
\[replace db=music.db\&eqTITLEdata=One night only\&append=T]TITLE=One night only\&ARTIST=Bee Gees\[/replace]\
\[replace db=music.db\&eqTITLEdata=Sylvias Mother\&append=T]TITLE=Sylvias Mother\&ARTIST=Dr.Hook\[/replace]\
\[replace db=music.db\&eqTITLEdata=Maggie May\&append=T]TITLE=Maggie May\&ARTIST=Rod Stewart\[/replace]\
\[replace db=music.db\&eqTITLEdata=Romanza\&append=T]TITLE=Romanza\&ARTIST=Andrea Bocelli\[/replace]\
\[replace db=music.db\&eqTITLEdata=When a man loves a woman\&append=T]TITLE=When a man loves a woman\&ARTIST=Percy Sledge\[/replace]\
\[replace db=music.db\&eqTITLEdata=Black angel\&append=T]TITLE=Black angel\&ARTIST=Savage Rose\[/replace]\
\[replace db=music.db\&eqTITLEdata=1999 Grammy Nominees\&append=T]TITLE=1999 Grammy Nominees\&ARTIST=Many\[/replace]\
\[replace db=music.db\&eqTITLEdata=For the good times\&append=T]TITLE=For the good times\&ARTIST=Kenny Rogers\[/replace]\
\[replace db=music.db\&eqTITLEdata=Big Willie style\&append=T]TITLE=Big Willie style\&ARTIST=Will Smith\[/replace]\
\[replace db=music.db\&eqTITLEdata=Tupelo Honey\&append=T]TITLE=Tupelo Honey\&ARTIST=Van Morrison\[/replace]\
\[replace db=music.db\&eqTITLEdata=Soulsville\&append=T]TITLE=Soulsville\&ARTIST=Jorn Hoel\[/replace]\
\[replace db=music.db\&eqTITLEdata=The very best of\&append=T]TITLE=The very best of\&ARTIST=Cat Stevens\[/replace]\
\[replace db=music.db\&eqTITLEdata=Stop\&append=T]TITLE=Stop\&ARTIST=Sam Brown\[/replace]\
\[replace db=music.db\&eqTITLEdata=Bridge of Spies\&append=T]TITLE=Bridge of Spies\&ARTIST=T\`Pau\[/replace]\
\[replace db=music.db\&eqTITLEdata=Private Dancer\&append=T]TITLE=Private Dancer\&ARTIST=Tina Turner\[/replace]\
\[replace db=music.db\&eqTITLEdata=Midt om natten\&append=T]TITLE=Midt om natten\&ARTIST=Kim Larsen\[/replace]\
\[replace db=music.db\&eqTITLEdata=Pavarotti Gala Concert\&append=T]TITLE=Pavarotti Gala Concert\&ARTIST=Luciano Pavarotti\[/replace]\
\[replace db=music.db\&eqTITLEdata=The dock of the bay\&append=T]TITLE=The dock of the bay\&ARTIST=Otis Redding\[/replace]\
\[replace db=music.db\&eqTITLEdata=Picture book\&append=T]TITLE=Picture book\&ARTIST=Simply Red\[/replace]\
\[replace db=music.db\&eqTITLEdata=Red\&append=T]TITLE=Red\&ARTIST=The Communards\[/replace]\
\[replace db=music.db\&eqTITLEdata=Unchain my heart\&append=T]TITLE=Unchain my heart\&ARTIST=Joe Cocker\[/replace]

These results are then able to be 'appended' to the music.db

**Example WebDNA code:**

```
[interpret]
  [result]
[/interpret]
```
