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

## Cursor movement

`b` move back a word or special chars starting at first char

`e` or `w` move forward a word

`B` move back a word without regard to special chars

`h` move cursor left

`j` move cursor down

`k` move cursor up

`l` move cursor right

`0` move cursor to beginning of line

`$` move cursor to end of line

`G` move cursor to last line of file

`{number}G` move cursor to specified file line

## Editing

`x` delete a character

`D` delete all text on the line after cursor position

`c ->` delete a char to the right of the cursor and insert a char

`cb` delete word and insert

`C` delete to end of line and insert

`vc` delete visual selection, and insert

`u` undo last command

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
