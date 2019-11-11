# Bash Scripts

The start of a bash script begins with `#!/bin/bash`. This is called the *shebang*, and it defines which program is run to interpret the script.

`#` is used for comments.

In bash scripts you can run anything that you would run on the bash command line. This makes scripts incredibly useful for build processes that require multiple steps to run.

## Variables

Example of assigning a variable:

`hi="hello world"`

Example of calling a variable:

`echo $hi`

### Substrings

Substrings use the following convention:

`${string:start_index:end_index}`

Note that in OSX bash does not support negative numbers for the index values

## Control Flow

### `if` Statements

`if` statements have the following format:

```
if [ condition ] ; then
  # code block
fi
```

### Loops

TODO
