# Curses

Curses (along with ncurses i.e. new curses) is an older library for the C language that allows you to write scripts that act as interactive terminal applications (think of tools like vim or roguelike games). Python has a curses library as part of the std lib that acts as a wrapper. It is extremely portable and works in most Unix environments and even in things like MS-DOS.

The python.org [tutorial](https://docs.python.org/3/howto/curses.html#curses-howto)

And the [std lib ref](https://docs.python.org/3/library/curses.html)

## Basics

`import curses` to include the library in your script.

`win = curses.initscr()` initializes the library and returns a window object the size of your terminal that is assigned to the variable that, in this case, we are calling `win`, but `screen` or whatever else might be appropriate would be fine too. The rest of this reference page will use the `win` variable for cases where you call a method from this window objecxt, and `curses` for cases where you are calling a method from the curses library itself.

`curses.endwin()` closes the window object and returns you to your normal terminal prompt environment. As important to know as `:wq` in vim!

`win.refresh()` is needed to update the screen with any changes that have been made in the buffer.

`win.addstr(row, col, str)` adds strings to the buffer in positions row and col. Note that the positioning is zero-indexed, and begins in the top left-hand corner of the window object.

`win.addch(row, col, char)` adds a character to the buffer in positions row and col. Useful for variables where you definitely don't want them to be a string (like characters displayed in a roguelike)

`win.clear()` clears out the window object and resets the cursor object to position 0, 0.

`curses.napms(x)` creates a timed delay in milliseconds.

## Common Defaults

`curses.noecho()` turns off the echoing of keys to the screen

`curses.cbreak()` removes the buffer input terminal style where it waits for the Enter key before processing commands

`win.keypad(True)` handles special keys.

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

`pad = curses.newpad(100, 100)` when you need to create a window that is larger than the dimensions of the window object you are operating out of you can create a pad which then only displays a portion of the pad at a time.

`pad.refresh(pad_y, pad_x, win1_y, win1_x, win2_y, win2_x)` is used to refresh a pad. Its parameters include the coordinates of the pad area to display, as well as the starting and ending dimensions of the window where the pad will be displayed.
