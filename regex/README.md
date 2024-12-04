# Regular Expressions
Regular Expressions are composed of two types of characters, **metacharacters** and **literals**. Literals match to the literal character represented. Metacharacters have special meanings within a regex definition to help scope the pattern matching.

## Metacharacters
`^` start of a line
`$` end of a line
`.` match any one character
`|` or expression, meaning match either pattern to the left or the right
`(pattern)` limits the scope of an `|` metacharacter. We can refer to these as **subexpressions**.
`\` escape delimiter

### Metacharacter Quantifiers
`?` character that precedes it is optional, and not required
`+` an unlimited number of the preceding character allowed, but at least one required
`*` an unlimited number of the preceding character allowed, but not required

## Character Classes
`[xy]` allows you to check for an enumeration of matches. Thus, if you wanted to match either `live` or `love` you could use `l[io]ve`. Note that the characters contained in the character class count as a single character only. So `live` would return a match, as would `love`, but not `liove` or `loive`. 

To include a range of characters within a character class you can also use a dash, so if you want to check for any digits you could do `[0-9]`, and if you also wanted to include the alphabet you could use `[0-9a-zA-Z]`. 
A dash is only a metacharacter within the context of a character class.

The `^` metacharacter has a different meaning within the character class, where it negates the value. So `l[^io]ve` matched with `lave` would return true.

Character classes can be strung together, so `[Qq][^u]` checks for words that have an either upper or lower case q, but no u following it.
