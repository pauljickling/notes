# Hypermedia

Notes on the book that can be found on [hypermedia.systems](hypermedia.systems).

## Introduction

A hypermedia system is any system that adheres to a RESTful (Representational State Transfer) network architecture. The author emphasizes that they are not talking about JSON APIs that are often referred to as RESTful APIs since JSON lacks hypermedia controls. Instead it refers to a network where the exchange of hypermedia is built into the system.

## Chapter 1: Reintroduction

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

## Chapter 2: Components of a Hypermedia System

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

## Chapter 3: A Web 1.0 Application

To demonstrate the advantage of hypermedia architecture consider the hypothetical of building out a contact management web app that has CRUD capabilities called Contact.app.

### Tech Stack

Backend written in Python using the Flask framework and Jinja2 templating. Using this stack the goal will be for the server side to render HTML to return to clients instead of JSON.

### Intro to Flask

Flask consists of routes tied to functions that execute HTTP requests. It utilizes Python decorators to achieve this. A basic hello world page looks like this:

```python
@app.route("/")
def index():
    return "hello world"
```

Sometimes you want to redirect the root of a web page, which is easier to type in as a URL, to the actual application. The above code can be rewritten that way.

```python
@app.route("/")
def index():
    return redirect("/contacts")
```

### Contact.app Functionality

- Allows users to view a list of contacts
- Search the contacts
- Add a new contact
- View the details of a contact
- Edit the details of a contact
- Delete a contact

The "contacts/" route should show a searchable list of contacts. It will also handle a case of a search term that filters the contact list.

```python
@app.route("/contacts")
def contacts():
    search = request.args.get("q")
    if search is not None:
        contacts_set = Contact.search(search)
    else:
        contacts_set = Contact.all()
    return render_template("index.html", contacts=contacts_set)
```

The `search` variable checks the request's arguments to see if there is a query (i.e. `"q"`). If there is, it assigns a variable `contacts_set` to a `Contact` object that uses the `search` method, and uses the `search` variable (e.g. the params) as it's arguments. Otherwise it uses the `Contact` object's `all` method to retrieve all objects.

The route returns a template that uses the index.html file, and uses a `contacts` param assigned to the `contacts_set` variable.

This means that now an html template will be needed to handle this request. First lets build out the search form.

```
{% extends 'layout.html' %}

{% block content %}

  <form action="/contacts" method="get" class="tool-bar">
    <label for="search">Search Term</label>
    <input id="search" type="search" name="q"
      value="{{ request.args.get('q') or '' }}" />
    <input type="submit" value="Search"/>
  </form>
```

Next comes the contact table.

```
<table>
  <thead>
  <tr>
    <th>First <th>Last <th>Phone <th>Email <th/>
  </tr>
  </thead>
  <tbody>
  {% for contact in contacts %}
    <tr>
      <td>{{ contact.first }}</td>
      <td>{{ contact.last }}</td>
      <td>{{ contact.phone }}</td>
      <td>{{ contact.email }}</td>
      <td><a href="/contacts/{{ contact.id }}/edit">Edit</a>
        <a href="/contacts/{{ contact.id }}">View</a></td>
    </tr>
  {% endfor %}
  </tbody>
</table>
```

Our contact table provides routes for other features of our contact app: viewing and editing contacts. We also need to provide the ability to add a new contact.

```
<p>
    <a href="/contacts/new">Add Contact</a>
  </p>

{% endblock %}
```

Now we need to build out the functionality for these additional features. Starting with adding a contact.

```python
@app.route("/contacts/new", methods=['GET'])
def contacts_new_get():
    return render_template("new.html", contact=Contact())
```

Once again, we need an additional template.

```
<form action="/contacts/new" method="post">
  <fieldset>
    <legend>Contact Values</legend>
    <p>
      <label for="email">Email</label>
      <input name="email" id="email" 
        type="email" placeholder="Email"
        value="{{ contact.email or '' }}">
      <span class="error">
        {{ contact.errors['email'] }}
      </span>
    </p>
    <p>
      <label for="first_name">First Name</label>
      <input name="first_name" id="first_name" type="text"
        placeholder="First Name" value="{{ contact.first or '' }}">
      <span class="error">{{ contact.errors['first'] }}</span>
    </p>
    <p>
      <label for="last_name">Last Name</label>
      <input name="last_name" id="last_name" type="text"
        placeholder="Last Name" value="{{ contact.last or '' }}">
      <span class="error">{{ contact.errors['last'] }}</span>
    </p>
    <p>
      <label for="phone">Phone</label>
      <input name="phone" id="phone" type="text" placeholder="Phone"
        value="{{ contact.phone or '' }}">
      <span class="error">{{ contact.errors['phone'] }}</span>
    </p>

    <button>Save</button>
  </fieldset>
</form>

<p>
  <a href="/contacts">Back</a>
</p>
```

Now that we have a form for creating a new contact, we need a route that can handle the POST request.

```python
@app.route("/contacts/new", methods=['POST'])
def contacts_new():
    c = Contact(
      None,
      request.form['first_name'],
      request.form['last_name'],
      request.form['phone'],
      request.form['email'])
    if c.save():
        flash("Created New Contact!")
        return redirect("/contacts")
    else:
        return render_template("new.html", contact=c)
```

This defines a contact variable `c`, using the `Contact` object, and either saves it and redirects to `contacts/` or else it returns to the `new.html` contact page.

Contacts will have some sort of ID value associated with them, so that the URL routes for viewing them will be something like `/contact/42` or some other integer. This requires additional routing.

```python
@app.route("/contacts/<contact_id>")
def contacts_view(contact_id=0):
    contact = Contact.find(contact_id)
    return render_template("show.html", contact=contact)
```

The show.html page can look like this:

```
<h1>{{contact.first}} {{contact.last}}</h1>

<div>
  <div>Phone: {{contact.phone}}</div>
  <div>Email: {{contact.email}}</div>
</div>

<p>
  <a href="/contacts/{{contact.id}}/edit">Edit</a>
  <a href="/contacts">Back</a>
</p>
```

From here we can edit a contact.

```python
@app.route("/contacts/<contact_id>/edit", methods=["GET"])
def contacts_edit_get(contact_id=0):
    contact = Contact.find(contact_id)
    return render_template("edit.html", contact=contact)
```

And now the edit template.

```
<form action="/contacts/{{ contact.id }}/edit" method="post">
  <fieldset>
    <legend>Contact Values</legend>
    <p>
      <label for="email">Email</label>
      <input name="email" id="email" type="text"
        placeholder="Email" value="{{ contact.email }}">
      <span class="error">{{ contact.errors['email'] }}</span>
    </p>

    <p>
      <label for="first_name">First Name</label>
      <input name="first_name" id="first_name" type="text"
        placeholder="First Name" value="{{ contact.first }}">
      <span class="error">{{ contact.errors['first'] }}</span>
    </p>
    <p>
      <label for="last_name">Last Name</label>
      <input name="last_name" id="last_name" type="text"
        placeholder="Last Name" value="{{ contact.last }}">
      <span class="error">{{ contact.errors['last'] }}</span>
    </p>
    <p>
      <label for="phone">Phone</label>
      <input name="phone" id="phone" type="text"
        placeholder="Phone" value="{{ contact.phone }}">
      <span class="error">{{ contact.errors['phone'] }}</span>
    </p>
    <button>Save</button>
  </fieldset>
</form>

<form action="/contacts/{{ contact.id }}/delete" method="post">
  <button>Delete Contact</button>
</form>

<p>
  <a href="/contacts/">Back</a>
</p>
```

Now we need to handle the POST request.

```python
@app.route("/contacts/<contact_id>/edit", methods=["POST"])
def contacts_edit_post(contact_id=0):
    c = Contact.find(contact_id)
    c.update(
      request.form['first_name'],
      request.form['last_name'],
      request.form['phone'],
      request.form['email'])
    if c.save():
        flash("Updated Contact!")
        return redirect("/contacts/" + str(contact_id))
    else:
        return render_template("edit.html", contact=c)
```

This is, of course, essentially the same logic as our POST request to add a new contact, only now it is looking up an ID and updating it in-place.

Finally, it is time to handle deleting a contact.

```python
@app.route("/contacts/<contact_id>/delete", methods=["POST"])
def contacts_delete(contact_id=0):
    contact = Contact.find(contact_id)
    contact.delete()
    flash("Deleted Contact!")
    return redirect("/contacts")
```
