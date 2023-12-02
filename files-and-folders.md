# FIles & Folders

### \[writefile] [context](https://docs.webdna.us/contexts-and-tags)

\[writefile] functions allows you to perform a wide variety of tasks, from copying a purchased eBook to a special folder for downloading, to placing an uploaded file on your server.

```
[writefile file=saved_files/new_filename.txt&secure=f]
  Write this info into new_filename.txt
[/writefile]
```

Text is written exactly as in the template; it is not interpreted by the browser. Tabs, returns and multiple spaces will be written as is. However, the text WILL be interpreted by WebDNA first, so you may use variables or any WebDNA code necessary to create your text.

**Parameters**

| Parameter | Description                                                                                             |
| --------- | ------------------------------------------------------------------------------------------------------- |
| path      | The path to where the file is to be written, including the new file's name                              |
| secure    | (optional) t or f - the file will be written using the permissions preference set in WebDNA preferences |

If a file by the same name exists, \[writefile] will overwrite it. \[writefile] is also used to handle uploaded files, such as jpgs, pdf, or any other format.

**Example WebDNA code:**

```
[writefile file=saved_files/filename-[date %Y%m%d].txt&secure=f]
  Write this info into the new file and the date is [date] at [time].
  The browser that was used to create this file was [browsername]
[/writefile]
```

The text file "filename-2021-09-03.txt" is created inside the folder "saved\_files" and the text is inserted:

_Write this info into the new file and the date is 09/03/2022 at 16:45:33._\
_The browser that was used to create this file was Mozilla/5.0 (Macintosh; Intel Mac OS X 10\_15\_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/14.1.2 Safari/605.1.15_

Notice that any WebDNA \[xxx] tags inside the \[writefile]\[/writefile] context are substituted for their real values before being written to the file. You may specify a full or partial URL to the file, as in "/some-folder/file.txt" or "local-folder/file.txt."

Static webpages or even dynamic pages can be generated if \<!--HAS\_WEBDNA\_TAGS--> is added at the begining of the file. WebDNA can literally autogenerate new programs. See \[raw] context.

**See also:**\
\[appendfile], \[copyfile], \[movefile], \[deletefile].

### \[copyfile] [tag](https://docs.webdna.us/contexts-and-tags)

Copy a file from one location to another within a website.

```
[copyfile from=folder_one/file_one.txt&to=folder_two/file_two.txt]
```

\


**Parameters**

| Parameter | Description                                                                                      |
| --------- | ------------------------------------------------------------------------------------------------ |
| from      | The path to where the existing file is, including the file's name                                |
| to        | The path to where the file is to be written/copied to, including the file's existing or new name |
| secure    | v8.6.5+ (optional) T (true) or F (false). If left unspecified then false is assumed.             |

\[copyfile] can be used to copy any type of file.

### \[movefile] [tag](https://docs.webdna.us/contexts-and-tags)

\[movefile] moves the file to a new location, and then deletes the original once the file is safely written to disk in the new loaction. If a file by the same name already exists, \[movefile] will overwrite it. You may specify a different folder for the file to be moved to.

```
[movefile from=folder_one/file_one.txt&to=folder_two/file_two.txt]
```

\


**Parameters**

| Parameter | Description                                                                              |
| --------- | ---------------------------------------------------------------------------------------- |
| from      | The path to where the file is, including the file's name                                 |
| to        | The path to where the file is to be copied to, including the file's existing or new name |

\[movefile] can be used to move any type of file.

**See also:**\
\[appendfile], \[copyfile], \[writefile], \[deletefile].

### \[deletefile] [tag](https://docs.webdna.us/contexts-and-tags)

\[deletefile] will permanently delete a file from your webserver.

```
[deletefile file=folder_one/delete_this.txt]
```

\


**Parameters**

| Parameter | Description                                                          |
| --------- | -------------------------------------------------------------------- |
| file      | The path to the file that is to be deleted, including the file name. |

Putting \[deletefile file=filename.txt] in a template immediately deletes the file named "filename.txt". Paths are relative to the template, so "somefolder/filename.txt" deletes the file down a level from the template inside a folder named "somefolder," while "../filename.txt" deletes the file in the folder one level up from the template.

This operation can not be reversed, the file is un-recoverable

### \[appendfile] [context](https://docs.webdna.us/contexts-and-tags)

To add text to the end of an arbitrary text file, put the new text within an \[appendfile] context.

```
[appendfile file=selected_file.txt]This text will be added to the end of the file[/appendfile]
```

\[appendfile] creates a new file if one does not exist already. All text is put at the end of the file, after any text that may already be there.

Carriage returns inside the context are written to the file exactly as they appear. Any WebDNA \[xxx] tags inside the context are substituted for their real values **before** being written to the file. You may specify a full or partial URL to the file, as in "/some-folder/file.txt" or "local-folder/file.txt."

The file must not be a database file that WebDNA currently has open.

### \[createfolder] [tag](https://docs.webdna.us/contexts-and-tags)

Create an empty folder on your webspace

```
[createfolder path=images/home_page]
```

A new folder "home\_page" is created inside the folder "images".

**Parameters**

| Parameter | Description                                                                |
| --------- | -------------------------------------------------------------------------- |
| path      | Path to where the folder is to be created, including the new folder's name |

\[createfolder] will overwrite an existing folder of the same name already on the disk, and all the files within; use with caution.

### \[copyfolder] [tag](https://docs.webdna.us/contexts-and-tags)

Copy a folder and all its content on your web server.

```
[copyfolder from=sales2021&to=archives/sales2021]
```

Using \[copyfolder] immediately copies the folder named sales2021 into another folder named archives.

**Parameters**

| Parameter | Description                                                                                  |
| --------- | -------------------------------------------------------------------------------------------- |
| from      | The path to where the existing folder is, including the folder's name                        |
| to        | The path to where the folder is to be copied to, including the folder's existing or new name |

\[copyfolder] can be used to rename the resulting folder.

**Example WebDNA code:**

```
[copyfolder from=allrecords&to=records2021]
```

\[copyfolder], will overwrite an existing folder of the same name already on the disk, and all the files within; use with caution.\


### \[deletefolder] [tag](https://docs.webdna.us/contexts-and-tags)

\[deletefolder] will permanently delete a folder from your webserver.

```
[deletefolder path=archives/oldorders]
```

The folder "oldorders" is deleted from the archives folder.

**Parameters**

| Parameter | Description                                                              |
| --------- | ------------------------------------------------------------------------ |
| path      | The path to the folder that is to be deleted, including the folder name. |

All files inside the folder are deleted, and all subfolders are deleted. This operation is un-recoverable

### \[listfiles] [context](https://docs.webdna.us/contexts-and-tags)

The \[listfiles] context will display a list of all the files & folders within a folder.

**Parameters**

| Parameter      | Description                                                                                                                                 |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| path           | (required) The path to the folder which is to be listed. This path is relative to the template.                                             |
| ShowInvisibles | (optional) Apple platform only: if set to T, then display invisible filenames, if set to F, do not display invisible filenames              |
| Name           | (optional) filters files by name                                                                                                            |
| Exact          | (optional, used with Name) if set to T, then display filenames that match 'Name' criteria exactly; if set to F, a partial match can be made |

**Tags available inside the \[listfiles] context**

| Tag           | Description                                                                                                                                                                                                                                                               |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \[isfolder]   | "T" if this item is a folder. "F" otherwise.                                                                                                                                                                                                                              |
| \[isfile]     | "T" if this item is a file. "F" otherwise.                                                                                                                                                                                                                                |
| \[filename]   | Name of the file.                                                                                                                                                                                                                                                         |
| \[fullpath]   | Full path to the file, starting from its containing FolderPath.                                                                                                                                                                                                           |
| \[moddate]    | Last date the file was modified.                                                                                                                                                                                                                                          |
| \[modtime]    | Last time the file was modified.                                                                                                                                                                                                                                          |
| \[createdate] | Date the file was created.                                                                                                                                                                                                                                                |
| \[createtime] | Time the file was created.                                                                                                                                                                                                                                                |
| \[size]       | The file's size, in bytes.                                                                                                                                                                                                                                                |
| \[index]      | The file's position in the list                                                                                                                                                                                                                                           |
| \[startpath]  | The folder path leading to the file.                                                                                                                                                                                                                                      |
| \[break]      | From version 8.1, if the \[listfiles] context sees the \[break] tag while executing a loop, it will stop looping, once it finishes the current loop. Thus the \[break] tag should only appear in a \[showif] statement that is evaluated at the end (bottom) of the loop. |

**Example WebDNA code:**\
Use the "Name" and "Exact" parameters with the \[listfiles] context to "filter" the results inside a \[listfiles] context.\
If the current folder contains the files:\
file1.txt\
file2.txt\
file2.txt

```
[listfiles path=/&name=file1&exact=f]
  [filename]
[/listfiles]
```

Results: file1.txt

**Example WebDNA code:**\
List all the .png & .jpg files in a folder named "images":\
file1.txt\
file2.png\
file3.txt\
file4.gif\
file5.jpg

```
[listfiles path=images/]
  [showif [isfile]=T]
    [if ("[filename]" ^ ".png")|("[filename]" ^ ".jpg")]
      [then]
        [filename]
      [/then]
      [else][/else]
    [/if]
  [/showif]
[/listfiles]
```

Results:\
file2.png\
file5.jpg

There is no facility for sorting the file list within the \[listfiles] context. This is determined by the server. One way to sort is to capture the results in a \[table] context, then search and sort the table as desired.

### \[fileinfo] [context](https://docs.webdna.us/contexts-and-tags)

\[fileinfo] will display information about a nominated file or folder.

```
[fileinfo file=archives/sales2021.txt][/fileinfo]
```

**Parameters**

| Parameter | Description                                                                                |
| --------- | ------------------------------------------------------------------------------------------ |
| file      | The path to the file/folder which is to be queried. This path is relative to the template. |

**The following tags are available inside a \[fileinfo] context:**

| Tag            | Description                                                                                                                                                                                                                                                         |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \[isfolder]    | "T" if this item is a folder. If it is a file the result will be "F".                                                                                                                                                                                               |
| \[isfile]      | "T" if this item is a file. If it is a foler the result will be "F".                                                                                                                                                                                                |
| \[exists]      | "T" if this file/folder actually exists on disk.                                                                                                                                                                                                                    |
| \[filename]    | Name of the file.                                                                                                                                                                                                                                                   |
| \[fullpath]    | Full path to the file.                                                                                                                                                                                                                                              |
| \[moddate]     | Last date the file was modified.                                                                                                                                                                                                                                    |
| \[modtime]     | Last time the file was modified.                                                                                                                                                                                                                                    |
| \[size]        | The size of the file, in bytes.                                                                                                                                                                                                                                     |
| \[startpath]   | The folder path leading to the file.                                                                                                                                                                                                                                |
| \[imagewidth]  | If the file is a GIF or JPEG file, displays the image's width in pixels. This can be very slow for large JPEG files, because the entire file must be read into memory first. We suggest you do this only once, then store the information in a database somewhere.  |
| \[imageheight] | If the file is a GIF or JPEG file, displays the image's height in pixels. This can be very slow for large JPEG files, because the entire file must be read into memory first. We suggest you do this only once, then store the information in a database somewhere. |

**Example WebDNA code:**\
To display information about a file or folder (including whether it exists or not), place a \[FileInfo] context into a WebDNA template. Specify an absolute or relative path to the file in the same way as you would specify an absolute or relative URL. The file name itself may be a WebDNA \[xxx] tag.

```
[fileinfo file=../images/background.jpg]
  File Name: [filename]
  Modified: [moddate]
  Size: [size]
[/fileinfo]
```

\


### \[filecompare] WebDNA v5.0+ [tag](https://docs.webdna.us/contexts-and-tags)

Compares the size, date, or CRC32 value of two given files.

```
[filecompare method=size&file1=bigimage.png&file2=smallimage.png
```

**Parameters**

| Parameter | Description                                                                                                                                                                                           |
| --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| method    | size or date or CRC32                                                                                                                                                                                 |
| file1     | The path to the first file, including the name of the file.                                                                                                                                           |
| file2     | The path to the second file, including the name of the file.                                                                                                                                          |
| file1crc  | Arbitrary CRC32 value, in place of an actual path for file1. This is useful if you want to store the CRC32 value of a file and use it at a later time to test if the file contents have been changed. |

CRC32 is a 32-bit Cyclic Redundancy Check code, used mainly as an error detection method during data transmission. If the computed CRC bits are different from the original (transmitted) CRC bits, then there has been an error in the transmission. If they are identical, it can be assumed that no error occurred (there is a chance 1 in 4 billion that two different bit streams have the same CRC32). The idea is that the data bits are treated as a data polynomial and the CRC bits represent the remainder of the division of the data polynomial by a fixed, known polynomial (called the CRC polynomial).

**Example WebDNA code:**

```
[filecompare method=size&file1=bigimage.png&file2=smallimage.png]
```

Results will be one of the following:\
SMALLER - file2 is smaller than file1\
LARGER - file2 is larger than file1\
SAME - file1 and file2 are the same size

**Example WebDNA code:**

```
[filecompare method=date&file1=bigimage.png&file2=smallimage.png]
```

Results will be one of the following:\
OLDER - file2 is older than file1\
NEWER - file2 is newer than file1\
SAME - file1 and file2 have the same create date

**Example WebDNA code:**

```
[filecompare method=CRC32&file1=bigimage.png&file2=smallimage.png]
```

Results will be one of the following:\
DIFFERS - the files differ in content\
SAME - the files have the same content

Errors are displayed as such:\
ERROR\_FILE1 - 'file1' could not be found or opened\
ERROR\_FILE2 - 'file2' could not be found or opened\
ERROR\_BOTH - neither file could be found or opened\
