# Pulpscript

Full docs [here](https://play.date/pulp/docs/pulpscript/)

Comments: `// hello pulpscript`

Event handlers:

```
on eventName do
  // logic goes here
end
```

Calling an event handler: `call "eventName"`

For tiles that implement an event use emit: `emit "eventName"`

There are some built-in events:

- `load`
- `start`
- `enter`
- `exit`
- `finish`
- `loop`
- `update`
- `bump`
- `confirm` (when A button is pressed)
- `cancel` (when B button is pressed)
- `crank` (when crank turned)
- `dock` (when crank is docked)
- `undock` (when crank is undocked)
- `draw`
- `interact`
- `collect`
- `change`
- `select`
- `dismiss`
- `invalid`
- `any`

Assigning globally scoped variable: `varName = 0`

Pulpscript has standard arithemtic operations.

Increment value: `varName++`
Decrement value: `varName--`
Increment by four: `VarName += 4`

Pulpscript has Lua syntax `if` conditionals. But it uses the more common `!=` syntax for evaluating not equal to conditions. Compound expressions are not supported :-(

Pulpscript has `while` loops.

Pulpscript does not support user-defined functions. Event handlers cannot return values.

Variables can be persisted across launches using the `store` and `restore` functions. This saves or loads a JSON package. In the web IDE, this is stored in browser local storage.

The `event` variable is a built-in read-only value. It has `dx` and `dy` values that range from `-1` to `1` reflecting the most recent attempted movement direction along each axis. `tx` and `ty` set the room coodinates of the target tile.

The `config` variable can be used to override some default behavior.

`datetime` is a read-only variable.

Pulpscript supports string interpolation with curly braces: `log "count is now set to {count}"`. Variables can be padded to the left or right, e.g. `{6,0:score}`, `{label:8, }`, `{3, :value}`

Pulpscript has the following functions:

- `log`: logs to console
- `dump`: logs the current values in `config`, `event`, persistent storage, and custom variables
- `say`: displays message in a textbox. Messages can be manually positioned and sized.
- `ask`: provides list of options for player to select from
- `menu`: creates a paginated menu with options at a given position
- `fin`: displays the end credits and triggers `finish` event
- `swap`: swaps tiles
- `frame`: gets the current frame of the current instance of a tile. Frames are zero-based.
- `play`: replaces the current instance of a tile with the tile matching `tileId` or `tileName`, then plays the frames of that tile in sequence
- `wait`: waits for a duration in seconds before doing something
- `shake`: shakes the screen for a specified duration
- `tell`: allows one tile to make changes to another.
- `call`: calls an event handler.
- `emit`: emits an event handler.
- `mimic`: allows a tile to mimic the behavior of another.
- `ignore`/`listen`: toggles if the game should accept user input or not.
- `act`: make the player interact with the sprite or collect the item one tile in front of them
- `draw`: draws the requested tile at `x,y` coordinates
- `hide`: prevents the player from being drawn
- `window`: draws a window frame at `x,y` with dimensions `w,h`
- `label`: ?
- `fill`: fills a rectangle `x,y,w,h` with a color (white/black)
- `crop`: crops to `x,y,w,h`
- `goto`: moves player to `x,y` in current room or in the room requested
- `sound`: plays a sound
- `once`: plays a song without looping
- `loop`: loops a song
- `stop`: stops a song
- `bpm`: set the bpm of song
- `store`: saves state
- `restore`: loads state
- `toss`: removes a variable from persistent storage

Pulpscript also has the following math functions:

- `random`: either `max` or `min, max`
- `floor`
- `ceil`
- `round`
- `sine`
- `cosine`
- `tangent`
- `radians`
- `degrees`

Functions that return a value:

- `invert`
- `solid`
- `type`
- `id` gets id of name
- `name` gets name of id
