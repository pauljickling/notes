# Inform 7

Inform 7 is a programming language based on natural language structures that is used for creating parser-based interactive fiction. It has a reasonable baseline of default ideas and concepts useful for interactive fiction, and then is designed in a way to be easily extendible.

# Key Syntaxes

`"<Title>" by author` Appears at the top of every work.

`Use <feature>.` Enables certain optional features for example `Use American dialect.` or `Use scoring.`

`To <verb> is a verb.` Adds a verb that is not present in Inform 7's vocabulary.

`Say "<string>"` An instruction to output a string based on triggered conditions.

`[Comments are included inside square brackets like this]`

`Say "<string with some [text substitution]>"` Text substitutions allow the use of variables and useful built-in tools for handling things like using singular/plural forms and verb tenses correctly, lists of things, etc. 

`A <object> is a kind of thing.` Things are a specific kind of object. Things are extendible, so you can write `A music instrument is a kind of thing. A saxophone is a kind of music instrument.` The sort of intuitions you might have about classes in object-oriented programming are relevant here.

`A <thing> can be <foo> or <bar>.` You can apply details or states to a thing. So you can write `A glass of water can be full or empty.` and it is also possible to add additional state if you'd like, `A glass of water can be full, partially-full, or empty.`

`A <thing> is usually <foo>.` Since things are can have several states, you can say what they usually are. Otherwise it will default to the first thing declared.

`Instead of <verb> the <noun>, say "<string>"`. Most things will have some default defined behavior, but you can create exceptions with the `Instead of` syntax that allows you to provide some alternative behavior.

`Before <doing the thing>:` This allows you to prepend some custom logic before a default behavior fires off.

`After <doing the thing>:` Similarly you can append custom logic to particular actions as well.

`If <the thing> <meets the condition>:` Standard conditional logic. Inform also supports else-if and else statements, but note that it makes the curious decision to use the term `otherwise` instead of `else`.

`Now <the thing> is <some other thing>.` `Now` is a key word that transforms the state of something.

## Organizing Code

Inform has headings and subheadings to organize code. It uses the metaphor of books, and so things can be divided into "Volume", "Book", "Part", "Chapter", or "Section".

Headings must not contain a typed line break, and there should be a skipped line before and after it.

## Useful References

[Inform 7 Programmer's Manual](https://zedlopez.github.io/i7doc/i7prog/index.html)
