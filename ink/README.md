# INK

Ink is a scripting language designed for producing interactive fiction content developed by Inkle Studios and is open-source. The reference document is [here](https://github.com/inkle/ink/blob/master/Documentation/WritingWithInk.md).

## BASICS

### Include Files

Ink scripts can be divided up, and included at the top of an ink file.
```
INCLUDE istanbul.ink
INCLUDE dinner_party.ink
```

### Content

Basic written content in Ink is like markdown in that it doesn't require any additional syntax to be provided. One simply writes the content.

### Comments

Expressed as with JavaScript, `//` for single line comments, or `/* ... */` for multiline.

Additionally, the Inky IDE will highlight `TODO` lines.

### Escape Character

If you need to escape a character normally reserved for syntax you can use a backslash `\`.

### Choices

At the heart of interactive fiction is the ability to make narrative choices. Ink provides two methods for providing links: one-time choices, and repeatable choices.

One-time choices cease to become available after the choice has been made. This is the default pattern for a lot of interactive fiction.

```
* Examine the painting
* Drink your coffee
```

This provides options for "Examine the painting" or "Drink your coffee" that will generate additional content. By contrast, if we were in a section of a narrative where choices could be repeated, we can use what are called "sticky choices", and we would provide these options in this way:

```
+ Take a sip of water
+ Whistle a tune
```

By default the choices you make appear as printed text in your narrative as you move onto the next piece of content. If you wanted to remove this behavior you would include the text in brackets.

```
* [Examine the painting]
* [Drink your coffee]
```

Fallback choices are used when no other choices are available, but the narrative hasn't terminated. An empty choice with a divert (see Diverts section below) is a falback choice that appears after all other choices are exhausted (and thus, will never work with sticky choices). `* -> next_section`. To add text to a fallback choice you add two divert arrows. `* -> I felt I was out of options -> next_section`.

### Knots

Knots are an organizing structure in Ink that controls the flow of branching narratives. Additionally knots accept parameters, allowing for greater flexibility and logical operations on your narrative compositions. The basic syntax of a knot is as follows: `=== travel_to_istanbul ===`. You can then link to your knot in your choices. You would do that in this way: `* [I travelled to Istanbul] -> travel_to_istanbul`.

### Diverts

The technical term for the link to the knot mentioned is a "divert". It uses the `->` syntax. Although a common pattern is to use choice, divert, knot this isn't strictly necessary. One could include a divert at the end of one knot and attach it to the next knot. There is also a special divert `-> END` that ends the piece. It is even possible to include diverts mid-sentence, although they typically produce a newline. There is a feature called "glue" that doesn't create a new line. The syntax for it is `<>`.

### Stitches

Stitches are a "sub-knot". Stitches are declared using `= second_encounter` syntax, and can be reached by a divert by including the knot and dot notation, e.g. `-> dinner_party.second_encounter`. Note that you don't need to use the knot name when diverting to a local stitch.

### Conditionals

Under the hood the Ink engine always tracks all the knots a player visits. So this is the simplest way to use conditionals.

```
* { not istanbul } [I took my first trip to Istanbul] -> istanbul
* { istanbul } [I returned to Istanbul] -> istanbul
```

Conditionals can be combined with logical operators AND `&&` and OR `||`.

Additionally, these conditonals don't have to just be treated as booleans. It is also counting the number of times a knot is visited.

`* {on_the_road > 7} [I had been on the road for over a week] -> on_the_road`

Text can also be included conditionally. The syntax for it is as follows:

`{glasses_of_wine > 3: I was feeling quite drunk.|I was enjoying myself at the dinner party.}`

### Alternative Text

Alternative text, called "alternatives" allows for variations of text in the same knot. Alternatives are included with curly braces `{}` and the content for alternatives are seperated by different types of delimiters depending on how the text is alternated.

Sequencing changes up the text in sequence, incrementing with each visit to the knot, until it reaches the end of the list.

`I arrived at this place {for the first time|for the second time| for the third time|yet again}.`

A cycle also increments through a list, but it loops back to the beginning after it reaches the last element. The syntax is the same, but they begin with an ampersand.

A once-only alternative also moves through the list sequentially, but after the last element, no content is displayed afterwards. A once-only has the same syntax, but begins with an exclamation.

Shuffles choose randomly from the list. The begin with a tilde.

Alternatives can have blank elements, can be nested, can include diverts, and can be used within choices.

### Game Queries

Ink has several built-in functions that track game state. `{CHOICE_COUNT() == x} Choice` tracks the number of choices in the knot, useful if you want to limit the number of available options.

`TURNS()` returns the number of turns that have occured. `TURNS_SINCE(->knot)` is also an option.

`SEED_RANDOM(x)` can fix a seed number which can be helpful for debugging purposes.

### Tags

Tags can add some additional logic to Ink scripts depending on the game environment. See the Web Features section for details on tagging for web builds. Tags are created by adding a `#` character.

## WEAVE

Weave refers to a simplified story flow that doesn't involve having to define lots of knots, choices, and diverts.

### Gathers

Sometimes in narrative content you want to provide the user with a set of choices that are more about flavor or player expression than branching narratives. In those cases after providing a set of choices and responses, you can append a gather mark `-` to pick the narrative back up.

In this way choices and gathers can form a chain of content for a single knot.

### Nested Flow

Within a weave of gather and choices there can be follow up choices and gather points that can be nested within the overall structure.

```
- "Want to go on a date?"
* Yes
    "Great! What day were you thinking?"
    * * Thursday
    * * Friday
    - - "Oh, Friday is no good for me. How about Saturday?"
    * * Sure
    * * That doesn't work for me.
* No
- "Alright then!"
```

There is no limit to how nested you want your flow to be, but like JavaScript callback hell, it is probably in your best interest to break up content with knots or stitches if things get too nested.

### Labels

Gathers and choices can be labelled with `()`. This allows us to refer back to the gather point or choice later on. e.g. It could be included in a conditional that only executes if a particular choice or gather point was visited. As with stitches, if it isn't local the label will need to referred to using the knot and dot notation.

## VARIABLES AND LOGIC

### Global Variables

Variables are declared with the `VAR` keyword. Valid types are strings, integers, and diverts. Lists are also a data type, but are covered in a later section.

Note that strings must be declared with double quotes `"`, and not single ones.

Vars can be interpolated with curly braces. Booleans will act as conditionals. Strings can be compared based on equality, inequality, and substrings (with the `?` notation).

The tilde mark `~` allows operations to be performed on variables.

### Math

Ink supports addition, subtraction, multiplication, division, and modulus operations. There is also a built-in function for to the power of e.g. `POW(3, 2)` would be 9.

`RANDOM(min, max)` assigns pseudo-random numbers. Both parameters are inclusive, so for a simulation of a six sided die you would write `RANDOM(1, 6)`. Note that due to how the game engine is designed it is not possible to generate random numbers for newly created variables, however you can bypass this with a tilde operation.

```
// d&d stats
VAR str = 0
~ str = RANDOM(3, 18)
```

(We'll ignore that the rules in D&D are for rolling a 3d6, and thus the probability curve would differ to this implementation)

Arithmetic operations are type based, so operations on integers will return an integer, and operations on floating point numbers returns a floating point. `INT()` `FLOOR()` and `FLOAT()` can convert types as desired.

### Conditional Blocks

In addition to `if` statements covered earlier, `if else` and `else` blocks can also be implemented.

```
{
    // if x is 0 then set y to 0
	- x == 0:
		~ y = 0
    // else if x is greater than 0 set y to x - 1
	- x > 0:
		~ y = x - 1
    // else set y to x + 1
	- else:
		~ y = x + 1
}
```

Conditional blocks are not limited to logical evaluations, they can also be used to gate narrative content, and can even include choices (but not gather points, so these should remain fairly concise).

### Switch Blocks

Switch statements are also available for enumeration:

```
{ x:
- 0: 	zero
- 1: 	one
- 2: 	two
- else: lots
}
```

### Multiline Alternatives

You can use curly braces and switch block style syntax (without the `else` statement) to write out alternatives. In place of an initial variable that is evaluated, you instead use alternative keywords like `stopping` `shuffle`, etc. There are also some special types of shuffle types available this way: `shuffle once` that shuffles once and then turns into a standard sequencer, and `shuffle stopping` which shuffles, but then stops at the last element.

### Temporary Variables

The tilde character not only performs operations on variables, but can also be used to assign temporary variables that are discarded after leaving the stitch or knot it is defined in.

### Knot and Stitch Parameters

As mentioned earlier, knots and stitches can accept parameters, which increases the amount of narrative composition possible.

```
* [Get a dog] -> pet_store("dog")
* [Get a cat] -> pet_store("cat")
* [Get a rabbit] -> pet_store("rabbit")

=== pet_store(pet) ===
I got a {pet} from the store.
```

You can use parameters to pass temp values from one stitch or knot to another. Temporary variables are also safe to use recursively.

Diverts can also be accepted as parameters, but in those cases, and those cases only, the divert must be typed, e.g. `=== travel_plans (-> travel_option) ===`.

### Functions

Parameters make knots function-like, but they are not true functions because they lack call stacks and return values.

However Ink does support functions. Functions are written as knots, but with the keyword `function` preceding the name of the function. They have the following characteristics:

- They do not contain stitches
- They don't use diverts or offer choices
- They can call other functions
- They can include printed content
- They can return a value of any type
- They can recurse

Return statements are provided with `~ return <expression>`.

#### Parameters Can be Passed by Reference

Functions can use the parameter's referent instead of creating a temporary variable by using the `ref` keyword. So:

```
=== function add(ref x, y) ===
    ~ x = x + y
```
This updates the value of `x` by adding the variable `y` to it.

### Constants

Constants can also be declared using the `CONST` syntax.

## ADVANCED FLOW CONTROL

### Tunnels

Tunnels allow for substories that allow for more intricate story branching. The syntax is: `-> next_story ->`. This means that the story diverts to `next_story` and then returns here. That way there are lots of ways you could get to `next_story` without having to worry about keeping track of where the user originally came from.

Multiple tunnels can be chained together.

`-> second_story -> third_story ->`

Tunnels use a call stack so they can recurse.

### Threads

Threads allow knots and stitches to be integrated into the current knot/stitch using the syntax `<-`. When using threads you should also use the keyword `-> DONE` which indicates that a particular knot or stitch is complete, and it should start piping in threaded content.

## ADVANCED STATE TRACKING (LISTS)

### Lists

Lists in Ink are a list of states. They are declared this way:

`LIST timeOfDay = morning, noon, afternoon, evening, night`

Use the tilde to declare what value for the list to take.

`~ timeOfDay = afternoon`

At this point you can query the value.

```
{ timeOfDay == night:
    It was too dark to see.
- else:
    Light poured in through the window.
}
```
You can use `()` around one value from a list to assign a default state when assigned.

Current state increments with `++` and decrements with `--`.

### Reusing Lists

Elements of a list can be assigned to variables. That allows the same state to be used in multiple places. Lists can also have the same element. In those cases when assigning an element to a variable you will need to use the list name and dot notation.

### List Values

`{LIST(element)}` will produce a number.

`{LIST(index)}` will produce a value.

### Multivalued Lists

Lists are boolean sets. So when earlier we initialized a starting value, there was one element present in the list. In fact, multiple elements can be present.

`LIST dogsInRoom = (Frisket), (Phoebe), Pixie, (Chowder)`

This is also valid syntax:

`LIST dogsInRoom = (Frisket, Phoebe, Chowder)`

You can also assign an empty list:

`LIST futureDays = ()`

`+=` and `-=` notation can be used to update list elements.

There are several useful built-in functions for querying lists. `LIST_COUNT()` (list length), `LIST_MIN()` (first in-state element), `LIST_MAX()` (last in-state element), and `LIST_RANDOM()` (random in-state element).

Lists can also be queried to see if it possesses an element using `?` or `has` syntax, e.g.

`{ dogsInRoom ? (Frisket, Chowder)} // True`

`hasnt` or `!?` can be used for lacking elements.

`^` can be used to return any element shared among two lists.

`LIST_ALL()` counts all elements of a list, regardless of whether they are in-state or not.

`LIST_RANGE(list, min_int, max_int)` lets you create slices of a list. This can also be done with the element names instead of the integers.

`LIST_INVERT()` switches the state of the list.

### Multi-list Lists

It is possible to assign variables that include elements from multiple lists. This allows for richer tracking of state and world properties.

## Web Features

Inky provides tooling for building out an html page with the JavaScript necessary to create your piece of interactive fiction. There are a couple of features that can be utilized specifically when building for a web environment.

### Images

To insert an image you can use an image tag.

`# IMAGE: portrait.png`

The tag can also be included after a block of text.

### Audio

To add audio you can use an audio tag.

`# AUDIO: gunshot.ogg`

There is also an AUDIOLOOP tag.

`# AUDIOLOOP: track01.ogg`

### URL Links

You can create links to other pages.

`# LINK: https://itch.io`

### Background

You can specify a background image.

`# LINK: background.png`

### Clearing the Screen

To remove all currently available text you can add a clear tag, which will wipe out all preceding text.

`# CLEAR`

### Global Tags

You can specify dark mode with a tag.

`# theme: dark`

The built-in CSS has classes that will be able to handle all the changes automatically. So modifying the CSS it will be possible to add other custom themes.

`# author: Paul Jickling`

This tag can be included at the top of your main ink file and will automatically include your byline.

### Custom CSS Tags

You can add CSS classes to blocks of text.

`(c) 2023 # CLASS: footer`

And then you can write CSS to style that block of text.

```css
.footer {
    position: absolute;
    bottom: 1rem;
}
```

### Built-in CSS Class: The End
Inky has a built in class for styling the end text at the end of your interactive fiction.

```
# CLASS: end
Fin
-> END
```

### Restart

There is also a `# RESTART` tag that when reached will restart the story from the beginning.

### Editing CSS

Inky has a well laid out CSS file with nice defaults, and that makes it very straight forward to modify it as desired with very low risk of unintended behavior.

### Editing JavaScript

Any web page generated by Inky will include three JavaScript files.

1. `main.js` this script contains the logic for tags and the behavior for the presentation of text
2. `<yourStoryName>.js` is the exported ink content
3. `ink.js` is the JavaScript implementation of the ink engine

The exported ink content and `ink.js` scripts should be left alone. However should you need the main.js script is well documented and written to be extensible.

Alternatively, you could include your own JavaScript file and simply alter the `index.html` file to include the script.
