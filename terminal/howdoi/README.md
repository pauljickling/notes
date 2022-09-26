# How Do I...

This README contains some simple suggestions for how to do basic text manipulation using the command line.

## Combine Two or More Files?

`cat file1 file2` etc.

## Append a New Row to a CSV File?

`sed -i.bak "s/$/"$FOO\"/g" file.csv`

## Print the Total Value of Column N?

`awk -F, '{ sum += $N } END { print sum }' file.csv`

## Handle Software Updates for OSX via CLI?

`softwareupdate -l` to get the list of updates. It will list which updates are recommended. The following methods are possible for install:

`softwareupdate -r` for recommended updates

`softwareupdate -i -a` for all updates applicable to your system

`softwareupdate -i {pkg name}` to install specific updates.

`softwareupdate --ignore {pkg name}` to install all updates except for the specified package
