# How Do I...

This README contains some simple suggestions for how to do basic text manipulation using the command line.

## Combine Two or More Files?

`cat file1 file2` etc.

## Append a New Row to a CSV File?

`sed -i.bak "s/$/"$FOO\"/g" file.csv`

## Print the Total Value of Column N?

`awk -F, '{ sum += $N } END { print sum }' file.csv`
