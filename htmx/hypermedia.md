# Hypermedia

Notes on the book that can be found on [hypermedia.systems](hypermedia.systems).

## Chapter 1: Introduction

A hypermedia system is any system that adheres to a RESTful (Representational State Transfer) network architecture. The author emphasizes that they are not talking about JSON APIs that are often referred to as RESTful APIs since JSON lacks hypermedia controls. Instead it refers to a network where the exchange of hypermedia is built into the system.

## Chapter 2: Reintroduction

Hypermedia includes non-linear branching from one location in media to another. Hypermedia control is an element that describes an interaction by encoding information about that interaction directly and completely within itself. The most obvious example of this is an html link where it contains an html element contained within the existing piece of media as well as a url that describes the interaction that changes the state of the browser.

Hypertext is a subset of hypermedia systems. HTML is the most successful form of hypertext.

HTML allowed interactivity via anchor and form tags to update state. However HTML never continued developing beyond these features that were implemented back in the mid 90s. To fill in the gap as user demands became increasingly sophisticated were JavaScript single page applications relying on a JSON data layer to manage UI changes. Although these applications are good at what they do, and serve a valuable purpose, it has also introduced an enormous amount of technical complexity to serving web applications.

This book proposes that returning to a hypermedia system can reduce this complexity while still providing these modern web experiences using tools like htmx. It defines these new types of applications as Hypermedia-Driven Applications (HDA): "A web application that uses _hypermedia_ and _hypermedia_ exchanges as its primary mechanism for communicating with a server."

Here is sample HTMX:

```
<button hx-get="/contacts/1" hx-target="#contact-ui"> <1>
    Fetch Contact
</button>
```

This sends a GET request to `/contacts/1` and replaces the target element with the content received from the GET request.
