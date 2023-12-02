---
description: SQL & MySQL database connections
---

# SQL

### \[SQLconnect] WebDNA v6.0 - 6.2 [context](https://docs.webdna.us/contexts-and-tags)

Opens and persists a 'named' connection to a SQL server, for the durration of the template.

\[SQLConnect Params]...\[/SQLConnect]

Generally speaking, the first step when using the SQL contexts is to make a connection to your database server. This is done with the \[SQLConnect] context.

**Example WebDNA code:**

```
[SQLConnect dbType=MySQL&host=192.168.1.1&database=base&uid=sa&pwd=pass&conn_var=conn1]
<table>
<tr><td>Host:</td><td>[SQL_HOST]</td></tr>
<tr><td>Server Version:</td><td>[SQL_SERVERVER]</td></tr>
<tr><td>Client Version:</td><td>[SQL_CLIENTVER]</td></tr>
</table>
[/SQLConnect]
```

| Parameter                  | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| dbtype                     | (Optional) - Can be one of three reserved values - MYSQL, MSSQL, or ORACLE - depending on the type of RDBMS you wish to connect to.If 'dbType' is not given, WebDNA defaults to MYSQL. For WebDNA 6.0, MYSQL is the only type supported.                                                                                                                                                                                                                                 |
| host                       | (Required) - Either the IP Address or the host name of the database server you wish to connect to.                                                                                                                                                                                                                                                                                                                                                                       |
| port                       | (Optional) - The non-standard/non-default port of the database host you wish to connect to; do not provide a value unless your database administrator has informed you that the you must use a specific port in order to connect to the server.                                                                                                                                                                                                                          |
| database                   | (Optional) - The name of the database to which you wish to connect; almost all the time you will want to provide a database name to connect to; if you do not supply the "database" parameter, the engine assumes you wish to connect to the RDBMS as admin in order to execute admin-type tasks, such as creating tables, etc.; please note that in this case you will more than likely only be required to provide admin credentials in order to connect successfully. |
| uid                        | (Optional) - The username credential you want to use to connect to the SQL server.                                                                                                                                                                                                                                                                                                                                                                                       |
| pwd                        | (Optional _unless uid is used_) - The password credential you want to use to connect to the SQL server.                                                                                                                                                                                                                                                                                                                                                                  |
| conn\_var ( or just 'var') | (Required) - the name of the connection variable that will be created on successful connection.                                                                                                                                                                                                                                                                                                                                                                          |

The following tags are available inside a \[SQL] context:

| Tag                | Description                                                                    |
| ------------------ | ------------------------------------------------------------------------------ |
| \[SQL\_HOST]       | The IP Address/host name of the database server.                               |
| \[SQL\_SERVERTYPE] | The type of the database server; types returned: MYSQL, MSSQL, or ORACLE.      |
| \[SQL\_SERVERVER]  | The SQL database server version number.                                        |
| \[SQL\_CLIENTVER]  | The version number of the client being used to connect to the database server. |
| \[SQL\_DBNAME]     | The name of the database you have connected to, if any                         |

### \[SQLinfo] WebDNA v6.0 - 6.2 [context](https://docs.webdna.us/contexts-and-tags)

\[SQLinfo Params]...\[/SQLinfo]\
Reports back information about a SQL connection that has already been established via \[SQLconnect].

**Example WebDNA code:**

```
[SQLconnect dbType=MySQL&host=192.168.1.1&database=base&uid=sa&pwd=pass&conn_var=conn1]
Connected successfully
[/SQLConnect]SQLinfo reports:
[SQLinfo conn_ref=conn1]
Host: [SQL_HOST]
Server Version: [SQL_SERVERVER]
Client Version: [SQL_CLIENTVER]
[/SQLinfo]
```

| Parameter                 | Description                                                                                                                 |
| ------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| conn\_ref (or just 'ref') | The name of a SQLConnect variable created (via the conn\_var or var variable) by the successful execution of \[SQLconnect]. |

The following tags are available inside a \[SQLinfo] context:

| Tag                | Description                                                                    |
| ------------------ | ------------------------------------------------------------------------------ |
| \[SQL\_HOST]       | The IP Address/host name of the database server.                               |
| \[SQL\_SERVERTYPE] | The RDBMS type of the database server; type returned: MYSQL, MSSQL, or ORACLE. |
| \[SQL\_SERVERVER]  | The version number of the SQL server; e.g. 7.0.                                |
| \[SQL\_CLIENTVER]  | The version number of the client being used to connect to the database server. |
| \[SQL\_DBNAME]     | The name of the database you have connected to, if any.                        |

### \[SQLexecute] WebDNA v6.0 - 6.2 [context](https://docs.webdna.us/contexts-and-tags)

Executes and persits the results of an SQL statement.

\[SQLExecute Params]WebDNA to evaluate to a single SQL statement\[/SQLExecute]

Once you have made a successfull connection to a SQL server, using the \[SQLconnect] context, you can use the \[SQLexecute] context to execute all manner of native SQL commands and to create result sets from SQL queries ("select" statements).

**Example WebDNA code:**

```
[SQLconnect dbType=MySQL&host=192.168.1.1&database=base&uid=sa&pwd=pass&conn_var=conn1]
Connected successfully
[/SQLconnect][SQLexecute conn_ref=conn1&result_var=rs1]
select firstName,lastName from employees;
[/SQLexecute]
```

\[SQLexecute] expects its content to resolve or evaluate to a single SQL command of some sort. You can place WebDNA here if you wish but \[SQLexecute] will fail if its contents are not limited to a valid SQL statement after the inner parse is complete. Refer to the documentation of your particular SQL server software to determine what a "valid" SQL statement is.

After a successfull execution, you would then normally use the \[SQLresult] and \[founditems] contexts to retrieve the results of the SQL statement.

| Parameter                   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| conn\_ref (or just 'ref')   | (Required) - The name of the connection variable you created with a prior execution of \[SQLconnect] (the value you set for the "conn\_var" or "var" parameter)                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| result\_var (or just 'var') | (Optional) - The name of the SQLresult variable you wish to create on successful execution. This 'result' variable can be used later by setting the "result\_ref" (or "ref") parameter of \[SQLresult] to this value. If you do not specify a variable name, none will be created and it is assumed you wish to execute non-Select SQL commands. Even if you do execute a non-Select command, you may still want to 'save' the result. For example, if you execute a delete command, you can still specify a result variable, so that you can access other information, such as the number of rows affected. |

### \[SQLresult] WebDNA v6.0 - 6.2 [context](https://docs.webdna.us/contexts-and-tags)

\[SQLresult] is used to display the result of a \[SQLexecute]

**Parameters**

| Parameter                            | Description                                                                                                                                                      |
| ------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>result_ref<br>(or just 'ref')</p> | (Required) - The name of the SQLResult variable you created with a prior execution of \[SQLexecute] (the value you set for the "result\_var" or "var" parameter) |

The following tags are available inside a \[SQLresult] context:

| Tag                            | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \[commandtext]                 | The SQL command string that was executed via the \[SQLexecute] context.                                                                                                                                                                                                                                                                                                                                                                                                                     |
| \[numfound]                    | A number indicating how many records were returned as the result of the SQL statement (a 'select' statement in most cases). Some SQL statements will not result in a record set, i.e. DELETE, INSERT, DROP, etc... In these cases \[numfound] will be zero.                                                                                                                                                                                                                                 |
| \[numfields]                   | A number indicating the number of fields in the returned record set.                                                                                                                                                                                                                                                                                                                                                                                                                        |
| \[numrowsaffected]             | The number of rows changed by an INSERT/UPDATE/DELETE command.                                                                                                                                                                                                                                                                                                                                                                                                                              |
| \[insertID]                    | The insert ID of a successfull INSERT command.                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| \[founditems]...\[/founditems] | <p>Normally you put a [founditems] loop inside a [SQLresult] context to retrieve the records resulting from a SQL SELECT statement, so you can display all the matching records. You can put any record set field name inside the [founditems] loop to display them in HTML.There are other SQL commands that will return a records set. For example, in MySQL, the following statements will return a record set:<br>"show tables;"<br>"show processlist;"<br>"describe &#x3C;table>;"</p> |

**Example WebDNA code:**

```
[SQLconnect dbType=MySQL&host=192.168.1.1&database=base&uid=sa&pwd=pass&conn_var=conn1]
  Connected successfully
[/SQLconnect][SQLexecute conn_ref=conn1&result_var=rs1]
  select firstName,lastName from employees;
[/SQLexecute][SQLresult result_ref=rs1]
  [numfound] records found<br>
  <table border=1><tr><th>First Name</th><th>Last Name</th></tr>
    [founditems]
      <tr><td>[firstName]</td><td>[lastName]</td></tr>
    [/founditems]
  </table>
[/SQLresult]
```

It may sometimes be the case when any or all of the db fields are to be returned:\
Executing 'Select \* from mytable' will pull all field values into the record set. If the db fields are unknown, \[listfields] will return the field names from the data set.

**Example WebDNA code:**

```
[SQLconnect dbType=MySQL&host=192.168.1.1&database=base&uid=sa&pwd=pass&conn_var=conn1]
[/SQLconnect][SQLexecute conn_ref=conn1&result_var=rs1]
  select * from employees;
[/SQLexecute][SQLresult result_ref=rs1]
  [numfound] records found<br>
  <table border=1>
    <tr>
      [listfields]<th>[fieldname]</th>[/listfields]
    </tr>
    [founditems]
    <tr>
      [listfields]<td>[interpret][[fieldname]][/interpret]</td>[/listfields]
    </tr>
    [/founditems]
  </table>
[/SQLresult]
```

You can also use \[field], inside \[founditems], to retrieve field data by the fields position in the records set.

**Example WebDNA code:**

```
[SQLconnect dbType=MySQL&host=192.168.1.1&database=base&uid=sa&pwd=pass&conn_var=conn1]
[/SQLconnect][SQLexecute conn_ref=conn1&result_var=rs1]
  select * from employees; 
[/SQLexecute][SQLresult result_ref=rs1]
  [founditems]
    [loop start=1&end=[numfields]]
      [field seek=ordinal:[index]&get=NAME]: <b>[field seek=ordinal:[index]&get=VALUE]</b> [hideif [index]=[numfields]]- [/hideif]
    [/loop]<br>
  [/founditems]
[/SQLresult]
```

### \[SQLrelease] WebDNA v6.0 - 6.2 [tag](https://docs.webdna.us/contexts-and-tags)

\[SQLrelease result\_ref=]

\[SQLrelease] will close the result set and variable name created by a \[SQLexecute] context. The name is freed and available for use again.\
All SQL result objects are released at the end of template execution anyway, so it is not required to use \[SQLrelease]. However, it can come in handy if you wish to iterate several \[SQLexecute] contexts, re-using the same result variable name for each SQL execution.

| Parameters                | Description                                                                                                                                                               |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| conn\_ref (or just 'ref') | (Required) - The name of the SQLConnect variable you have created via the conn\_var (or var) parameter of a \[SQLconnect] context you have already successfully executed. |

### \[SQLdisconnect] WebDNA v6.0 - 6.2 [tag](https://docs.webdna.us/contexts-and-tags)

\[SQLdisconnect conn\_ref=]

\[SQLdisconnect] will close the database connection, releasing any resources (including any and all SQLResult variables) associated with the connection, and the \[SQLConnect] variable name is freed and available for use again.\
All SQL connections are terminated at the end of template execution anyway, so it is not required to use \[SQLdisconnect]. However, it can come in handy if you wish to iterate several connections, re-using the same connection variable name for each connection.

| Parameters                | Description                                                                                                                                                               |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| conn\_ref (or just 'ref') | (Required) - The name of the SQLConnect variable you have created via the conn\_var (or var) parameter of a \[SQLconnect] context you have already successfully executed. |

### \[SQL] WebDNA v3.0+ [context](https://docs.webdna.us/contexts-and-tags)

Performs a SQL statement on an ODBC data source, this is a separate method to \[SQLconnect].\
SQL is used to perform a query on a SQL type database through an ODBC connection.\
You may specify any dsn (Data Source Name) that has been properly configured through the ODBC setup control panel on the web server computer.

\[SQL Statement]query results\[/SQL]

The \[SQL] context is not limited to searching -- you may perform any legal SQL statement, such as SELECT, INSERT, DROP, etc. The SQL language is too broad to describe here; it is assumed you have a working knowledge of SQL before using this context.

**Example WebDNA code:**

```
[SQL dsn=Pubs&statement=SELECT * from Authors]
Found [numfound] items
[founditems]
[au_lname], [au_fname], [title]
[/founditems]
[/SQL]
```

Whenever WebDNA encounters a \[SQL] context, it uses ODBC to attempt to make a connection to the DSN you specify. It then executes the SQL statement and retrieves the results, if any. For SQL SELECT statements, you almost always put a \[founditems]...\[/founditems] context inside the \[SQL] context so you can display the information from the matching records.

you can substitute any \[xxx] tags in the SQL parameters, as in \[SQL dsn=Pubs\&statement=SELECT \* from Authors where au\_lname > '\[form.name]'].

UNIX users can get infomation on how to setup WebDNA with ODBC

| Parameter | Description                                                                                                                    |
| --------- | ------------------------------------------------------------------------------------------------------------------------------ |
| dsn       | Name of the database you wish to search.                                                                                       |
| username  | Username required to access this ODBC database (Optional for Window platform, if none specified, "sa" is used).                |
| password  | Password required to access this ODBC database (Optional for Window platform, if none is specified, a blank password is used). |
| statement | Any legal SQL statement.                                                                                                       |
| max       | A number indicating how many records should be displayed at once before showing a list of "Show Items xx-yy" hyperlinks.       |

The following tags are available inside a \[SQL] context:

| Tag                            | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| \[NumFound]                    | A number indicating how many records matched the search request. Some ODBC drivers do not support this feature, so WebDNA compensates by visiting every record in the database in order to count them. For large datasets, this can be very slow, and you should consider writing a SQL statement that performs a count instead. Do **not** put \[NumFound] inside a SQL context that INSERTS new records, because the statement will be executed twice in order to perform the count. This will cause invalid a second record to be added to your database. |
| \[founditems]...\[/founditems] | Normally you put a [\[founditems\]](https://docs.webdna.us/founditemsContext.html) loop inside a \[SQL] context that has performed a SELECT statement, so you can display all the matching records. You can put any database field names inside the \[founditems] loop to display them in HTML.                                                                                                                                                                                                                                                              |
