---
description: >-
  Often important in web development is to establish the browser information so
  that customised code can be targeted to certain browsers. The information
  gained can also be used to exclude some visitors
---

# Browser Information

### \[browsername] [tag](https://docs.webdna.us/contexts-and-tags)

Putting \[browsername] in a template displays the name of the remote browser program running on the visitor's computer.

**Example WebDNA code:**

```
[browsername]
```

Results: Mozilla/5.0 (Macintosh; Intel Mac OS X 10\_15\_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/14.1.2 Safari/605.1.15

### \[ipaddress] [tag](https://docs.webdna.us/contexts-and-tags)

\[ipaddress] displays the ip address of the remote computer expanded to 3 digits per segment.

Alternatively, [\[realip\] ](broken-reference)displays the ip address without expanding to 3 digits per segment of the remote computer.

Using \[ipaddress] in a template will display the ip address of the remote computer that the visitor is using to view the site. All sections of the IP address are expanded to 3 digits, so that 207.67.2.14 will display as 207.067.002.014. This helps make partial comparisons inside \[showif] tags work more easily.

**Example WebDNA code:**

```
[ipaddress] 
```

Results: 180.075.030.116 (example)

### \[realip] WebDNA v8.1+ [tag](https://docs.webdna.us/contexts-and-tags)

\[realip] displays the ip address without expanding to 3 digits per segment of the remote computer.

Using \[realip] in a template will display the ip address of the computer that the visitor is using to view the site.

**Example WebDNA code:**

```
[realip] 
```

Results: 180.75.30.116

### \[referrer] [tag](https://docs.webdna.us/contexts-and-tags)

Displays the URL of the referring page

Putting \[referrer] in a template displays the URL of the referring page that led to this one.

**Example WebDNA code:**

```
[referrer]
```

Results: for this page https://docs.webdna.us/search-results

This will not work if the previous page was a form using post, ie: \<form method="post">

### \[httpmethod] [tag](https://docs.webdna.us/contexts-and-tags)

\[httpmethod] in a template displays the method by which the client browser requested the template.

**Example WebDNA code:**

```
[httpmethod]
```

Resolves: to either "GET" or "POST"\


### \[thisfile] [tag](https://docs.webdna.us/contexts-and-tags)

\[thisfile] will display the file name plus the path to the file relative to the root of the webserver.

**Example WebDNA code:**

```
[thisfile]
```

Results: /var/www/html/webdna\_root/commands-contexts/files-folders/filename.dna

### \[thishost] [tag](https://docs.webdna.us/contexts-and-tags)

\[thishost] will display the domain of the website that you are viewing.

**Example WebDNA code:**

```
[thishost]
```

Results: docs.webdna.us

### \[thisport] [tag](https://docs.webdna.us/contexts-and-tags)

A very quick and easy method of determining what port the browser is connect on. \[thisport] simply returns the port number, usually 80 or 443.

**Example WebDNA code:**

```
[thisport]
```

Results: 443

### \[thisurl] [tag](https://docs.webdna.us/contexts-and-tags)

\[thisurl] displays the URL of the current page. Inserting \[thisurl] in your template displays the URL of the current page being displayed as a relative path from the root of the server hierarchy. It will not show the domain name.

**Example WebDNA code:**

```
[thisurl]
```

Results:\
When used on a page such as

http://www.domain.com/x/y/z/index.dna?a=b\&c=d

\[thisurl] will return /x/y/z/index.dna, note the return stops at the ?

If the variables from a url are also required such as a and c from this url:\
http://www.domain.com/x/y/z/index.dna?a=b\&c=d then use \[thisurlplusget]

\


**\[thisurl] when writing "Pretty URLs" using .htaccess**\
When writing "pretty urls" using .htaccess \[thisurl] will

NOT

return the rewritten url, however there is a workaround if the "requested URL" is required.

Example:\
\# This rule is to manage products when URL = http://domain.com/department/\[DEPT-URL]\
\# Example: http://domain.com/department/clothing\
RewriteRule ^department/(\[a-z,0-9\\\_\\-]+)$ /department-display.dna\\?d=$1 \[NC,L]

In this example the results of a "department" are displayed using this URL http://domain.com/department/clothing, then using RewriteRule the required data can be displayed by sending the DEPT-URL to be processed by /department-display.dna\\?d=$1.

This may not be ideal if you want to use the requested URL for further navigation as \[thisurl] will return "/department-display.dna" or "/department-display.dna?d=clothing" if using \[thisurlplusget].

By capturing the request\_uri and passing the value as a variable to the WebDNA page simpley add it as a variable to the end of the rewrite string:\
/department-display.dna\\?d=$1\&varREQUESTSTRING=%{request\_uri} << note this is uri not url

Now on the WebDNA page:\
varREQUESTSTRING = \[varREQUESTSTRING] will display the original requested string as:\
varREQUESTSTRING = /department/clothing

### \[thisurlplusget] [tag](https://docs.webdna.us/contexts-and-tags)

\[thisurlplusget] displays the URL of the current page and the variables passed in a url. Inserting \[thisurlplusget] in a template displays the URL of the current page being displayed as a relative path from the root of the server hierarchy. It will not show the domain name. The advantage of \[thisurlplusget] vs \[thisurl]is that all the variables after the ? are also displayed

**Example WebDNA code:**

```
[thisurlplusget]
```

Results: When used on a page such as http://www.domain.com/x/y/z/index.dna?a=b\&c=d \[thisurlplusget] will return /x/y/z/index.dna?a=b\&c=d note that all of the variables in the url are returned

### \[setmimeheader] [tag](https://docs.webdna.us/contexts-and-tags)

Add or force a new MIME header to the outgoing HTML use \[setmimeheader]. MIME headers are normally used to create redirect requests and cookies. WebDNA already has special tags for generating redirects and cookies, there are cases when MIME headers for other purposes are required to be set.

**Example WebDNA code:**

```
[setmimeheader name=Content-Type&value=text/html; charset=iso-8859-1]
```

**Parameters**

| Parameter | Description                             |
| --------- | --------------------------------------- |
| Name      | The name of the MIME header to set.     |
| Value     | The value of the MIME header being set. |

### \[getmimeheader] WebDNA v6.0+ [tag](https://docs.webdna.us/contexts-and-tags)

Displays a specific MIME named header, if no header of that name exists, then nothing is returned.

**Example WebDNA code:**

```
[getmimeheader name=HTTP_ACCEPT_LANGUAGE]
```

Results: This will return the the human language that the viewer is able to read.

**Parameters**

| Parameter | Description                                                                                                                                     |
| --------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| Name      | The name of the MIME header to list.                                                                                                            |
| Exact     | T(rue) or F(alse) whether to exactly match the name of the parameter or match any name that contains the "name" value. (Default value is true.) |

### \[listmimeheaders] [context](https://docs.webdna.us/contexts-and-tags)

To display a list of all the MIME headers available sent from the remote browser, put a \[listmimeheaders] context into a template.

**Parameters**

| Parameter | Description                                                                                                                                     |
| --------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| Name      | The name of the MIME header to list.                                                                                                            |
| Exact     | T(rue) or F(alse) whether to exactly match the name of the parameter or match any name that contains the "name" value. (Default value is true.) |

**Tags available inside a \[listmimeheaders] context:**

| Tag      | Description                                                  |
| -------- | ------------------------------------------------------------ |
| \[index] | A number indicating this mime header's placement in the list |
| \[name]  | The name of the mime header                                  |
| \[value] | The value associated with the mime header                    |

**Example WebDNA code:**

```
[listmimeheaders]
  [index] [name] [value]<br>
[/listmimeheaders]
```

Results: This will return a list of ALL the MIME headers available to the page.

**Example WebDNA code:**

```
[listmimeheaders name=HTTP_HOST&exact=t]
  [value]
[/listmimeheaders]
```

Results: This example returns the value of the MIME header HTTP\_HOST, in the case of this page the value returned will be docs.webdna.us, note that https:// is not returned.

### \[returnraw] WebDNA v8.6.5 [context](https://docs.webdna.us/contexts-and-tags)

Sends 'raw' MIME headers and data back to browser.

\[returnraw]MIME Headers and Body content\[/returnraw]

To get more control over the exact MIME header information sent to the browser, place a \[returnraw] context into a template. You can create 'true' URL-redirect pages this way, or use it to force the remote browser to display password dialogs. Probably the most common use for this feature is to create "click-through" statistics on pages where you place advertisements with hyperlinks leading away from your site. ReturnRaw allows you to insert some WebDNA (to increment a hit counter, for instance) before leading the customer away from your site.

**Example WebDNA code:**

```
[returnraw]HTTP/1.0 200 OK
Content-type: text/html[unurl]%0D%0A%0D%0A[/unurl]<html>
...your HTML here...
</html>[/returnraw]
```

the above example must have several \<Carriage Return>\<LineFeed> at the end of the line in order to work. On a PC, simply using NotePad to create the file will put CR/LF in the proper places. On a Macintosh you may use BBEdit or another text editor to set the file type to DOS-style line endings.

For illustration purposes, the example above shows the MIME headers automatically created for you whenever you display any normal page from WebDNA. Of course, since these headers are always created automatically, this example does nothing special -- the real usefulness of this feature comes when you change the MIME headers to do something special, like a URL-redirect.

The following example causes the remote browser to immediately redirect to a different URL. This is better that the META tag some newer browsers support, because almost all browsers understand the low-level MIME header URL redirect.

```
[returnraw]HTTP/1.0 302 Found
Location: http://www.webdna.us/[unurl]%0D%0A%0D%0A[/unurl](some WebDNA here that does something interesting)
[/returnraw]
```

\


```
[returnraw]HTTP/1.1 301 Moved Permanently
Location: http://www.webdna.us[unurl]%0D%0A%0D%0A[/unurl][/returnraw]
```

when you put a \[returnraw] context into a template, any text outside the \[ReturnRaw] container is ignored. So if you want to display some HTML after the raw MIME headers, you must put that HTML inside the context.

**'BinaryBody' option:**

\[returnraw binarybody=...]\
Allows the WebDNA programmer to designate a file (binary or text) to be attached as the 'body' of an HTTP response. This makes it possible to send a binary file (image, executable, etc...) back to the client. For example, you can create download links for any type of file, and that would force the 'save as' dialog to open on the client machine.

Example code: Creates a download link for every file in the current folder, binary or text.

```
<!--HAS_WEBDNA_TAGS-->
<HTML>
<BODY>
[!]Initailize the getfile variable[/!]
[text secure=f]getfile=[/text][!]Check if filename was passed in[/!]
[showif [getfile]!]
[text]line_ending=%0D%0A[/text][!]Generate the response[/!]
[returnraw binarybody=[getfile]]HTTP/1.0 200 OK[unurl][line_ending][/unurl][!]
[/!]Status: 200[unurl][line_ending][/unurl][!]
[/!]Content-Type: application/octet-stream[unurl][line_ending][/unurl][!]
[/!]Content-Disposition: attachment; filename="[getfile]"[unurl][line_ending][line_ending][/unurl][!]
[/!][/returnraw][/showif][!] Generate a list of download links for all files in local directory [/!]
Click on a filename to download.
[listfiles path=.]
[showif [isfile]=T]
<a href=[thisurl]?getfile=[url][filename][/url]>[filename]</a><br>
[/showif]
[/listfiles]
</BODY>
</HTML>
```
