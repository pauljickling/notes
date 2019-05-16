## Hash Tables

Hash tables make use of hash functions to produce an array where it is indexed by a string rather than by sequential numbers. Hash tables sometimes are also referred to as hash maps, maps, dictionaries, associative arrays.

### Hash function

A hash function is a function that inputs a string, and returns a number. The only requirement for the hash function is a key/value pairing where the key is the string, and the value is a number.

### Efficiency

The efficiency of a properly managed hash table is O(1). However an improperly managed hash table is O(n). Hash tables need to manage the array size for all the key/value pairs. Key/value pairs that have to go into the same index location are joined in a linked list, and if the items passed to linked lists are not balanced the hash table will have all the inefficiencies of a linked list with none of the benefits. In practice, most hash table implementations simply readjust the size of the array. For example, [read this implementation in Python](https://mail.python.org/pipermail/python-list/2000-March/048085.html).

### Practical Implementations

Most languages have built in hash tables, for example Python's dictionaries. So there typically isn't any need to reinvent the wheel. One notable exception is JavaScript. In JavaScript typically one relies on object literals for a data structure that approximates hash tables, however because of JavaScript's prototypal inheritance system this can lead to some unintended consequences. To implement a proper hash table you need to implement a *bare object*. A bare object is an object that lacks the prototypal inheritance of the JavaScript Object.

`let hashMap = Object.create(null);`

Read more about bare objects [here](http://ryanmorr.com/true-hash-maps-in-javascript/).
