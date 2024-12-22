# Text variables

### \[text] WebDNA v3.0+ [context](https://docs.webdna.us/contexts-and-tags)

Text variable names are limited to a MAXIMUM of 49 characters.\
Text variables are fundamental building blocks of the WebDNA language. They are simple to understand and easy to weave into your code.

```
[text]variablename=value[/text]
```

**Parameters**

| **Parameters** | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| multi          | (optional) "T" or "F". Allows you to assign more than one text variable in a single context. \[text multi=T]var1=Joe\&var2=Fred\[/text] simultaneously assigns the two variables. (There is no need to use multi=f for single variables.)                                                                                                                                                                                                                                                                                                      |
| show           | (optional) "T" or "F". Default behavior is to hide the value when assigning to a text variable (the opposite of a math variable). If you want the value to be shown at the same time it is assigned to a variable, you may set Show=T. But if you set multi=T at the same time, then no values will be displayed. (There is no reason to ever use show=f, as this is default behavior.)                                                                                                                                                        |
| secure         | (optional) "T" or "F". Default is "T". Setting secure=F makes the text variable overrideable by incoming form variables. This is not recommended, and the default behavior is secure. Sometimes you might want a form on the preceding page to override an embedded variable, and in this case, you'd use secure=f for the embedded text contect. However, this also makes it possible for a visitor to type in the name=value pair in the URL, which would override the embedded one. So use this with caution, only if no harm could result. |

One valuable way of using a text variable is to set the value of something repetitive, so you don't have to do it over and over on the page. This way, there is only one place to edit.

**Example WebDNA code:**

```
[text]maincolor=#660000[/text]
[text]secondcolor=#eebb21[/text]
[text]thirdcolor=#f0e2b8[/text]
```

Retrieve the variable value by "calling" the variable name

```
[secondcolor]
Will display #eebb21 on the page
```

**Text variable names are limited to a MAXIMUM of 49 characters.**

The use of \[text multi=t] provides the ability to set more than one variable in the one statement.

**Example WebDNA code:**

```
[text multi=t]maincolor=#660000&secondcolor=#eebb21&thirdcolor=#f0e2b8[/text]
```

Text variables can be numbers, and they can be used in math equations. A number is still a number, as long as it isn't tainted with non-numerical characters i.e. 24a will not work as a number.

WebDNA expects anything after the = to be part of the set variable. If the value also contains an =, everything after the FIRST = will be returned in the value when the variable is called eg: db=somedb.db\&eqsomefielddatarq=value\&somefieldsort=1.\
Using \[text multi=t] in this situation will fail.

If a text variable and a math variable have the same name, the text variable will override the math variable.

There are a few _reserved_ scope names:\
global - refers to the normal/secure template variable space.\
local - When used inside of a function or scope context, refers to the variable space associated with the current function or scope.\
insecure - Refers to the insecure template variable space, this space also includes HTML form variables
