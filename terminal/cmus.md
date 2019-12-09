# CMus

CMus is a terminal based music player built using ncurses that uses a vim-esque interface. For example, to add music to Cmus you type `:add <directory>`.

Any vim-like eperience wouldn't be complete without beginning with an explanation for how to exit the program. You can press `q` to quit.

## Standard Library Commands

In the default library view there will be a column of artists/albums, and a second column of songs. In this view the following commands are relevant:

`tab` switches between columns
`enter` play selected song
`x` play song
`v` stop song
`c` pause song
`b` next song
`z` previous song
`l` +5 sec
`h` -5 sec
`.` +1 min
`,` -1 min
`r` toggle repeat
`s` toggle shuffle

## Views

There are other views aside from the standard library view, and these are toggled via `1` through `7`.

`1` Standard Library
`2` Sorted Library
`3` Playlist
`4` Play Queue
`5` File Browser
`6` Filters
`7` Settings

Settings is particularly useful for a new user because it has a list of keybindings.

## Copying Tracks Between Views

To utilize views list the playlist or play queue you will need to able to move songs between views.

`a` Copy to library (views 1-2)

`y` Copy to playlist (view 3)

`e` Append to play queue (view 4)

`E` Prepend to play queue (view 4)

## Organizing Playlists

`:pl-create <playlist name>` Create a new playlist

`p` Move selected song down in the playlist order

`P` Move selected song up in the playlist order

`D` Remove selected song from the playlist
