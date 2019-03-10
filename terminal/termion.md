# Termion

Termion is a Rust crate that provides a TTY interface using an expressive API.

## Color Text

Termion can format text colors in stdout. The Reset variant sets text to the default color.

`println!("{} Hello {} World! {} Colors have been reset.", color::Fg(color::Red), color::Fg(color::Green), color::Fg(color::Reset));`

`Fg` stands for Foreground color. Use `Bg` for Background color.

If the default colors are insufficient for your needs you can also provide 24 bit rgb values.

`termion::color::rgb(r, g, b)`

## Clearing the Screen

The default way to clear the terminal screen in Rust would be something like the following statement:

`print!("{}[2J", 27 as char);`

Termion provides an API that is easier to understand.

`println!("{}", clear::All);`

Additional variants include `CurrentLine` `AfterCursor` and `BeforeCursor`.

## Cursor Manipulation

The terminal cursor controls the placement of characters in the display. Termion provides a grid interface.

`cursor::Goto(4, 2)`

The first parameter is the column placement, and the second parameter is the row placement.

In addition to specifying specific coordinates, you can simply move the cursor directionally as well.

`cursor::Down(2)` moves the cursor down 2 spaces

There are also structs for showing and hiding the cursor.

`cursor::Hide()`

`cursor::Show()`

## Raw Mode

Raw mode allows for the development of an interactive tty interface. Raw mode disables stdin, line buffering, and scrolling.

### Entering Raw Mode

#### Imports

```
use std::io::{stdout, Write};
use termion::raw::IntoRawMode;
```

#### Assign a stdout mutable variable

`let mut stdout = stdout().into_raw_mode().unwrap();`

### Display in Raw Mode

Use `writeln!` while in raw mode, and control the display with the cursor.

`writeln!(stdout, "Hey there.").unwrap();`

You will always need to `unwrap()`.

## Inputs

### Keys

#### Imports

```
use std::io::stdin;
use termion::Event::Key;
use termion::Input::TermRead;
```

#### Assign a stdin variable

`let stdin = stdin();`

#### Controller

```
for key in stdin.keys() {
    match key {
        Key::Char('q') => break,
        Key::Left => println!("left arrow"),
        Key::Char(key) => println!("{}", key),
        _ => println!("Oops!"),
    }    
}
```

## Learning More

For more details there is an [excellent blog post on using Termion](https://ticki.github.io/blog/making-terminal-applications-in-rust-with-termion/).
