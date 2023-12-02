# Sessions

### \[session] WebDNA v8.1 [tag](https://docs.webdna.us/contexts-and-tags)

Using \[session] is a WebDNA feature that recognises a browser through its "fingerprint", and removing the need for a cookie in most cases. It is fast and easy to use.

**Associated Tags**

| Tag               | Description                                                                                                                                                                                          |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \[session]        | \[session] has a unique value once the session is started. This value can be moved from one page to another using POST or GET, or appended to a link or redirect.                                    |
| \[sessionstart]   | Starts a session by recording the day, time of the day, IP, Unique browser ID and (**optional**) the life in seconds, like in \[sessionstart life=60]                                                |
| \[sessionend]     | Kills a \[session] by deleting it from the reserved.db                                                                                                                                               |
| \[sessionalive]   | T (true) or F (false) Checks the current time against the session time to determine if the session is still alive                                                                                    |
| \[sessionip]      | Shows the IP address stored in the session, WebDNA formatted \[ipaddress] (with leading 0's)                                                                                                         |
| \[sessionipmatch] | T (true) or F (false) Checks if the session IP address is the same as the current IP address                                                                                                         |
| \[browseridmatch] | T (true) or F (false) Checks the browser "fingerprint" against the session's browser ID                                                                                                              |
| \[sessiondate]    | Shows the date stored in the session                                                                                                                                                                 |
| \[sessiontime]    | Shows the time stored in the session                                                                                                                                                                 |
| \[sessionlife]    | Calculates how many seconds a session has till it expires, shows 0 if expired                                                                                                                        |
| \[sessionutime]   | Shows the session's Unix Time (also known as POSIX time or Epoch time, defined as the number of seconds that have elapsed since 00:00:00 Universal Coordinated Time (UTC), Thursday, 1 January 1970) |

**Example WebDNA code:**

```
If a visitor's browser does not return a previously set session, then start a new session
sessionstart = [sessionstart life=1200]  < starts a 20 min (60 seconds x 20 minutes) session
session = [session]Now send the [session] value back to the visitor in a POST form:
<input type="hidden" name="session" value="[session]">
or in a url:
mypage.dna?session=[session]When the visitor clicks through to mypage.dna, check for the session:
session = [session]
sessionalive = [sessionalive]
sessionlife = [sessionlife]
sessionIPmatch = [sessionIPmatch]
browserIDmatch = [browserIDmatch]
sessionip = [sessionip]
sessiondate = [sessiondate]
sessiontime = [sessiontime]
sessionutime = [sessionutime][sessionlife] will indicate how much longer the session has until it expires,
this is useful if a user is limited to the amount of time spent on a page or a website, 
or if the need to record how long that user has spent on the site.A simple use:
[if ("[sessionalive]"="T")]
   [then]  OK  [/then]
   [else]  redirect out of secure area  [/else]
[/if]
```

**HINT:**\
The page that RECEIVES the session MUST include the \[session] tag, if it is being passed as a variable named "session" WebDNA will recognise the variable and use the value, however if the session value is being passed with a different name such as "CheckUser" then that variable needs to be converted to "session" on the receiving page, simply turn it into a text variable as such:\
\[text]session=\[CheckUser]\[/text], now WebDNA can use the value to do further calculations on the session such as the ones above in the example.

**ADVANTAGES:**\
Works with search engines\
Security is increased by not allowing a cookie to be manipulated\
Easier to use and much faster than a client-side cookie\
Good for visitors who do not want to leave information behind them (cookie disabled)\
Allows to easily kill a session\
Allows to move a session from one browser to another browser

In certain rare cases, it is possible to find two identical browser "fingerprints" or BrowserID. It is not advised to do visitor recognition based only on Browser ID

### \[sessionstart] WebDNA v8.1+ [tag](https://docs.webdna.us/contexts-and-tags)

\[sessionstart] starts a session by recording the day, time of the day, IP, Unique browser ID and optionally the life of the session in seconds.

**Example WebDNA code:**

```
[sessionstart life=1200]
starts a 20 min (60 seconds x 20 minutes) session
[sessionstart] without any specification has an unlimited lifetime
```

The session is recorded inside a database "reserved.db" in /WebDNA (FastCGI version) or in /globals/ (server version), one column is for the "browserID" and the other one for the session value.

The value of the unique session is available through the WebDNA tag \[session], a unique session identifier, and allows the developer to pass this \[session] information from one page to the other, store it and link it to a name and/or a shopping cart, or other requirement.

**USAGE:**\
Follow the visitor by moving the \[session] value from one page to the other with a POST or GET\
Identify a visitor who logged-in : associates the tag \[session] with the user name\
Identify a visitor who comes back after a period of time. Depending on the settings, testing the session will enable recognition of the visitor.

In certain rare cases, it is possible to find two identical browser "fingerprints" or BrowserID. It is not advised to do visitor recognition based only on Browser ID

### \[sessionend] WebDNA v8.1+ [tag](https://docs.webdna.us/contexts-and-tags)

A \[sessionend] tag will "kill" the session and remove it from the reserved.db

### \[sessionlife] WebDNA v8.1+ [tag](https://docs.webdna.us/contexts-and-tags)

\[sessionlife] returns the number of seconds that the session will remain alive. If sessionstart has been set without a time limit, \[sessionlife] will return "-1".

### \[sessionalive] WebDNA v8.1+ [tag](https://docs.webdna.us/contexts-and-tags)

To determine if a session is still alive and not expired use \[sessionalive] this will return T (true) or F (false)

### \[sessionip] WebDNA v8.1+ [tag](https://docs.webdna.us/contexts-and-tags)

To determine if a session is still alive and not expired use \[sessionip] tag will return the ip address of the browser expanded to 3 digits per segment.

**Example WebDNA code:**

```
[sessionip]
```

Results: 180.75.30.116

### \[sessionipmatch] WebDNA v8.1+ [tag](https://docs.webdna.us/contexts-and-tags)

To determine if a session ip address is the same as the one being returned from the browser, use \[sessionipmatch] this will return T (true) or F (false)

### \[sessiondate] WebDNA v8.1+ [tag](https://docs.webdna.us/contexts-and-tags)

\[sessiondate] tag will return the date that the session was started in the format that your WebDNA preferences are set, ie _mm/dd/yyyy_ or _dd/mm/yyyy_.

### \[sessiontime] WebDNA v8.1+ [tag](https://docs.webdna.us/contexts-and-tags)

\[sessiontime] tag will return the time that the session was started in the format hh:mm:ss

### \[sessionutime] WebDNA v8.1+ [tag](https://docs.webdna.us/contexts-and-tags)

\[sessionutime] tag will return the time that the session was started in UNIX format. Defined as the number of seconds that have elapsed since 00:00:00 Universal Coordinated Time (UTC), Thursday, 1 January 1970.
