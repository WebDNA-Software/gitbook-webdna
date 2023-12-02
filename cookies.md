---
description: >-
  From version 8.5 developers have the built in functionality to save cookies as
  both https or http only, save a cookie with MaxAge or save a cookie with any
  of the emerging cookie settings required.
---

# Cookies

### \[setcookie] [tag](https://docs.webdna.us/contexts-and-tags)

Cookies are a great way to remember visitors, and to store variables from page to page.

```
[setcookie name=cookieName&value=cookieValue&expires=expireDate&path=/&domain=yourdomain.com]
```

Using \[setcookie] tag in a template causes the remote browser to create or replace a cookie of that name in its local list of cookies. Use \[getcookie] or \[listcookies] later to retrieve that value from the remote browser.

**Parameters**

| Parameter | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| name      | The name of the cookie                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| value     | The value to be saved in the cookie                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| expires   | The expiry date in the format: Wednesday, 09-Nov-2022 23:12:40 GMT                                                                                                                                                                                                                                                                                                                                                                                                                       |
| path      | The default value for path, '/', will allow any template on the website to access the cookie. You can confine a cookie to a specific directory. For instance, if you specify a path value of '/admin/', then only templates within the '/admin' directory can access the cookie.                                                                                                                                                                                                         |
| domain    | The domain must be the domain from which the cookie is being set, otherwise the browser will not provide the cookie information. Setting the domain parameter limits which domain may access the cookie. If you specify 'www.yourdomain.com', then only templates within that domain can access the cookie. Specifying just 'yourdomain.com' for the domain, will allow templates from 'www.yourdomain.com', 'secure.yourdomain.com', or 'whatever.yourdomain.com' to access the cookie. |

**New \[setcookie] parameters (from WebDNA 8.5)**

| Parameter | Description                                                                                                                                                                                                                          |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| HttpOnly  | (optional) HttpOnly should be T, just like Secure. It adds a HttpOnly to the cookie, and treats everything else as a F.                                                                                                              |
| MaxAge    | Max-age sets the time in seconds for when a cookie will be deleted ie MaxAge=30 will expire the cookie in 30 seconds                                                                                                                 |
| Raw       | RAW lets you add anything you want to the cookie. You can use it conjunction with the other parameters or in place of them. You must still specify a name and value. Separate multiple raw parameters with a semi-colon and a space. |

**Example secure cookie code:**

```
[setcookie name=cookieName&value=cookieValue&HttpOnly=T&Raw=domain=example.com; secure]
or
[setcookie name=cookieName&value=cookieValue&Raw=domain=example.com; secure; HttpOnly]
```

### \[getcookie] [tag](https://docs.webdna.us/contexts-and-tags)

Displays the value of the cookie named "member" that the remote browser has remembered. If no cookie of that name exists, then nothing (a blank value) is returned.

```
[getcookie name=cookieName]
```

It is important to understand that when you set a cookie on a page, it is NOT available via \[getcookie] further down in the same page. Only when the user accesses the NEXT page does the cookie become available. For this reason, \[redirect \[thisurl]] is often used immediately after setting a cookie to make it available.

### \[listcookies] [context](https://docs.webdna.us/contexts-and-tags)

listcookies lists all the cookie names and values sent from the remote browser. listcookies is very helpful when writing code using cookies, to see if and when cookies are being set.

```
[listcookies parameters]Cookie Tags[/listcookies]
```

\


**Parameters**

The \[listcookies] context has optional parameters to modify the list of cookies produced. To list the cookies whose name begins with _"text"_

| Parameter | Description                                                                                                                                |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| name      | (optional) The name of the cookie to list.                                                                                                 |
| exact     | T or F (optional) Whether to exactly match the cookie name or return any cookie name that contains the "name" value. (Default value is T.) |

**Tags available inside a \[listcookies] context:**

| Tag      | Description                                             |
| -------- | ------------------------------------------------------- |
| \[index] | A number indicating this cookie's placement in the list |
| \[name]  | The name of the cookie                                  |
| \[value] | The value associated with the cookie                    |

**Example WebDNA code:**

```
[listcookies name=customer&exact=t]
[index] [name] [value]
[/listcookies]
```
