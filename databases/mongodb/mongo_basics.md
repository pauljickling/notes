# MongoDB Basics

### Terminal interface

`sudo service mongod start`

`sudo service mongod restart`

`sudo service mongod stop`

### Entering Mongo Shell

`mongo`

### Examining Databases and Collections

`show dbs`

`show collections`

`use {db/collection}`

## Structure

MongoDB is a document-oriented database rather than a relational one. When querying data MongoDB has databases, which contain collections, which contain documents, which contain key-value pairs. MongoDB documents resemble Javascript objects.

Documents always have a unique `"_id"` key. They are generated automatically.

Keys are always strings.

Collections can contain documents or sub-collections. Example of a subcollection format:

`blog.posts`
`blog.author`

Sub-collections don't have any special properties (they are just more collections), but they are useful for organization purposes.

### CRUD Interface

Using the MongoDB shell you can create a document the same way you would create an object in Javascript:

```
posts = { "title" : "My Blog Post",
        "content" : "Here's my blog post",
        "Date" : new Date()}

```

Use the insert method to add a document to a collection.

`db.blog.insert(post)`

You can query a collection using the find method.

`db.blog.find()`

To just find one document in a collection use the findOne method.

`db.blog.findOne()`

You can pass parameters to these queries to narrow the scope of the search.

Modifying a document is similar to creating one.

`posts.comments = []`

Updating a document uses two parameters, first the critera for the document being updated, and then the new document.

`db.blog.update({title: "My Blog Post"}, post1)`

Finally, to delete a document use the remove method.

`db.blog.remove({title: "My Blog Post"})`
