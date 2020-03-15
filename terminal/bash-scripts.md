# Bash Scripts

The start of a bash script begins with `#!/bin/bash`. This is called the *shebang*, and it defines which program is run to interpret the script.

Note, if you want to be really efficient, you can use `#!/bin/sh` if you aren't using any features from the borne again shell.

`#` is used for comments.

In bash scripts you can run anything that you would run on the bash command line. This makes scripts incredibly useful for build processes that require multiple steps to run.

You can also begin your script with the command `set -e` To have the bash script stop if any program it calls fails. Similarly, you can use `set -u` for bash to stop and fail if any unset variables are used. Combined with debugging you might begin your script with `set -eux`.

## Variables

Example of assigning a variable:

`hi="hello world"`

Note that there are no spaces next to the equal sign and the variable and its assignment.

Example of calling a variable:

`echo "$hi"`

In some instances you will need to quote your variable using the `${hi}` syntax. Generally you should stick to quoting your variables.

### Substrings

Substrings use the following convention:

`${string:start_index:end_index}`

Note that in OSX bash does not support negative numbers for the index values

## Functions

```
say_hello () {
    echo "Hello  there";
}
say_hello
```

## Control Flow

### `if` Statements

`if` statements have the following format:

```
if [[ condition ]] ; then
  # code block
fi
```

Note that the spaces between the square brackets are needed. Single square brackets are used for programs, and double square brackets are used for bash syntax. Generally either work, but it is better to use double square brackets. Double square brackets are needed if you want to make a binary comparison operator (i.e. `10 > 5`) and not file redirection.

### Checking if a file exists

```
if [[ -e /path/to/file ]] ; then
    # do something
fi
```

### Loops

```
for i in `seq 1 10`
do
 echo "$i"
done
```

Note that `seq` is a key word for sequence ranges.

#### Looping through Files and Directories

One thing bash is really good at is looping through files and directories. The syntax for looping through directories from your currently working directory is:

```
for DIRECTORY in \*/ ; do
    echo "$DIRECTORY"
done
```

To loop through files in a directory:

```
for FILE in * ; do
    echo "$FILE"
done
```

Note that this will search every file in the directory since `*` is a wild card character. This is why for the directory search you have `*/`, essentially you are searching for any file that ends with a `/` i.e. a directory. So if you wanted to search for JavaScript files in a directory, you would write `for JSFILE in *.js ; do`.

#### Looping through lines of a file

```
while read i; do
    echo "$i"
done < /path/to/file
```

### Running Multiple Programs

`sed -i .bak "s/my_varaible/my_variable/g" app.js; node app.js` Runs these two programs no matter what

`sed -i .bak "s/my_varaible/my_variable/g" app.js && node app.js` Only runs the node script if the sed program exited with return code `0` (`0` almost always means exited successfully).

`sed -i .bak "s/my_varaible/my_variable/g" app.js || node app.js` Only runs the node script if the sed program fails.

## Debugging

`set -x` causes Bash to print out every command before running it.

You can use [ShellCheck](https://www.shellcheck.net/#) for debugging Bash scripts.
