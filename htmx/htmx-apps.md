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

### Triggering HTTP Requests

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

### HTMX vs HTML Responses

One difference between traditional HTTP requests, and what happens with htmx requests is that with htmx the responses can be partial bits of html instead of a full page. This kind of HTML transfer is referred to as "transclusion", and allows for html to be embedded within existing HTML documents. Thus our button that makes a request for `contacts` can fetch the following content:

```html
<ul>
  <li><a href="mailto:joe@example.com">Joe</a></li>
  <li><a href="mailto:sarah@example.com">Sarah</a></li>
  <li><a href="mailto:fred@example.com">Fred</a></li>
</ul>
```

This is closer in appearance to how a React developer might write composable bits of UI components instead of having to write an entirely separate HTML page.

### Targeting Other Elements

To include partial HTML within an existing HTML document, it necessarily creates the question of where the content should go. By default, it will simply go within the element that triggered the request. This is typically not going to be a desired outcome. A list of contacts within a button element is going to be extremely bizarre. Instead, typically this will be specified with the `hx-target` attribute. Thus, our button will instead look like: `<button hx-get="/contacts" hx-target="#main">` Where `main` is the ID of some element in the DOM where we want this contact to live.
