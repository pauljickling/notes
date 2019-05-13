## Objects

Objects group together various properties and methods that are only accessible via reference to that object. *Properties* are key/value pairs where the key is always a string, and the value is any valid type of JavaScript value. When objects contain functions these functions are called *methods*. JavaScript provides a CRUD (Create, Read, Update, Delete) interface for interacting with object properties and methods.

### Creating Objects

JavaScript has two methods of creating objects: *literal notation* and *constructor notation*.

#### Literal Notation

`let book = { title: 'Los Perros Romanticos', author: 'Roberto Bolano' };`

#### Constructor Notation

There are in fact two ways to use constructor notation. One method is to create a variable that is a new Object. However using JavaScript's system of prototypal inheritance it is also possible to create new kinds of objects.

New Object Technique:

```
let book = new Object();
book.title = 'Fleurs du Mal';
book.author = 'Charles Baudelaire';
```

New Prototype Technique:

```
function Book(title, author) {
  this.title = title;
  this.author = author;
}

let book = new Book('Twenty-one Love Poems', 'Adrienne Rich');
```

#### ES6 Class Syntax

ES6 provides a class syntax for constructing objects. Although JavaScript is a language that features prototypal inheritance rather than classical inheritance, the class syntax provides a way to create prototypal objects that will be familiar to programmers that come from an object-oriented background where a class is distinct from its object instances (while in fact no such distinction exists in JavaScript).

```
class Book {
  constructor(title, author) {
    this.title = title;
    this.author = author;
  }
}
```

### Reading Object Properties

Objects can be accessed using either dot notation or with strings in brackets.

```
console.log(book.author);
console.log(book['title']);
```

### Updating Object Properties

Dot and bracket notation both work for updating objects.

```
let score = { home: 0, away: 0 };
score.home++;
score['home']++;
console.log(score); // { home: 2, away: 0 }
```

### Deleting Object Properties

Properties can be removed from objects using the delete keyword.

```
// Rendering the book's author anonymous
delete book.author;
```
