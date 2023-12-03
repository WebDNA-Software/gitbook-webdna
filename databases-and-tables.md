# Databases & Tables

### \[append] [<mark style="color:red;">context</mark>](https://docs.webdna.us/contexts-and-tags)

Appends a new record with the specified field values to the end of a database.

```
[append db=base.db]cust=Grant&address=1492 High Street&zip=9000[/append]
```

\


**Parameters**

| Parameter  | Description                                                                                         |
| ---------- | --------------------------------------------------------------------------------------------------- |
| autonumber | Instructs WebDNA to automatically generate the 'next highest number' value for the given fieldname. |

**Example WebDNA code:**

```
[append db=base.db&autonumber=cust_id]cust=Grant&address=1492 High Street&zip=9000[/append]
```

The database "base.db" is opened, and a new record is added to the end. The field name "cust" is set to "Grant," the field name "address" is set to "1492 High Street" the field name "zip" is set to "9000," and the field name "date" is set to the current date.

Any field names not existing in the database are ignored, and any fields you do not specify are left blank in the new record. Certain letters are illegal, such as \<tab> or \<carriage return>, so they are converted to \<space> and \<soft return> before being added to the database. Some computers use the two-character sequence \<carriage return>\<line feed> to indicate a single end of line, which is automatically converted to a single \<soft return> character before being added to the database.

You may specify an absolute or partial path to the database file, as in "/WebCatalog/folder/base.db" or "../base.db". Relative paths start in the same folder as the template, just like URLs, so "../" will look "up" one folder level from the template, and "/" will start at the web server's root.

Normally all database file paths are relative to the local template, or if they begin with "/" they are relative to the web server's virtual host root. You may optionally put "^" in front of the file path to indicate the file can be found in a global root folder called "Globals" inside the WebCatalogEngine folder. This global root folder is the same regardless of the virtual host.

You can specifiy a WebDNA \[table], in place of a db file, in the Append context.\
For example: \[append table=TableName]values\[/append]

You can use \[thisautonumber] tag from within an \[append] or \[replace] context, to retrieve the current auto-generated number if the AUTONUMBER parameter was used.

Some database filenames are reserved.\
You may not create your own database named "WebCatalog Prefs," "Users.db," "ErrorMessages.db," "StandardConversions.db," or "Triggers.db"

\


### \[replace] [context](https://docs.webdna.us/contexts-and-tags)

Replaces each found record in a database with the new field values.

```
[replace Parameters]new values[/replace]
```

**Parameters**

| Parameter          | Description                                                                                                                                                                                            |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| db                 | path to database file, relative to this template.                                                                                                                                                      |
| table              | In place of a db file, you can specify a named reference to a WebDNA [table](https://docs.webdna.us/Table.html) object.                                                                                |
| _SearchParameters_ | Search information describing which records should be found and replaced. Can be any complex search criteria, works exactly like \[search] context.                                                    |
| Append             | (Optional) "T" if you want a new record to be added to the end of the database in the case where no records were found to be replaced. Any fields you do not specify are left blank in the new record. |
| autonumber         | (Optional) Instructs WebDNA to automatically generate the 'next highest number' value for the given fieldname.                                                                                         |

To replace records in an ODBC-compliant table controlled by a SQL server, use the \[SQL] context.

You can specifiy a WebDNA table, in place of a db file, in the Replace context.\
For example: \[replace table=TableName&...]values\[/replace]

To replace records in a database, put a \[replace] context into a template. Whenever WebDNA encounters a Replace, it immediately searches for the specified records in the database, and replaces those records' fields with the named field values inside the Replace.

**Example WebDNA code:**

```
[replace db=somebase.db&eqNAMEdata=Grant]name=John&address=1492 Street&zip=9000&date=[date][/replace]
```

The database "somebase.db" opens and all records whose name field is "Grant" are found. The field names "name" is set to "John," "address" is set to "1492 Street" "zip" is set to "9000," and "date" is set to the current date. Notice that any WebDNA \[xxx] tags inside the context are first substituted for their real values before being written to the database. The name of the database itself may also be an \[xxx] tag, as in \[replace db=\[formvariable]]\[/replace].

Any field names not existing in the database are ignored, and if you leave some existing field names out of the replace context, they will remain unchanged in the database. Certain letters are illegal, such as TAB or CARRIAGE RETURN, so they are converted to SOFT TAB and SOFT RETURN before being added to the database. Some computers use the two-character sequence CARRIAGE RETURN - LINE FEED to indicate a single end of line, which is automatically converted to a single SOFT RETURN character before being added to the database. These 'soft' characters are automatically converted back to 'hard' versions (the originals) whenever you retrieve fields from a search of the database.

You may specify an absolute or relative path to the database file, as in "/WebCatalog/GeneralStore/somebase.db" or "../somebase.db".

Normally all database filepaths are relative to the local template, or if they begin with "/" they are relative to the web server's virtual host root (MacOS and Unix versions only; PC versions treat the DBServer.exe folder as root regardless of the virtual host). You may optionally put "^" in front of the file path to indicate the file can be found in a global root folder called "Globals" inside the WebCatalogEngine folder. This global root folder is the same regardless of the virtual host.

You can use the 'AUTONUMBER=' parameter with the \[append] or \[replace] context to instruct WebDNA to automatically generate the 'next highest number' value for the given fieldname. This is useful for 'ID' type fields, where unique values are required.

Here is a demonstration of the AUTONUMBER feature using a WebDNA table, this will also work on a WebDNA database field.

**Example WebDNA code:**

```
[table name=table_1&fields=ID,NAME,EMAIL][/table][append table=table_1&AUTONUMBER=ID]NAME=Scott&EMAIL=scott@here.com[/append]
[append table=table_1&AUTONUMBER=ID]NAME=Lee&EMAIL=lee@there.com[/append]
[append table=table_1&AUTONUMBER=ID]NAME=OMNI&EMAIL=omni@everywhere.com[/append]
[delete table=table_1&eqIDdata=2]
[append table=table_1&AUTONUMBER=ID]NAME=Lee&EMAIL=lee@there.com[/append][search table=table_1&neIDdata=[blank]]
  [founditems]
    [ID] - [NAME] - [EMAIL]
  [/founditems]
[/search]
```

Results:

```
1 - Scott - scott@here.com
3 - OMNI - omni@everywhere.com
4 - Lee - lee@there.com
```

WebDNA automatically generated the ID value by calculating the next largest value, given the existing ID values in the table.

\[thisautonumber] tag can be used from within an \[append] or \[replace] context, to retrieve the current auto-generated number, if the AUTONUMBER parameter was used.

### \[delete] [tag](https://docs.webdna.us/contexts-and-tags)

```
[delete db=some.db&eqNAMEdata=Fred]
```

Putting \[delete db=some.db\&eqNAMEdata=Fred] in your template opens the some.db database (you can specify a DatabasePath), finds all the records whose NAME field contains "Fred," and deletes those records and exclusively those records. Any valid search parameters are allowed, as defined in the \[search] context.

if the database has username and password fields, then the records will not be deleted unless the visitor's web browser username/password match the record's username/password.

You can specifiy a WebDNA table, in place of a db file.\
For example: \[delete table=TableName&...].

### \[exclusivelock] WebDNA v4.0 [context](https://docs.webdna.us/contexts-and-tags)

Prevents other threads from simultaneously accessing a single or group of databases.

```
[exclusivelock database list]...WebDNA...[/exclusivelock]
```

To prevent a group of databases from being modified by other threads (other 'hits' to the server, or other templates or triggers), wrap an \[exclusivelock] context around the WebDNA code which will be making the important exclusive changes.

**Example WebDNA code:**

```
[exclusivelock db=orders.db&db=lineitems.db&db=accesslog.db]
...search, delete, or modify any of orders.db, lineitems.db, or accesslog.db while being assured that no other threads can modify any of these databases until the closing /exclusivelock tag is reached.
[/exclusivelock]
```

The list of database names is first alphabetized so as to maintain a consistent locking order (a technique which prevents internal deadlocks), then each database lock is acquired one at a time until all locks are acquired, then the interior WebDNA is executed. If any lock cannot be acquired, the other databases are unlocked, and the interior WebDNA is not executed.

### \[lastautonumber] WebDNA v5.1+ [tag](https://docs.webdna.us/contexts-and-tags)

Displays the last auto-generated number.

Putting \[lastautonumber] in your template displays the same value as the last auto-generated number used in the last \[append] or \[replace] context that used the 'AUTONUMBER' parameter. Only valid for the current template instance.

**Example WebDNA code:**

```
[append db=mydata.db&autonumber=ID]name=Jack&lastname=Jones[/append]
The last autonumber in mydata.db = [lastautonumber]
```

Results: The last autonumber in mydata.db = 73561

### \[closedatabase] [tag](https://docs.webdna.us/contexts-and-tags)

```
[closedatabase db=mydata.db]
```

Putting \[closedatabase db=mydata.db] in a template causes the specified database to be written to disk and closed. This is only needed for special cases, usually before appending to a file, where you need to change a file perhaps cached in RAM.

WebDNA automatically closes databases when it needs more memory, so you typically do not need to use this tag.

Adding 'commit=f' as \[closedatabase db=mydata.db\&commit=f] will cause the specified database file to be closed but not written to disk, any unsaved changes will be lost

### \[commitdatabase] [tag](https://docs.webdna.us/contexts-and-tags)

```
[commitdatabase db=mydata.db]
```

Putting \[commitdatabase db=mydata.db] in a template causes the specified database file to be written but not closed, it will remain in RAM. This is only needed for special cases where it is absolutely certain that a database has to been written to disk.

### \[flushdatabases] [tag](https://docs.webdna.us/contexts-and-tags)

Putting \[flushdatabases] in a template causes ALL databases to be written to disk and closed. This is only needed for special cases, usually before appending to a file, where you need to change a database that may be cached in RAM, and you do not know the exact name of the database. WebDNA automatically closes databases when it needs more memory, so you typically do not need to use this tag.

It is important to understand that writing all your databases to disk might take several seconds, depending on the size of your databases and the speed of your disk. In most cases, \[commitdatabase] is more appropriate.

### \[addfields] WebDNA v5.0 [context](https://docs.webdna.us/contexts-and-tags)

The \[addfields] context adds new fields to an existing WebDNA database.

```
[addfields db=...]...WebDNA...[/addfields]
```

First, let's use the following code to create a new test database and run a simple search on the new database.

**Example WebDNA code:**

```
[closedatabase db=addfields_test.db]
[writefile file=addfields_test.db]ID  NAME
1      Scott
2      Rusty
3      David
4      Daniel
5      Dustin[/writefile][search db=addfields_test.db&neIDdata=[blank]&rank=off]
  [founditems]
    [ID] - [NAME]<br>
  [/founditems]
[/search]
```

Results:\
1 - Scott\
2 - Rusty\
3 - David\
4 - Daniel\
5 - Dustin

Now, let's use the \[addfields] context to add EMAIL and PHONE fields to the 'addfields\_test.db' database, initializing the new field value with some arbitrary data. We will again perform a simple search on the same database to confirm that the new fields, and field data, have been added.

**Example WebDNA code:**

```
[addfields db=addfields_test.db]EMAIL=us@com&PHONE=12-1235[/addfields][search db=addfields_test.db&neIDdata=[blank]&rank=off]
  [founditems]
    [ID] - [NAME] - [EMAIL] - [PHONE]<br>
  [/founditems]
[/search]
```

Results:\
1 - Scott - us@com - 12-1235\
2 - Rusty - us@com - 12-1235\
3 - David - us@com - 12-1235\
4 - Daniel - us@com - 12-1235\
5 - Dustin - us@com - 12-1235

the \[addfields] context will not add fieldnames that already exist in the target database.

\[addfields] will not add more than 4 fields at one time; in the following example, the **"fifth"** field will not be added

```
[addfields db=test.db]address=[blank]&website=[blank]&brochure=[blank]&Home=[blank]&fifth=[blank][/addfields]
```

\[deletefields] feature has been added in version 8.6\
\[deletefields db=test.db\&fieldnames=column1,column4]

### \[listdatabases] [context](https://docs.webdna.us/contexts-and-tags)

Lists all the currently open databases.

```
[listdatabases] Database Tags [/listdatabases]
```

**The following tags are available inside a \[listdatabases] context:**

\


| Tag            | Description                                                                                                                                                                                                                                                                   |
| -------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \[name]        | the name of the database (can be a partial or full path).                                                                                                                                                                                                                     |
| \[index]       | A number from 1 to the total number of databases, indicating this database's index placement in the list.                                                                                                                                                                     |
| \[numrecords]  | The number of records (rows) in this database.                                                                                                                                                                                                                                |
| \[numfields]   | The number of fields (columns) in this database.                                                                                                                                                                                                                              |
| \[memoryusage] | <p>Woukd show the RAM usage for a specific database.<br>[format thousands 0d][math][memoryusage]/1024[/math][/format]K</p>                                                                                                                                                    |
| \[xxx]         | <p>Any other WebDNA tag or context, including [ListFields]<br>[listfields path=[name]][fieldname], [/listfields]</p>                                                                                                                                                          |
| \[break]       | From version 8.1, if the \[listdatabases] context sees the \[break] tag while executing a loop, it will stop looping, once it finishes the current loop. Thus the \[break] tag should only appear in a \[showif] statement that is evaluated at the end (bottom) of the loop. |

**Example WebDNA code:**

```
[listdatabases]
  Name:[name]<br>
  # Records:[numrecords]<br>
[/listdatabases]
```

\


### \[listfields] [context](https://docs.webdna.us/contexts-and-tags)

Lists all the fields in the specified database, SQLResult record set, or SQL table

```
[listfields Params] Database Tags [/listfields]
```

To display a list of all the fields in a particular database, put a \[listfields] context into a template. You may put a \[listfields] context inside the \[listdatabases] context to automatically list all the fields in all the databases.

**The following tags are available inside a \[listfields] context:**

\


| Tag                                                                              | Description                                                                                                    |
| -------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| \[FieldName]                                                                     | the name of the field.                                                                                         |
| \[index]                                                                         | A number from 1 to the total number of fields, indicating this field's index placement in the list.            |
| **The following tags are available when using \[listfields] to list SQL fields** |                                                                                                                |
| \[ordinal]                                                                       | Will retrieve the literal position of the current iterated field.                                              |
| \[datatype]                                                                      | Will retrieve the named SQL datatype of the current iterated field.                                            |
| \[allownull]                                                                     | Returns whether or not (T or F) the current iterated field will accept a null value as a possible entry/value. |
| \[width]                                                                         | Will retrieve the maximum length of data the current iterated field will allow.                                |

**Example WebDNA code:**

```
[listdatabases]
  Fields in database [name]:<br>
  [listfields db=[name]]
   Fieldname: [fieldname]<br>
  [/listfields]
<hr>
[/listdatabases]
```

To display a list of all fields in an SQL table (after a \[SQLConnect] context), specify the SQL connection and database table.

**Example WebDNA code:**

```
[listfields conn_ref=conn1&table=inventory]
  <b>[fieldname]</b><br>
  ordinal: [ordinal]<br>
  datatype: [datatype]<br>
  allownull: [allownull]<br>
  width: [width]<br>
<hr>
[/listfields]
```

To display a list of all fields in a \[SQLresult] record set, include the \[listfields] inside of a \[SQLresult] context.

**Example WebDNA code:**

```
[SQLresult result_ref=rs1]
The result set from query "select * from employees;" has [numfields] fields:<br>
  [listfields]
    <b>[fieldname]</b><br>
    ordinal: [ordinal]<br>
    datatype: [datatype]<br>
    allownull: [allownull]<br>
    width: [width]
    <hr>
  [/listfields]
[/SQLresult]
```

### \[createdb] WebDNA v8.5 [tag](https://docs.webdna.us/contexts-and-tags)

\[createdb] will create a formatted WebDNA database.

**Parameters**

| Parameter | Description                                                                 |
| --------- | --------------------------------------------------------------------------- |
| name      | The name of the database to create, including the path if required.         |
| fields    | The name of the fields to be added to the database                          |
| overwrite | (optional) T/F defaults to F. Allow an existing database to be overwritten. |

**Example WebDNA code:**

```
[createdb name=/data/newdatabase.db&fields=FROM,TO,MATCH&overwrite=f]
```

### \[dbencrypt] WebDNA v8.6+ [tag](https://docs.webdna.us/contexts-and-tags)

\[dbencrypt] is used to create an encrypted copy of a database using a database as the source.

Encrypted databases are written out by WebDNA, the newly encrypted database file name must end in "\_encrypted.db". WebDNA will write the new file if it does not exist.

**Parameters**

| Parameter | Description                                                                 |
| --------- | --------------------------------------------------------------------------- |
| from      | The name of the database to be copied and the path to the db if required.   |
| to        | The name of the NEW database and the path to the db if required.            |
| overwrite | (optional) T/F defaults to F. Allow an existing database to be overwritten. |

**Example WebDNA code:**

```
[dbencrypt from=/data/original.db&to=/data-encrypted/copy_encrypted.db&overwrite=f]
```

Error message when a db attempts to overwrite a db when overwrite is blank or set to F or f\
File exists and OVERWRITE not set

Encrypted databases are portable, this means that the encrypted databases can be used on another server with a different copy of WebDNA that is version 8.6 or higher.

### \[table] WebDNA v5.0 [context](https://docs.webdna.us/contexts-and-tags)

\[table] allows you to quickly create a temporary 'in line' database that is local to the template and not part of the global database cache. A table can be used in any context that accepts a database 'db' as a parameter (using the syntax table=_tablename_).

With \[table], you define the name of the table and the fields. Between the table tags, you populate the table with tab delimited values, with the record separated by carriage returns, just like a .db file.

**Parameters**

| Parameter | Description                                                               |
| --------- | ------------------------------------------------------------------------- |
| name      | The name used to reference the table during the duration of the template. |
| fields    | A comma delimited, ordered, list of fieldnames to be used for the table.  |

Tables can be static, such as a simple from/to db for a \[convertchars] context or dynamic, such as a file listing. The table allows you to further search and sort a directory of files.

**Example WebDNA code:**

```
[table name=filelisting&fields=fname,fsize,fdate]
[listfiles images/uploads][filename]  [size]  [moddate]
[/listfiles]
[/table]
```

**Example WebDNA code:**

```
[search table=filelisting&nefnamedatarq=[blank]&fsizesort=1]
  [founditems]
    [filename] ([fsize]), [fdate]<br>
  [/founditems]
[/search]
```

You could use these examples to offer three different sort options for the file listing, something \[listfiles] alone can't do.

You can declare a table by using the context without adding records. Then, later on the page, you can use append and replace (and delete, for that matter) to populate the table. Then when you're done with all that, do your search.

Another example to replace each letter of the alphabet with the corresponding number:\
**Example WebDNA code:**

```
[text]alphabet=abcdefghijklmnopqrstuvwxyz[/text]
[table name=t1&fields=from,to]
[listchars chars=[alphabet]]
[char]  [index]
[/listchars]
[/table]
[convertchars table=t1]this is your text[/convertchars]
```

Results: 208919 919 25152118 2052420
