# Sed and Awk

Sed and awk are text processing languages that can be used on the command line or in bash scripts. They are incredibly handy for handling repetitive changes to a document.

Useful links for both these tools:

[sed](https://www.grymoire.com/Unix/Sed.html)

[awk](https://www.grymoire.com/Unix/Awk.html)

## Sed

The simplest sed command has the following format:

`sed "s/<old phrase>/<new phrase>/g" <text source>`

This goes through every line of text looking for the old phrase and replacing it with the new phrase.

You can also specify a particular line or range of lines like in the following example that only replaces the text hello with goodbye on line 5:

`sed "5s/hello/goodbye/g" foo.txt`

To specify the last line of a final you can use the `$` character.

`sed "$s/THE EDN/THE END/g" my_book.txt`

Sometimes you may need to search or replace some text that is using the standard delimiter. In those instances you can supply an alternative delimiter.

`sed "s|</dv>|</div>|g" index.html`

You can also use single quotation marks for the search and replace operation if you need to look for double quotation marks. And of course you can use the backslash to escape any characters that have a special meaning in bash.

The `&` character indicates a matched pattern where you don't necessarily know what the exact pattern will be. For example, lets say you want to put parenthesis around a particular matched pattern, you could write `sed "s/[a-z]*/(&)/g" file`

In addition to replacing lines, sed can also be used to delete lines that match the regular expression.

`sed /expression/d foo.txt`

### Sed on Macs

If you want to use a sed script that works equally well on Mac's version of Linux along with other Linux OSs you should supply the `-i.bak` flag which will create a backup file that you are editing.

## Awk

Whereas sed operates on lines, awk operates on columns. That makes it handy for manipulating csv files, for example. The following command would print the second column in your file: `awk '{ print $2 }' file`

By default awk uses whitespace as a delimiter. The `-F` flag is used to specify the delimiter. `awk -F, '{ print $2 }' file.csv`

Some useful built-in variables include `$0` which is for the entire line, `$NR` is the line number, and `$NF` which is the last field on a line.

You can use regex patterns in awk to filter your results. `awk '/foo/ { print $0 }' file` will only return lines that contain a regex match.

Awk can evaluate conditionals. `awk -F, '$2 > 6 { print $0 }' file` will print the lines where the 2nd column has a value greater than 6. The `FNR` keyphrase can be used to evaluate row numbers, so `awk 'FNR == 1 { print $2 }'` will print out the 2nd column from the first row.

Awk has associative arrays, which is incredibly useful, so you could include something like `name[$1]++;` to place data from the first column into your `name` associative array.

You can set variables to your awk script prior to execution with the `-v` flag e.g. `awk -v foo=bar -v cool=wow ...`. It is advisable not to set these variables to one of the built-in variables as it will overwrite them and make your script harder to understand.

`BEGIN` and `END` are important keywords in awk. awk scripts follow a format of a pattern and action, with the action enclosed in curly braces. The default pattern is implicit if no pattern is specified, and it matches every line of the program. By contrast, `BEGIN` is a pattern that is performed before any pattern is read, and `END` is a pattern that is performed after the last line is read.

`\47` is a way to insert a single quotation mark without having to use even more verbose shell syntax.

### Built-in Vars

- `FS` Field Seperator. Equivalent to the `-F` flag in the commandline
- `OFS` Output Field Seperator. By default the following statement `print $2, $3` would output the 2nd and 3rd field with a space between them. `OFS=", "`, for example, would output that with a comma separating the field values
- `NF` Number of Fields.
- `NR` Number of Records (e.g. rows or file lines)
- `RS` Record Separator. By default the newline character, but you could set it to anything
- `ORS` Output Record Separator. Similar to `OFS` you can set how awk output handles new records
- `FILENAME` is the value of the file name being read

### Writing an Awk file

Using the `-f` flag (lowercase) will allow you to specify an awk file which is useful for more complicated instructions. You can also write self-contained awk scripts by beginning your script as follows: `#! /bin/awk -f` or `#! /usr/bin/awk -f` depending on the location of awk on your system.
