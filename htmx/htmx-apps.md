# Chapter 4: Extending HTML As Hypermedia

Limitations of the contacts app built out in the last chapter:

- For the user there is a noticable refresh moving between pages. Each interaction requires fetching and rendering a new HTML document.
- All updates are handled with HTTP POST.

Historically these limitations have driven web developers towards building single page applications that rely on JavaScript to mutate DOM elements as needed to handle application logic and updates.

HTMX offers an alternative approach to solve these limitations. As mentioned in a prior chapter, the only HTML elements that can issue HTTP requests are anchor tags and forms. HTMX extends these properties to HTML in a more generalized way so that other HTML elements, events, and HTTP requests types become available tools for a web developer.

Additionally, there is no requirement that a new link should be required to load a whole new page. Instead, it is possible to replace elements within the current document.

## Extending HTML as Hypermedia with HTMX

To use htmx in your web applications, you simply have to load the library:

```html
<head>
    <script src="https://unpkg.com/htmx.org@2.0.1" crossorigin="anonymous"></script>
</head>
```

Although htmx is a JavaScript library, no knowledge of JavaScript is required to use it. HTML has simply been extended to allow the developer to do things in HTML that previously they would have done using JavaScript.

## Triggering HTTP Requests

Any element can issue HTTP requests. They are as follows:

- `hx-get`
- `hx-post`
- `hx-put`
- `hx-patch`
- `hx-delete`

Whenever a user interacts as intended with that element, the following HTTP request type is issued. So if we wanted a button that gets a list of contacts, we can write it this way:

```html
<button hx-get="/contacts">Get Contacts</button>
```

## HTMX vs HTML Responses

One difference between traditional HTTP requests, and what happens with htmx requests is that with htmx the responses can be partial bits of html instead of a full page. This kind of HTML transfer is referred to as "transclusion", and allows for html to be embedded within existing HTML documents. Thus our button that makes a request for `contacts` can fetch the following content:

```html
<ul>
  <li><a href="mailto:joe@example.com">Joe</a></li>
  <li><a href="mailto:sarah@example.com">Sarah</a></li>
  <li><a href="mailto:fred@example.com">Fred</a></li>
</ul>
```

This is closer in appearance to how a React developer might write composable bits of UI components instead of having to write an entirely separate HTML page.

## Targeting Other Elements

To include partial HTML within an existing HTML document, it necessarily creates the question of where the content should go. By default, it will simply go within the element that triggered the request. This is typically not going to be a desired outcome. A list of contacts within a button element is going to be extremely bizarre. Instead, typically this will be specified with the `hx-target` attribute. Thus, our button will instead look like: `<button hx-get="/contacts" hx-target="#main">` Where `main` is the ID of some element in the DOM where we want this contact to live.

## Swap Styles

When targetting an element, the html fragment will become a child of the targetted element. However, depending on the DOM and CSS structure, it may be preferable to swap out elements. In that case `hx-swap` would be the attribute to use. Unlike `hx-target` however, `hx-swap` has an enumerated set of values that are valid:

- `innerHTML`: default, replacing inner html of target element
- `outerHTML`: replacing the entire target element with the response
- `beforebegin`: insert the response before the target element
- `afterbegin`: insert the response before the first child of the target element
- `beforeend`: append the response after the last child of the target element
- `afterend`: append the response after the target element
- `delete`: deletes the target element regardless of response
- `none`: no swap performed

## Using Events

Typically buttons are used for events, specifically the click event since that is what buttons are there for, to be clicked on. Like with other parts of HTML that have nice features, in htmx these kinds of properties have been generalized, in this case with the `hx-trigger` attribute. For things like buttons, and forms that already have default events there is no need for `hx-trigger`, but it allows us to extend HTML. You can also use `hx-trigger` to modify event behavior, even if this isn't a particularly good idea. So, for example, you could change the behavior of a button where it triggers when the mouse enters it.

```html
<button 
    hx-get="/contacts"
    hx-target="#main"
    hx-swap="outerHTML"
    hx-trigger="mouseenter">
        Get Contacts
</button>
```

More practically, we might extend our app with the keyboard shortcut `ctrl-l` to load contacts. `hx-trigger` supports a comma separated list. So we can include keyboards with `hx-trigger="click, keyup"`. This will listen for any `keyup` event, so now we need a way to filter through `keyup` events. Filters are applied like so: `hx-trigger="click, keyup[ctrlKey && key == 'l]`.

If we applied this `hx-trigger` attribute to our button, it should be noted that only keyup events within the button will trigger the request. This behavior is derived from JavaScript's event bubbling model. `hx-trigger` supports the ability to listen to other elements for events by adding a `from:` modifier that specifies the target element. In the case of a keyboard shortcut, the body element makes the most sense since you will want it to always be listening. So our final trigger attribute looks like `hx-trigger="click, keyup[ctrlKey && key == 'l'] from:body`.

## Passing Request Parameters

HTML forms are powerful, but also have lots of limitations that make them frustrating to work with. htmx adds flexibility by providing the `hx-include` attribute that allows the inclusion of various values within a request. So the search filter for the contacts app could be modified in this way:

```html
<div id="main">
  <label for="search">Search Contacts:</label>
  <input id="search" name="q" type="search" placeholder="Search Contacts">
  <button hx-post="/contacts" hx-target="#main" hx-include="#search">
    Search The Contacts
  </button>
</div>
```

`hx-include` takes a CSS selector value, and allows you to specify which values to send along with the request, in this case the input field that is used for the search filter.

Note that most attrbiutes that accept CSS selector values also support relative CSS selectors, e.g. `closest::`, `next::`, `previous::`, `this::`, etc.

## Inline Values

An additional way to include values in an htmx request is the `hx-vals` attribute, which allows for static values in the request. If you have additional information that should be included in a request, you could use this instead of the more traditional hack-y approach of including hidden inputs.

An example would look like this: `<button hx-get="/contacts" hx-vals='{"state":"MD"}'>Get Maryland Contacts</button>`

`hx-vals` accepts a JSON value to generate the following URL param: `/contacts?state=MD`.

In addition to JSON, you could pass JavaScript values with the `js` prefix, e.g.: `<button hx-get="/contacts" hx-vals'js:{"state": getCurrentState}'>Get Selected State Contacts</button>`

## History Support

One advantage htmx applications have over SPAs is that it doesn't break the browser's history API without a lot of extra work on the part of the developers. You probably will need to work with that API, but it will be less burdensome overall.

For example, when using the `hx-target` attribute it will load the content (`contacts/`) but it will not create a new history entry. If we want to add a new history entry, we use the `hx-push-url` attribute, which accepts a boolean value, so you can just set it to `"true"` and the `contacts` page will be added to the user's browser history. Note that one case that still needs to be handled is when a user refreshes their browser, there needs to be a mechanism to handle reloading partial html fragments. This will be covered in a future chapter.

# Chapter 5: HTMX Patterns

## AJAX-ifying the App

Adding the `hx-boost="true"` attribute to anchor and form tags allows them to issue AJAX requests. This helps speed up the app since it will only have to replace `body` content instead of full page reloads that also evaluate `head`, etc. That means it doesn't have to reload CSS and JS content, for example, which tends to be heavier than plain HTML.

Another advantage is this will avoid the issue that crops up with applications where a user makes a change to application state, and they are confronted with a flash of unstyled content from when the content becomes available, but before the rest of the document is available to be rendered appropriately.

## Attribute Inheritance

Adding the `hx-boost="true"` attribute to every single tag is unnecessary. You can apply it to a parent tag and it will apply to all it's children. And if you'd like one link that doesn't inherit this property you can simply overwrite it with `hx-boost="false"`. As such, it typically makes sense to include the `hx-boost` attribute to the `body` tag.

## Progressive Enhancement

`hx-boost` is a progressive enhancement. Your application will still work even if a user has a browser client that doesn't have JavaScript enabled.

## A Note on Modals

Modals are a popular UI element in many web applications, but introduces client-side state that can create difficulties when developing a hypermedia-based apllication.

They are okay when they are used for views that aren't a resource that is altered in the application, so it is okay in the following use-cases:
- Alerts
- Confirmation dialogs
- Forms for creating/updatiing entities

Otherwise they should be avoided, and inline editing or a separate page would be preferable.

Similarly, be careful with the CSS attribute `display: none` as this removes the element from the DOM, which impacts accessibility features. A work around is to include the following `vh` (visually hidden) class that you can attach to your elements:

```css
.vh {
    clip: rect(0 0 0 0);
    clip-path: inset(50%);
    block-size: 1px;
    inline-size: 1px;
    overflow: hidden;
    white-space: nowrap;
}
```

These CSS attributes should be consistent across different browsers.

# Chapter 6: More HTMX Patterns

## Active Search Pattern

Active Search is when a user types text into a search box, and results are dynamically shown. The first step is to add an HTTP GET request whenever the input element receives a keyup event.

```html
<form action="/contacts" method="get" class="tool-bar">
  <label for="search">Search Term</label>
  <input id="search" type="search" name="q"
    value="{{ request.args.get('q') or '' }}"
    hx-get="/contacts"
    hx-trigger="search, keyup delay:200ms changed"
    hx-target="tbody"
    hx-select="tbody tr"/>
  <input type="submit" value="Search"/>
</form>
<table>
  <tbody>
  </tbody>
</table>
```

This resolves many issues. `hx-get` fetches the html content of the `contacts` route. `hx-trigger` creates a trigger for our search input element that uses a 200ms keyup delay, and targets an empty `tbody` element below. Finally, `hx-select` selects the appropriate row of `contacts` route instead of the entire page. That being said, there are more efficient ways to implement this feature if we make server side changes to only serve the needed HTML content.

## HTTP Request Headers in HTMX

TODO
