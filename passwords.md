# Passwords

### \[username] [tag](https://docs.webdna.us/contexts-and-tags)

Using \[password] and \[username] is an easy was to show the browser login dialog box

Putting \[username] in a template will display the username entered into the remote browser's last Realm password dialog.

**Example WebDNA code:**

```
[showif [username]!MyLogin]
  [authenticate user]
[/showif]
[showif [password]!MyPassword]
  [authenticate password]
[/showif]
```

This will display the browser's regular login dialog box unless the last Realm was MyLogin:MyPassword

### \[password] [tag](https://docs.webdna.us/contexts-and-tags)

Using \[password] and \[username] is an easy was to show the browser login dialog box

Putting \[password] in a template displays the password entered into the remote browser's last Realm password dialog.

**Example WebDNA code:**

```
[showif [username]!MyLogin]
  [authenticate user]
[/showif]
[showif [password]!MyPassword]
  [authenticate password]
[/showif]
```

This will display the browser's regular login dialog box unless the last Realm was MyLogin:MyPassword

### \[protect] [tag](https://docs.webdna.us/contexts-and-tags)

Using \[protect] in a template causes the user's browser to display the username/password dialog until the visitor enters a username/password belonging to one of the specified groups. The Users.db database provided with WebDNA contains the fields user, pass, groups, expire\_date and expire\_del. It does not contain any other information such as name, address, so it is primarily useful when there are admin pages that need to be accessed by specific users.

**Parameters**

| Parameter  | Description                                                                                                                                   |
| ---------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| group name | A user group name must be present in the \[protect _group1,group2_] tag. If more than one group is allowed, then used a comma delimited list. |

**Users.db**\
Users may be added to the Users.db in the WebDNA administration area, under the Security section. The Admin group has built-in privileges to access the site's WebDNA preferences, so create new groups other than admin for use within a web site. Only use the group admin for users authorised to configure the site's WebDNA settings.T here is no special "create group" button in WebDNA admin, a group can be created in the group text area when creating a user.

**Example WebDNA code:**

```
[protect admin,developers]
```

This will only allow members of the admin and the developers groups to access the pages if they authenticate correctly.

**Working with groups example WebDNA code:**\
Using the \[protect _groups_] tag with multiple groups, but each group is to be delivered different content:

```
[protect group1,group2]
[search db=Users.db&eqUSERdatarq=[uppercase][username][/uppercase]&eqPASSdatarq=[url][encrypt][uppercase][password][/uppercase][/encrypt][/url]]
    [founditems]
        [text]varGROUP=[groups][/text]
    [/founditems]
[/search][showif [varGROUP]=group1]Content for GROUP1[/showif][showif [varGROUP]=group2]Content for GROUP2[/showif]
```

Or another method used to hide content from one of the groups

```
[search db=Users.db&GROUPSword=ww&woGROUPSdatarq=[uppercase]group1[/uppercase]&eqUSERdatarq=[uppercase][username][/uppercase]&eqPASSdatarq=[url][encrypt][uppercase][password][/uppercase][/encrypt][/url]]
    [showif [numfound]=0]
        Content hidden from GROUP1 
    [/showif]
[/search]
```

After users with groups are created, use the \[protect _groups_] tag on any page. When using mulitiple groups, using \[protect group2,admin], this gives a group2 member authority to access the protected page, it also gives any member of admin the same access, but it does NOT confer admin status to anyone in group2.

Be careful how groups are named. The code that checks the User.db looks for whole words to match the group. If groups are named in someway that could be misconstrued as two separate words, such as ABC\_GROUP1, ABC\_GROUP2 and group ABC also exists, then the first two will be seen by WebDNA as belonging to ABC, because the underscore is read as a delimiter.

Other problematic delimiters are dashes, spaces and periods.

The difference between \[protect] and \[authenticate] tags is that the \[protect] is directly associated with WebDNA's Users.db, whereas the \[authenticate] tag is for developers who prefer to create their own user system, such as a database with more extensive fields and functionality than the simple Users.db.

**Directory Protection**\
Directories cannot be protected with WebDNA, because the webserver does not return any processing data to WebDNA if a directory listing is requested, so something such as a directory of images can not be protected. In this instance control with an .htpasswords file, or realm protection on a server level is required.

### \[authenticate] [tag](https://docs.webdna.us/contexts-and-tags)

\[authenticate] is a low-level tool requiring further code to make it work for password protection schemes.

An example of how \[protect] makes use of \[authenticate] by inspecting the file "MultiGroupChecker" in the WebCatalogEngine folder, or in the Sandbox folder or the WebDNA folder is using fcgi version.

**Basically, this is how it works:**\
The "MultiGroupChecker" checks

1. if the \[username] is in the Users.db
2. if the \[username] and \[password] match in the Users.db
3. if the \[username] is allowed access to the specified group in the \[protect _groups_] tag
4. if the \[username] has expired

If the user fails step 1, the authentication box will display. If the user fails step 2, the authentication box will display. If the user passes step 1 - 4 then the user will be allowed to access the page without the authentication box displaying.
