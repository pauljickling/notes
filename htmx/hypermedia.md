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

## Chapter 3: Components of a Hypermedia System

A hypermedia system includes the following components: a hypermedia format like HTML, a network protocol (HTTP), a server that presents a hypermedia API respnding to network requests and responses, and a client that interprets the responses.

A discussion of network requests and response codes ensues. Caching strategies are discussed, and an [article on caching](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching) is included.

### REST

REST is discussed as a network architecture that contains the following properties:
1. It is a client-server architecture
2. It is stateless
3. It allows caching
4. It contains a uniform interface
5. It is a layered system
6. It may allow for code-on-demand (i.e. scripts)

Note that session cookies, which are ubiquitous in the current internet environment, violate the stateless property of a RESTful architecture when they are used as a key lookup for information.

Layered systems means that multiple servers can act as intermediaries between a client and a server that acts as a source of truth. Thus we have architectures such as a load balancer that serves client requests to multiple servers, CDNs, etc.

Including JavaScript in an HTML page does not in and of itself violate RESTful properties as laid out here, however when JavaScript is used to replace this hypermedia model it certainly does.
