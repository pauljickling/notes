# Curses

Curses (along with ncurses i.e. new curses) is an older library for the C language that allows you to write scripts that act as interactive terminal applications (think of tools like vim or roguelike games). Python has a curses library as part of the std lib that acts as a wrapper. It is extremely portable and works in most Unix environments and even in things like MS-DOS.

The python.org [tutorial](https://docs.python.org/3/howto/curses.html#curses-howto)

And the [std lib ref](https://docs.python.org/3/library/curses.html)

## Basics

`import curses` to include the library in your script.

`win = curses.initscr()` initializes the library and returns a window object that is assigned to the variable that, in this case, we are calling `win`, but `screen` or whatever else might be appropriate would be fine too.

`curses.endwin()` closes the window object and returns you to your normal terminal prompt environment. As important to know as `:wq` in vim!

`curses.refresh()` is needed to update the screen with any changes that have been made in the buffer.

`curses.addstr(x, y, str)` adds strings to the buffer in positions x and y. Note that the positioning is zero-indexed, and begins in the top left-hand corner of the window object.

`curses.clear()` clears out the window object and resets the cursor object to position 0, 0.

`curses.napms(x)` creates a timed delay in milliseconds.
