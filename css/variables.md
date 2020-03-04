# Defining Variables in CSS

It used to be that to define variables (or custom properties in CSS-speak) you needed to use a preprocessor like SASS or LESS. However it is now possible to define variables in plain CSS.


To declare the variable:
```
element {
  --main-bg-color: brown;
}
```

To use the variable:

```
element {
  background-color: var(--main-bg-color);
}
```

Like many newer CSS features, it is unavailable in Internet Explorer, but if that is not a target browser custom properties can be a nice way to simplify your stylesheet.

## Imports

If you are working with multiple CSS files it can be useful to have a file that contains your variables that you import into your other files. You can do this by including `@import "{relative/path/to/file}"` at the top of your file.
