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

`sed "$ s/THE EDN/THE END/g" my_book.txt`

Sometimes you may need to search or replace some text that is using the standard delimiter. In those instances you can supply an alternative delimiter.

`sed "s|</dv>|</div>|g index.html"`

You can also use single quotation marks for the search and replace operation if you need to look for double quotation marks. And of course you can use the backslash to escape any characters that have a special meaning in bash.

The `&` character indicates a matched pattern where you don't necessarily know what the exact pattern will be. For example, lets say you want to put parenthesis around a particular matched pattern, you could write `sed "s/[a-z]*/(&)/g" file`

### Sed on Macs

If you want to use a sed script that works equally well on Mac's version of Linux along with other Linux OSs you should supply the `-i.bak` flag which will create a backup file that you are editing.

## Awk

Whereas sed operates on lines, awk operates on columns. That makes it handy for manipulating csv files, for example. The following command would print the second column in your file: `awk < file '{ print $2 }'`

By default awk uses whitespace as a delimiter. The `-F` flag is used to specify the delimiter. `awk -F, < file.csv '{ print $2 }'`

Some useful built-in variables include `$0` which is for the entire line, and `$NF` which is the last field on a line.
