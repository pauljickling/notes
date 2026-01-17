# Pulp

Pulp is a web tool for making games for Playdate. Pulp includes some basic objects that can be utilized for straight forward turn based adventure games. There is also a Pulpscript that allows for some additional extensibility. The Pulp editor has six modes elaborated below.

## Game

Game mode includes all the metadata associated with your game.

## Font

The font mode allows you to define and customize the font of your game.

## Room

This is the "game space". Rooms are composed of tiles. The following are tile types:

- Player
- Sprite
- Item
- World

Tiles can be solid (cannot be passed through) or not solid. Tiles can have multiple frames allowing for animation. World tiles do not have scripted behavior but sprite and item tiles can.

Rooms have exits that allow you to connect rooms together. Exits can be single or bidirectional (in the graph theory sense). There are also exits that generate an end game state.

## Song

Song mode allows you to compose music with a basic synthesizer. Pulp supports a keyboard interface for inputting notes with the middle row being white keys, and the top row black keys. Z and X lower and raise the octave.

There is a basic envelope filter for adjusting the quality of the notes.

## Sound

Similar to Song mode, but for sound effects instead of a looping soundtrack.

## Script

TODO
