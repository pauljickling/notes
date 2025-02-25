## Exiting Vim

`:q` quit

`:q!` force quit

`:wq` write quit (i.e. save and quit)

`:wqa` write quit all

## File Management

`:w` write file (i.e. save)

`:n` next file

`:N` previous file

`ctrl + w + n` create a new file in a new horizontal pane

## Insertions

`i` insert chars prior to cursor location

`I` insert chars at start of line from cursor position

`a` append chars after cursor location

`A` append chars at end of line

`o` newline append

`O` newline above cursor position

`ctrl + p` keyword completion

## Cursor movement

`b` move back a word or special chars starting at first char

`gE` move back a word or special chars to the last char

`e` or `w` move forward a word. w for beginning of word, and e for end.

`f + {char}` moves cursor towards next specified char

`F + {char}` moves cursor back towards specified char

`B` move back a word without regard to special chars

`h` move cursor left

`j` move cursor down

`k` move cursor up

`l` move cursor right

`0` move cursor to beginning of line

`$` move cursor to end of line

`gg` moves cursor to first line of the file

`G` move cursor to last line of file

`)` move forward one sentence

`(` move backward one sentence

`}` move forward one paragraph

`{` move backward one paragraph

`H` move to the top of the screen

`M` move to the middle of the screen

`L` move to the bottom of the screen

`{number}G` move cursor to specified file line

`{number}|` move cursor to specified column number on line

`g*` Find next phrase that matches word at cursor location

`*` move cursor to next instance of current word

`#` move cursor to previous instance of current word

## Editing

`x` delete a character as del key

`X` deletes a character as backspace key

`d {->}` deletes to the right of the cursor

`d {<-}` deletes to the left of the cursor

`dd` deletes entire line

`vd` deletes visual selection

`gv` selects last visual selection

`D` delete all text on the line after cursor position

`c {->}` delete a char to the right of the cursor and insert a char

`cb` delete word and insert

`C` delete to end of line and insert

`S` delete an entire line and insert

`vc` delete visual selection, and insert

`u` undo last command

`U` undo all changes on a line

`v + U` change selected text in visual mode to uppercase characters

`ctrl + r` redo last undo

`v` visual mode

`V` visual line mode selects entire lines for visual selection

`ctrl + v` visual block mode

`y` yank selected text (i.e. copy)

`Y` yanks entire line of text

`p` put text (i.e. paste) below or to the right of cursor

`P` put text above or to the left of cursor

`r + {char}` replace a single char at cursor

`R` enters replace mode. Replace mode is a write-over editing style.

`:%s/{find_text}/{replacement_text}/g` Replace found text with replacement text globally in a file

`:g/{pattern}/{vim command}` Globally execute the specified vim command on every line matching the specified pattern

`>` while in visual mode indent block

`>>` indent block while in normal mode

`<` while in visual mode remove indent from block

`<<` remove indent block while in normal mode

`ctrl+r = {math expression}` while in insert mode returns the product of a mathematical expression, e.g. `ctrl+r=5*8` will insert `40`

## Search

`/{expression}` search for expression

`/c{expression}` case insensitive search for expression

`n` search for next instance of expression

`N` move back to last instance of expression

`:noh` remove search highlighting until next search

`:%s/foo/bar/g` sed-like search and replace that finds all instances of foo, and replaces them with bar. Supports regexes.

`:%50,80s/foo/bar/g` Sets a range for the search function where it searches and replaces between lines 50 and 80.

## Markers

`m + {char}` set a marker to cursor position

``  ` + {char}`` go to specified marker

``  ` + {CHAR}`` go to specified marker in another file

`:marks` view list of markers

`:delmark + {char}` deletes a marker

`:delmarks!` deletes all markers

## Macros

`q + {num/char}` begin/end macro to a specified register

`@ + {num/char}` execute macro from specified register

## Windows Management

`:split` creates new window

`:vsplit` creates a new vertical window

`:close` closes a window

`ctrl + W` switches windows

`ctrl + W` + `{r/R}` moves current window down/left or up/right

`:edit .` opens current file directory

`:resize {signed num}` resize window

`:vertical resize {signed num}` resize window vertically

`zc` Close a fold

`zo` Open a fold

`za` Fold toggle

`:mks /path/to/{session name}.vim` Create a session (a specific configuration of windows and files)

`:source /path/too/{session name}.vim` Restore session

`:tabe {filename}` open file as tab

`gt` go to next tab

`gT` go to previous tab

`{i}gt` for to number speicifed tab

## Help

`:help` open help file

`:help {command}` open help to info about a specific command

## Misc

`esc` return to normal mode

`:verbose set ai? cin? cink? cino? si? inde? indk?` view where current indentation settings were set
