---
description: The built in eCommerce features of WebDNA
---

# eCommerce

### \[cart] [tag](https://docs.webdna.us/contexts-and-tags)

Putting \[cart] in your template automatically creates a unique shopping cart

Putting \[cart] in your template automatically creates a unique shopping cart that can be used in eCommerce. You must continue to pass the \[cart] value from page to page (or store it in a cookie for retrieval) in order to continue writing and modifying the shopping order (using tags such as \[addlineitem] and \[setheader]). If no cart value is specified when the customer lands on a new page with a \[cart] tag, then a new unique value is created displaying an empty cart, and you will lose contact with the previously created cart. (\[cart] will never display in its literal raw form, unless WebDNA processing is turned off altogether.)

The cart is a simple tab-delimited text file with the cart number for its name (no file extension) that you can open on the server and inspect in a text editor. To understand the fieldnames and structure, see the entry for order file.

The cart value is a long number derived from the current time. For this reason, \[cart] is also handy for generating a guaranteed-unique value somewhere, such as a record identifier or product SKU.

### \[orderfile] [context](https://docs.webdna.us/contexts-and-tags)

Displays the contents of a shopping cart.

\[orderfile FilePath]\
orderfile tags\
\[/orderfile]

To display the contents of a cart (either an active shopping cart or a completed order), place an \[orderfile] context into a template. You may display the header fields directly between the \[orderfile] tags, but to see the items (products) themselves, you must put a \[lineitems] context inside. (\[orderfile] with \[lineitems] is very much like \[search] with \[founditems].)

**Example WebDNA code:**

```
[orderfile [cart]]
Name: [name]
Address: [address1]
[address2]
[lineitems]
[lineindex] [quantity] [sku] [price]
[/lineitems]
Grand Total: [grandTotal]
[/orderfile]
```

| Parameter | Description                                                                                                                                                                                                                                                                           |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| cart      | Name of the shopping cart file to use. cart=\[cart] will look inside the ShoppingCarts folder (a preference value lets you set the path to the shopping carts folder) for the order file named \[cart].                                                                               |
| file      | (Optional) Path to the shopping cart file. This is used instead of cart when the file is in a different folder (such as the Orders folder or the Problems folder or some folder of your choosing. Shopping carts can reside anywhere, as long as you tell WebDNA where to find them.) |

See \[setheader] for the field values available inside \[orderfile]. See \[lineitems] for the array of variables available inside that context.

### \[addlineitem] WebDNA v6.2+ [context](https://docs.webdna.us/contexts-and-tags)

Adds a product to the specified shopping cart

\[addlineitem Parameters]values\[/addlineitem]

To add products to a visitor's shopping cart, put an \[addlineitem] context into a template. Whenever WebDNA encounters an \[addlineitem] context, it opens the shopping cart file (creating a new one if necessary) and adds the product (identified by its SKU) to the end of the lineitems in the shopping cart. The item's price, taxable, canemail, and unitshipcost information is found by looking for the values of those fields in the product database. You can use a different price by creating a formulas.db database.

**Example WebDNA code:**

```
[addlineitem cart=5678&sku=1234&db=catalog.db][!]
[/!]quantity=5&[!]
[/!]textA=Red[!]
[/!][/addlineitem]
```

The database "catalog.db" is searched, and sku 1234 is found. Shopping cart file "5678" opens, and a new line item is added to the bottom of the list, with a quantity of 5 and textA set to "Red" (as specified in the context above). The price is taken from the database's price field (or, if a formula for \[price] is available in formulas.db, the price is calculated using that formula).

Note: you may also add line items to order files that are not inside the ShoppingCarts folder. Using file=/folder/folder/cartname instead of cart=cartname, you can affect any order file in any folder.

Note: you may add a maximum of 100 line items to a shopping cart.

Here are the parameters to the \[addlineitem] context:

| Parameter       | Description                                                                                                                                                                                                                                                                                                                                                                                     |
| --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| db              | Product database containing the SKU, price, and other information.                                                                                                                                                                                                                                                                                                                              |
| sku             | Uniquely identifies which product should be added to the cart.                                                                                                                                                                                                                                                                                                                                  |
| cart            | Affected shopping cart file (from ShoppingCarts folder)                                                                                                                                                                                                                                                                                                                                         |
| file            | alternative to cart) Alternate affected shopping cart file (from any folder). Unlike cart, this file can be in any folder. Specify the file URL-relative to the template.                                                                                                                                                                                                                       |
| Context values  | Description (these values go inside the context)                                                                                                                                                                                                                                                                                                                                                |
| password        | (Optional) In order to change the price (see "price") you must provide the lineitem change password, which can be set in the preferences.                                                                                                                                                                                                                                                       |
| price           | (Optional) Sometimes you may need to change the price of a product while adding it to the cart. Normally you use a formula to vary pricing, but sometimes this alternate technique is preferred. Remember to put the line item change password into the parameters. There is a slight security risk when using this technique, as a hacker could create a URL containing their own price values |
| textA           | (Optional) Extra information of any kind that you want associated with this line item. Often used to store extra product information, such as "shoe size" or "color." Also used to pass catalog database fields such as \[title] through to the order file so they may be viewed later without needing the original database to look for the value of \[title].                                 |
| textB ... textZ | Same as textA above.                                                                                                                                                                                                                                                                                                                                                                            |
| quantity        | (Optional) Indicates quantity of this item. This quantity is used when calculating subtotals, unitshipcost, etc.                                                                                                                                                                                                                                                                                |
| taxable         | (Optional) "T" or "F". Overrides "taxable" field in the database - normally the information about the item's taxable status is taken from a field called "taxable."                                                                                                                                                                                                                             |
| canemail        | (Optional) "T" or "F". Overrides "canemail" field in the database - normally the information about the item's canemail (electronically deliverable) status is taken from a field called "canemail."                                                                                                                                                                                             |
| unitshipcost    | (Optional) A number indicating the item's price for shipping. Overrides "unitshipcost" field in the database - normally the information about the item's unitshipcost status is taken from a field called "unitshipcost." shiptotal and grandtotal use this number (multiplied by quantity) to determine the total shipping and grand total.                                                    |
| Header Field    | You may set any shopping cart header field (such as name, taxrate, address1, etc.) at the same time you add a product to the cart. Header fieldnames are different from the line item fieldnames, and WebDNA will know they are header fields.                                                                                                                                                  |

### \[lineitems] [context](https://docs.webdna.us/contexts-and-tags)

Loops through all the line items in an order file.

\[orderfile \[cart]]\
\[lineitems]\
lineitem variables\
\[/lineitems]\
\[/orderfile]

To display a list of all the line items in a shopping cart or order file, put a \[lineitems] context inside an \[orderfile] context (like placing a founditems loop inside a search context).

**Example WebDNA code:**

```
[orderfile [cart]]
[lineitems]
[sku], [price], [quantity]
[/lineitems]
[/orderfile]
```

The following tags are available inside a \[lineitems] context. Except for \[lineindex], these are the line item fieldnames in the order file.

| Tag                   | Description                                                                                                           |
| --------------------- | --------------------------------------------------------------------------------------------------------------------- |
|                       |                                                                                                                       |
| \[lineindex]          | A number from 1 to the maximum number of line items, indicating this item's index position in the list.               |
| \[SKU]                | The SKU of this line item in the shopping cart.                                                                       |
| \[quantity]           | Quantity of this line item the visitor wants to purchase.                                                             |
| \[price]              | Price of this line item.                                                                                              |
| \[taxable]            | "T" if this item is taxable, "F" if not.                                                                              |
| \[canemail]           | "T" if this item is electronically deliverable, "F" if not.                                                           |
| \[unitshipcost]       | Price to ship one unit of this item.                                                                                  |
| \[textA]              | Extra text field to be used for any purpose, e.g., catalog fields such as \[title] or options such as color and size. |
| \[textB] ... \[textZ] | Same as textA above.                                                                                                  |



### \[clearlineitems] [tag](https://docs.webdna.us/contexts-and-tags)

Remove all line items from the specified shopping cart

```
[clearlineitems cart=[cart]]
```

Putting \[clearlineitems] in your template will remove all line items from the specified shopping cart. Normally carts are found inside the ShoppingCarts folder, but you may specify a cart in any folder by using file=/folder/\[cart] instead of cart=\[cart].

### \[removelineitem] [tag](https://docs.webdna.us/contexts-and-tags)

\[removelineitem cart=cartID\&index=3]

Putting \[removelineitem] in your template immediately deletes the specified line item from the specified shopping cart file. See also \[addlineitem]. If the cart file is not in a ShoppingCarts folder, you may use the alternate form file=/folder/cartID instead of cart=cartID.

Take caution when removing multiple items. As you remove items, the index numbering changes. When you remove item number 2, the subsequent items move up one. For this reason, you need to figure out a way to adjust the index number. Decrement the index each time something's deleted, such as the following:

(In this example, in the incoming form the cart items each have a delete checkbox named kill\_\[index]. Note that in a math context, you do not need the square brackets within the expression.)

```
[math show=f]adjustby=0[/math][formvariables name=kill_&exact=F]
[math show=f]deleteitemnumber=[getchars 
start=6][name][/getchars]+adjustby[/math][removelineitem file=ShoppingCarts/[cart]&index=[deleteitemnumber]]
[math show=f]adjustby=adjustby-1[/math][/formvariables]
```

### \[setheader] [context](https://docs.webdna.us/contexts-and-tags)

Changes header values in a shopping cart.

\[setheader Parameters]values\[/setheader]

To change header values in a visitor's shopping cart, place a \[setheader] context into a template. Whenever WebDNA encounters a setheader context, it opens the shopping cart file and changes values in the headers you specify. The header fields of a shopping cart hold all the information for the order _except_ the specific items. The header contains all customer and payment information, as well as tax rate. The header also contains fields for credit card authorization codes returned once the card is processed.

There are a vast number of named header fields matching commonly used data, as well as 40 generic fields. Refer to the order file section for a list of all available header fields.

Example:

```
[setheader cart=5678&db=catalog.txt]ShipToZip=92128&Header9=Blue[/setheader]
```

Shopping cart file "5678" opens, and ShipToZip changes to 92128 and Header9 changes to "Blue" (as specified in the context above).

Here are the parameters to the \[setheader] context:

| Parameter      | Description                                                                                                                                                                                |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| db             | Product database containing the SKU, price, and other information.                                                                                                                         |
| cart           | Affected shopping cart file (from ShoppingCarts folder).                                                                                                                                   |
| file           | (alternative to **cart**) Affected shopping cart file (from any folder). Unlike **cart**, this file can be in any folder. Specify the file URL-relative to the template.                   |
| Context value  | Description (these values go inside the context)                                                                                                                                           |
| _Header Field_ | Any header field. See order file for the list of available header fields. You may also set any header field at the same time you add or modify a product to the cart (\[addlineitem] etc). |

### \[setlineitem] [context](https://docs.webdna.us/contexts-and-tags)

Modify an existing line item in an order file

\[setlineitem Parameters]values\[/setlineitem]

To change a line item in a visitor's shopping cart, put a \[setlineitem] context into a template. Whenever WebDNA encounters a \[setlineitem] context, it opens the shopping cart file and changes values in a line item (identified by its index). The item's quantity, textA-Z, and cart header fields are all changeable.

Example:

```
[setlineitem cart=5678&db=catalog.db]quantity=4&textA=Blue[/setlineitem]
```

Shopping cart file "5678" opens, and line item 3's quantity changes to 4 and textA changes to "Blue" (as specified in the context above).

Here are the parameters to the \[setlineitem] context:

| Parameter        | Description                                                                                                                                                                                                                                                                                                                                                    |
| ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| db               | Product database containing the SKU, price, and other information.                                                                                                                                                                                                                                                                                             |
| index            | Uniquely identifies which line item should be modified.                                                                                                                                                                                                                                                                                                        |
| cart             | Affected shopping cart file (from ShoppingCarts folder).                                                                                                                                                                                                                                                                                                       |
| file             | (alternative to **cart**) Affected shopping cart file (from any folder). Unlike **cart**, this file can be in any folder. Specify the file URL-relative to the template.                                                                                                                                                                                       |
| Context value    | Description (these values go inside the context)                                                                                                                                                                                                                                                                                                               |
| password         | (Optional) In order to change the price (see below) you must provide the line item change password, which can be set in the preferences.                                                                                                                                                                                                                       |
| price            | Optional) Sometimes you may need to change the price of a product after it has already been added to the cart. Normally you use a formula to vary pricing, but since formulas only apply when a product is initially added to the cart, this alternate technique may be needed. Remember to put the line item change password (see above) into the parameters. |
| _Lineitem Field_ | Any line item field. See order file for the list of available line item fields. Values set in the context will override values pulled from the product database.                                                                                                                                                                                               |
| _Header Field_   | Any header field may also be modified at this time. See order file for the list of available header fields.                                                                                                                                                                                                                                                    |

### \[validcard] [tag](https://docs.webdna.us/contexts-and-tags)

Checks legitimacy of credit card

Putting \[validcard] into a template displays "T" or "F", depending on the value of the number in accountNum. If the accountNum is a legitimate credit card number for the specified card type (according to the algorithm used to create such numbers), then "T" indicates it is good. Conversely, "F" indicates it is bad. This does not call the bank to verify funds on the card -- it merely does a simple numeric checksum to verify the number is consistent with a credit card number, and not a randomly punched in string of numbers. You would use this tag to validate the credit card field on your order form BEFORE writing it to the shopping cart order file. If it evaluates to F, you can display an alert and ask the customer to please double-check the credit card before continuing.

Example:

```
[validcard accountNum=cardNumber&card=VISA+MC]
```

| Parameter  | Description                                                                                                                                                                                                                                                                          |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| accountNum | (Required) The credit card number. Any extra spaces or non-numeric characters will be ignored.                                                                                                                                                                                       |
| card       | (Optional) Set card to the names of the credit cards that are allowed, separated by space or +. Possible values are ALL, IGNORE, VISA, MC, AMEX, DISC, JCB, DINER. If not specified, then ALL cards are allowed. IGNORE tells it to always pass the card regardless of its checksum. |

### \[purchase] [tag](https://docs.webdna.us/contexts-and-tags)

\[purchase cart=\[cart]]

Putting \[purchase cart=\[cart]] in your template moves the specified shopping cart file from the ShoppingCarts folder to the Orders folder. If the cart file is not in a ShoppingCarts folder, you may use the alternate file=/folder/\[cart] instead of cart=\[cart].
