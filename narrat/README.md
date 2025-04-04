# Narrat

Narrat is a game engine for making narrative games with RPG features. Games are built as Electron apps. They are configured with yaml config files, a bit of CSS, some TypeScript, and some custom narrat scripts (`.narrat` files).

## The Scripts

Narrat scripts also feature a yaml-like syntax with a couple of important keywords.

### Labels

Labels act as a chunk of game content. Every narrat game must have a `main` label, which is where the game will start.

### Syntax

All syntax commands can be found [here](https://docs.narrat.dev/commands/all-commands.html).

`jump <label>` Jumps to the specified label.

`run <label>` Executes the specified label, and then returns to this position. In this way labels can act as repeatable functions rather than one-off pieces of narrative content.

`talk <character> <image label> "<content>"` Talk outputs dialog from a character, showing their portrait as the dialog is displayed. When inserting basic narration instead of a character you can just have a line with `"<content>"`.

`think <character> <image label> "<content>"` Similar to talk, but for character thoughts. No quotation marks are inserted into the content.

`set_screen <screen label>` Sets the background to the specified screen as defined in the `screens.yml` config.

`set data.foo <value>` Assigns a data attribute (e.g. game variable) to a speicified value.

`%{$data.foo}` Interpolates the data attribute value.

#### Conditionals

```
if (<conditional expression>):
  <content>
elseif (<conditional expression>):
  <content>
else:
  <content>
```

#### Choices

Choices have the following structure.

```
choice:
  <question>
  "<answer n>":
    "<response n>"
  "<answer n+1>":
    "<response n + 1>"
```

Common things to include in responses in addition to character dialog would be `jump` or `run` instructions, or setting a variable.
