# How Do I...

This README contains some simple suggestions for how to do basic text manipulation using the command line.

## Combine Two or More Files?

`cat file1 file2` etc.

## Append a New Row to a CSV File?

`sed -i.bak "s/$/"$FOO\"/g" file.csv`

## Print the Total Value of Column N?

`awk -F, '{ sum += $N } END { print sum }' file.csv`

## Create a pseudo file and pipe the output to another command?

`command1 <( command to create pseudofile )`

Within the paranthesis bash will take the stdout of the command used, and pipe it to `command1`. So for example, if you wanted to examine changes made to a git PR locally you could use:

`git apply <(curl -L  https://github.com/<user/org>/<repo>/pull/<PR no>.diff)`

## Specify the file line range for grep output?

`sed -n '{min}, {max}p' {file} | grep {query}`

## Handle Software Updates for OSX via CLI?

`softwareupdate -l` to get the list of updates. It will list which updates are recommended. The following methods are possible for install:

`softwareupdate -i -r` for recommended updates

`softwareupdate -i -a` for all updates applicable to your system

`softwareupdate -i {pkg name}` to install specific updates.

`softwareupdate -i -a --ignore {pkg name}` to install all updates except for the specified package
