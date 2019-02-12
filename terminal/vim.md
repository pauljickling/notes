## Exiting Vim

`:q` quit

`:q!` force quit

`:wq` write quit (i.e. save and quit)

## File Management

`:w` write file (i.e. save)

`:n` next file

`:N` previous file

## Insertions

`i` insert chars prior to cursor location

`I` insert chars at start of line from cursor position

`a` append chars after cursor location

`A` append chars at end of line

`o` newline append

`O` newline above cursor position

`Ctrl + p` keyword completion

## Cursor movement

`b` move back a word or special chars starting at first char

`e` or `w` move forward a word. w for beginning of word, and e for end.

`f + char` moves cursor towards next specified char

`F + char` moves cursor back towards specified char

`B` move back a word without regard to special chars

`h` move cursor left

`j` move cursor down

`k` move cursor up

`l` move cursor right

`0` move cursor to beginning of line

`$` move cursor to end of line

`gg` moves cursor to first line of the file

`G` move cursor to last line of file

`{number}G` move cursor to specified file line

## Editing

`x` delete a character

`d ->` deletes to the right of the cursor

`d <-` deletes to the left of the cursor

`dd` deletes entire line

`vd` deletes visual selection

`gv` selects last visual selection

`D` delete all text on the line after cursor position

`c ->` delete a char to the right of the cursor and insert a char

`cb` delete word and insert

`C` delete to end of line and insert

`S` delete an entire line and insert

`vc` delete visual selection, and insert

`u` undo last command

`U` undo all changes on a line

`ctrl + r` redo last undo

`v` visual mode

`y` yank selected text (i.e. copy)

`p` put text (i.e. paste) below or to the right of cursor

`P` put text above or to the left of cursor

`r + {char}` replace a single char at cursor

`R` enters replace mode. Replace mode is a write-over editing style.

`:s/{find_text}/{replacement_text}/g` Replace found text with replacement text globally in a file

`>` while in visual mode ident block

`<` while in visual mode remove indent from block

## Search

`/{expression}` search for expression

`n` search for next instance of expression

`N` move back to last instance of expression

`:noh` remove search highlighting until next search

## Markers

`m + char` set a marker to cursor position

``  ` + char`` go to specified marker

``  ` + CHAR`` go to specified marker in another file

`:marks` view list of markers

`:delmark + char` deletes a marker

`:delmarks!` deletes all markers

## Macros

`q + {num/char}` begin/end macro to a specified register

`@ + {num/char}` execute macro from specified register

## Windows Management

`:split` creates new window

`:vsplit` creates a new vertical window

`:close` closes a window

`ctrl + W` switches windows

`:edit .` opens current file directory

## Help

`:help` open help file

`:help {command}` open help to info about a specific command

## Misc

`esc` return to normal mode
