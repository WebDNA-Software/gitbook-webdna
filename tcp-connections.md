---
description: >-
  Most of the world’s HTTP communication is via TCP/IP, a layered set of
  packet-switched network protocols between computers and network devices.
  Messages can be exchanged between servers using TCP conn
---

# tcp connections

### \[tcpconnect] WebDNA v3.0+ [context](https://docs.webdna.us/contexts-and-tags)

\[tcpconnect] is a powerful feature that allows the connection to a TCP port of another computer on the Internet.

**Parameters**

| Parameter | Description                                                                                                                                                                                                                                                                                                                                           |
| --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| host      | (Required) Name or ip address of the machine to connect to. Do not insert http or ftp into the host text. \[tcpconnect] is a very low-level connection, and it knows nothing about these protocols.                                                                                                                                                   |
| port      | (Optional) TCP port number to connect to. If not specified, port 80 is assumed. Although optional, most servers will only accept secure connections. RECOMMENDED: \&port=443                                                                                                                                                                          |
| ssl       | (Optional) Set this parameter to 'T' (case sensitive) to create a secure socket. The port parameter should normally be set to 443 in this case. Although optional, most servers will only accept secure connections. RECOMMENDED: \&ssl=T                                                                                                             |
| timeout   | From version 8.0.3 \[tcpconnect] lets you set a timeout in seconds so that if you are writing the result to a database data.db and the remote server doesn't respond in a reasonable amount of time, then the database will be released after the timeout expires. Otherwise a lock would remain held on data.db and denying a read from the database |

Although, it is powerful and flexible; the HTTP protocol is not suitable for use in a wide range of applications because it can be so easily monitored and replayed. For example, someone using a network monitor can easily capture passwords used to access a banking web site or replay requests that trigger financial transactions.

The Secure Sockets Layer (SSL) was designed to encrypt any TCP/IP based network traffic and provide the following capabilities:

* Prevents eavesdropping
* Prevents tampering or replaying of messages
* Uses certificates to authenticate servers and optionally clients

**The use of \[tcpconnect] using SSL and PORT options is highly recommended, most connections will fail without them**\
To embed the results of a TCP session into a page, place a \[tcpconnect] context into a template, and use \[tcpsend] contexts inside of that. The \[tcpsend] steps contained inside the context execute, and any returned value displays in place of the context. Any WebDNA tags inside the context are first substituted for their real values before the TCPSend executes.

Each component inside the \[tcpsend] context requires a "carriage return, line feed" at the end. This can be achieved in WebDNA by using the following string: \[unurl]%0D%0A\[/unurl]. The final line of the \[tcpsend] context requires a DOUBLE "carriage return, line feed" ie: \[unurl]%0D%0A%0D%0A\[/unurl].

\[tcpconnect] does nothing by itself, a \[tcpsend] must insert one or more contexts inside it to perform any real work. \[tcpconnect] establishes a connection to the TCP server program and provides an environment for the \[tcpsend] contexts to do their work and return text results.

**Example WebDNA code:**

```
[tcpconnect host=docs.webdna.us&port=443&ssl=T][!]
    [/!][tcpsend]GET /files/example.html HTTP/1.0[unurl]%0D%0A[/unurl][!]
      [/!]Host: docs.webdna.us[unurl]%0D%0A[/unurl][!]
      [/!][unurl]%0D%0A%0D%0A[/unurl][!]
    [/!][/tcpsend][!]
[/!][/tcpconnect]
```

\[tcpsend] will most likely error on the responding server if there are line breaks in the requesting code.\
**BEST PRACTICE:** Make \[tcpconnect] code all one line, or use WebDNA comments \[!] .... \[/!] to break up code into segments for easier reading during development.

In this example, the http command equivalent to: "https://docs.webdna.us/files/example.html" executes using a secure connection on port 443, and results in displaying the "example return" page on the site. Notice the use of \[unurl] to send \<carriage return>\<line feed>\<carriage return>\<line feed> as part of the TCPSend text. If you do not send the correct sequence of 2 x CR/LF characters, the remote web server will not return any text, and the \[tcpsend] will time out while waiting for a response.

### \[tcpsend] WebDNA v3.0+ [context](https://docs.webdna.us/contexts-and-tags)

\[tcpsend] is the 'engine room' of the \[tcpconnect] context. Within the \[tcpsend] context is where the data is conveyed to the connected server so that the required information can be returned.

**Parameters**

|            |                                                                                         |
| ---------- | --------------------------------------------------------------------------------------- |
| Parameter  | Description                                                                             |
| skipheader | (Optional) T/F instructs the WebDNA engine to 'strip' the MIME headers from the result. |

Use skipheader=t when requesting json data so that the returned headers do not corrupt the returning json or xml data.

\[tcpsend] and \[tcpconnect] will allow the recovery of information from remote servers, the usage of \[tcpconnect] and \[tcpsend] is mostly used to get data in the form of XML or JSON from another server so that the returned data can be used on the WebDNA site, stored in a database or used to communicate with a Payment Gateway.

To embed the results of a TCP session into a web page, use \[tcpconnect] context in a template, with \[tcpsend] contexts inside the \[tcpconnect]. The TCPSend steps contained inside the context executes, and any returned value is displayed. Any WebDNA tags inside the context are first substituted for their real values before the TCPSend executes.

Each component inside the \[tcpsend] context requires a "carriage return, line feed" at the end. This can be achieved in WebDNA by using the following string: \[unurl]%0D%0A\[/unurl]. The final line of the \[tcpsend] context requires a DOUBLE "carriage return, line feed" ie: \[unurl]%0D%0A%0D%0A\[/unurl].

With the requirement of a secure connection by using \&port=443\&ssl=T in a request, the requirement for the addition of declaring the "HOST" within \[tcpsend] becomes paramount.

**Example WebDNA code:**\
Requesting json data without skipheader=t

```
[tcpconnect host=docs.webdna.us&port=443&ssl=T][!]
  [/!][tcpsend]GET /files/example.json HTTP/1.0[unurl]%0D%0A[/unurl][!]
    [/!]Host: docs.webdna.us[unurl]%0D%0A[/unurl][!]
    [/!][unurl]%0D%0A%0D%0A[/unurl][!]
  [/!][/tcpsend][!]
[/!][/tcpconnect]
```

**Results:**

```
HTTP/1.1 200 OK Date: Sun, 22 May 2022 22:57:11 GMT Server: Apache/2.4.41 (Ubuntu) MIME-Version: 1.0 Expires: Sun, May 22 2022 22:57:11 GMT Connection: close Content-Length: 264 Vary: Accept-Encoding Content-Type: text/html 
{"firstname":"John", "lastname":"Smith", "age":30, "car":null}, 
{"firstname":"Sally", "lastname":"Jones", "age":42, "car":"BMW"}, 
{"firstname":"Susan", "lastname":"Barlow", "age":29, "car":"Ford"}, 
{"firstname":"John", "lastname":"Lyons", "age":18, "car":"Toyota"}
```

If the expected return is a json object, then this result will fail due to the extra content before the json component.

**Example WebDNA code:**\
Requesting json data WITH skipheader=t

```
[tcpconnect host=docs.webdna.us&port=443&ssl=T][!]
  [/!][tcpsend skipheader=t]GET /files/example.json HTTP/1.0[unurl]%0D%0A[/unurl][!]
    [/!]Host: docs.webdna.us[unurl]%0D%0A[/unurl][!]
    [/!][unurl]%0D%0A%0D%0A[/unurl][!]
  [/!][/tcpsend][!]
[/!][/tcpconnect]
```

**Results:**

```
{"firstname":"John", "lastname":"Smith", "age":30, "car":null}, 
{"firstname":"Sally", "lastname":"Jones", "age":42, "car":"BMW"}, 
{"firstname":"Susan", "lastname":"Barlow", "age":29, "car":"Ford"}, 
{"firstname":"John", "lastname":"Lyons", "age":18, "car":"Toyota"}
```

Now the returned json object can be used without error in javascript, or saved using WebDNA's \[jsonstore]

**Another example to get the change euro/dollar with an API:**

```
The full URL is https://docs.webdna.us/files/example-exchange-rate.json?from=USD&to=EUR
For this one, we have to simulate a real browser
[text]varHOST=docs.webdna.us[/text]
[text]varGET=/files/example-exchange-rate.json?from=USD&to=EUR[/text]
[text]crlf=[unurl]%0D%0A[/unurl][/text]
[text]USexchange=[!]
  [/!][tcpconnect host=[varHOST]&port=443&ssl=T&timeout=20][!]
    [/!][tcpsend skipheader=T][!]
      [/!]GET [varGET] HTTP/1.0[crlf][!]
      [/!]Host: [varHOST][crlf][!]
      [/!]User-Agent: Mozilla/4.0 (compatible; MSIE 5.01; Windows NT 5.0)[crlf][!]
      [/!][crlf][!]
    [/!][/tcpsend][!]
  [/!][/tcpconnect][!]
[/!][/text]US$1.00 = [format .2f][middle startafter="rate":"&endbefore="][USexchange][/middle][/format] EUR
```

**Results:**\
US$1.00 = 0.95 EUR
