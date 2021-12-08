# Curses

Curses (along with ncurses i.e. new curses) is an older library for the C language that allows you to write scripts that act as interactive terminal applications (think of tools like vim or roguelike games). Python has a curses library as part of the std lib that acts as a wrapper. It is extremely portable and works in most Unix environments and even in things like MS-DOS.

The python.org [tutorial](https://docs.python.org/3/howto/curses.html#curses-howto)

And the [std lib ref](https://docs.python.org/3/library/curses.html)

You can also consult the man pages for more info on curses. `man curses`.

## Basics

`import curses` to include the library in your script.

`win = curses.initscr()` initializes the library and returns a window object the size of your terminal that is assigned to the variable that, in this case, we are calling `win`, but `screen` or whatever else might be appropriate would be fine too. The rest of this reference page will use the `win` variable for cases where you call a method from this window objecxt, and `curses` for cases where you are calling a method from the curses library itself.

`curses.endwin()` closes the window object and returns you to your normal terminal prompt environment. As important to know as `:wq` in vim!

`win.refresh()` is needed to update the screen with any changes that have been made in the buffer.

`win.addstr(row, col, str, attribute)` adds strings to the buffer in positions row and col. Note that the positioning is zero-indexed, and begins in the top left-hand corner of the window object. The attribute arg is optional.

`win.addch(row, col, char)` adds a character to the buffer in positions row and col. Useful for variables where you definitely don't want them to be a string (like characters displayed in a roguelike)

`win.clear()` clears out the window object and resets the cursor object to position 0, 0.

`curses.napms(x)` creates a timed delay in milliseconds.

## Common Defaults

`curses.noecho()` turns off the echoing of keys to the screen

`curses.cbreak()` removes the buffer input terminal style where it waits for the Enter key before processing commands

`win.keypad(True)` handles special keys.

`curses.start_color()` allows the script to specify color values for text

### Using a Wrapper

If you write a bad script that crashes out unexpectedly from an unhandled exception you run the risk of your terminal still operating in the interactive mode (the curses library operates at a fairly low level!)

This can be mitigated by using the `wrapper()` function in the following way:

```
from curses import wrapper

def main(win):
    """
    Your application code
    """

wrapper(main)
```

## Restoring Terminal Defaults

```
curses.nocbreak()
win.keypad(False)
curses.echo()
curses.endwin()
```

## Window Management

`newwin = curses.newwin(height, width, y, x)` Creates a new window with a specified height and width starting from the y and x coordinates specified.

`curses.LINES` can be used to determine the ending y size

`curses.COLS` can be used to determine the ending x size

`newwin.mvwin(row, col)` relocate the window to the specified coordinates.

`pad = curses.newpad(100, 100)` when you need to create a window that is larger than the dimensions of the window object you are operating out of you can create a pad which then only displays a portion of the pad at a time.

`pad.refresh(pad_y, pad_x, win1_y, win1_x, win2_y, win2_x)` is used to refresh a pad. Its parameters include the coordinates of the pad area to display, as well as the starting and ending dimensions of the window where the pad will be displayed.

## Cursor Management

`curses.curs_set(bin)` a value of `0` or `1` where `0` disables the cursor, and `1` activates it.

## Color Management

`color_pair(n)` Provided as an argument to an `addstr()` function that returns a foreground and background color value. `n` is an int.

`curses.init_pair(n, foreground_color, background_color)` sets a color pair used by the `color_pair` function to a specified foreground and background color.

### Color Values

When `start_color()` is called, it initializes 8 color values.

| Index | Color   |
| ----- | -----   |
| 0     | Black   |
| 1     | Red     |
| 2     | Green   |
| 3     | Yellow  |
| 4     | Blue    |
| 5     | Magenta |
| 6     | Cyan    |
| 7     | White   |

When assigning colors using the `init_pair` function you can use constants available in the curses library like `curses.COLOR_CYAN`, etc.

## Attributes

`curses.A_BLINK` blinking text

`curses.A_BOLD` bold text

`curses.A_DIM` half bright text

`curses.A_REVERSE` reverse video text

`curses.A_STANDOUT` highlighted text

`curses.A_UNDERLINE` underlines text

## User Input

`getch()` refreshes the screen and then waits for the user to hit a key, displaying the key. A coordinate can be provided as an option for where the cursor should be located while awaiting input. It returns an integer value between 0 and 255 (the number of values for ASCII), plus some additional integer values for special keys like `KEY_UP`.

`getkey()` is similar, but converts the int value to a string.
