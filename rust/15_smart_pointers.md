# Smart Pointers

A *pointer* is a variable that contains an address in memory. The most common type of pointer in Rust is a reference. There isn't anything special about references, they just point to data and are able to borrow that value without any overhead.

*Smart pointers* are data structures that act like pointers, but they also have additional metadata and capabilities. Another distinction is that references are pointers that borrow data, whereas smart pointers have ownership of the data they are pointing to.

Examples of smart pointers include `String` and `Vec<T>` because they own some memory that you are able to manipulate.

Smart pointers are usually implemented with structs. Unlike an ordinary struct however, a smart pointer implementation has `Deref` and `Drop` traits. `Deref` allows an instance to behave like a reference so that you can write code that works with either references or smart pointers. `Drop` allows for code customization to handle when an instance of a smart pointer goes out of scope.

The following are the most common smart pointers used in the standard library:

- `Box<T>` allocates values on the heap
- `Rc<T>` is a reference counting type that enables multiple ownership
- `Ref<T>` and `RefMut<T>` are accessed through `RefCell<T>` which is a type that enforces borrowing rules at runtime instead of compiling time.

Other concepts to be aware of are the *interior mutability* pattern where an immutable data type exposes an API for mutating an interior value, and *reference cycles* which have the potential to leak memory.

## Using `Box<T>` to Point to Data on the Heap

Boxes store data on the heap instead of the stack. What remains on the stack are pointers pointing to data in the heap. Boxes don't have performance overhead, but this means they also don't have many extra capabilities. They are useful in the following scenarios:

- You have a type whose size can't be known at compile time and you want to use a value of that type in a context that requires an exact size

- You have a large amount of data and you want to transfer ownership and make sure the data won't be copied when this happens

- You want to own a value and you only care that it's a type that implements a particular trait as opposed to a specific type

### Using a `Box<T>` to Store Data on the Heap

The syntax for a box looks like this:

`let b = Box::new(5);`

This creates a box called b that points to the value 5, which is stored on the heap. This is a trivial example. You wouldn't normally use a box to store a single value.

### Enabling Recursive Types with Boxes

The Rust compiler needs to know how much space things take up. Recursive types are an unknowable size. The nesting of values in a recursion can continue indefinitely. Boxes, on the other hand, have a known size. Boxes make recursive types possible.

#### The Cons List

Functional programming has the concept of a *cons list*. It is a data structure that originates from Lisp and its dialects. The `cons` function (short for construction function) constructs a new pair from its two arguments, forming a single value and another pair. These pairs contain pairs form a list. The FP world even has jargon around this concept, "to cons x onto y" i.e. a new container instance that places x at the start of the new container, followed by the container y. These containers continue on until the last container contains a value of `Nil`. Thus the cons list consists of a recursion that has a recursive case of a container, and a base case of `Nil`.

A cons list implementation in Rust might look like this:

```
enum List {
    Cons(i32, List),
    Nil,
}
```

Then we could do something like:

```
use List::{Cons, Nil};

fn main() {
    let list = Cons(1, Cons(2, Cons(3, Nil)));    
}
```

If you tried running this code the compiler would complain that the enum "recursive type has infinite size", and the list is "recursive without indirection". In other words, the compiler has no idea how much space to allocate. On the other hand, the compiler knows how much space a pointer/box needs. So now lets rewrite the enum:

```
enum List {
    Cons(i32, Box<List>),
    Nil,
}
```

And our function:

```
use List::{Cons, Nil};

fn main() {
    let list = Cons(1,
        Box::new(Cons(2,
            Box::new(Cons(3,
                Box::new(Nil))))));    
}
```

It's more verbose, but we now have a recursively structured list working in Rust.

## The `Deref` Trait

The `Deref` trait allows you to customize the behavior of the *dereference operator*, `*` (not to be mistaken with when the astericks is used for multiplication or as a glob operator). This allows a smart pointer to be treated as a regular reference so that you can write code that operates on references, and use that code with smart pointers too.

### Following the Pointer to the Value with the Dereference Operator

Recall that a reference points to the address that stores some value. The dereference operator is how you access that value.

```
fn main() {
    let x = 5;
    let y = &x;

    assert_eq!(5, x);
    assert_eq!(5, *y);
}
```

Similarly, you could rewrite the y variable as `let y = Box::new(x);` and the code would work the same way. Boxes and references are both types of pointers that need to be dereferenced if you want to compare some type of value.

### Defining a Smart Pointer

Defining our own smart pointer will illustrate how smart pointers differ from a reference.

```
struct SmartPointer<T>(T);

impl<T> SmartPointer<T> {
    fn new(x: T) -> SmartPointer {
        SmartPointer(x)
    }
}
```

This `SmartPointer` tuple struct has a generic parameter `T` since it should be able to hold a value of any type. The method implementation takes a `T` parameter and returns a SmartPointer instance. All that's left for this `SmartPointer` implementation is it needs to be able to dereference values. To do that we need to implement the `Deref` trait.

```
use std::ops::Deref;

impl<T> Deref for SmartPointer<T> {
    type Target = T;

    fn deref(&self) -> &T {
        &self.0
    }
}
```

`type Target = T;` defines an associated type for the `Deref` trait to use. The `deref` function returns `&self.0`, a reference to the value the `*` operator wants to access.

### Implicit Deref Coercions with Functions and Methods

*Deref coercision* is a convenience that Rust performs on arguments to functions and methods. It converts a reference to a type that implements `Deref` into a reference to a type `Deref` can convert the original type into.

Deref coercision serves two purposes. One is to reduce the need to explicitly reference and dereference. It also makes it possible to write code that works for either references or smart pointers.

What does this all mean? Take a simple hello function:

```
fn hello(name: &str) {
    println!("Hello {}", name);    
}
```

Without deref coercion you would have to write something like this for our `SmartPointer` to work:

```
fn main() {
    let name = SmartPointer::new(String::from("Paul"));
    hello(&(*name)[..]);
}
```

Instead we can write this:

```
fn main() {
    let name = SmartPointer::new(String::from("Paul"));
    hello(&name);
}
```

### How Deref Coercion Interacts with Mutability

The `DerefMut` trait can be used to overide the `*` operator on mutable references.

Rust does deref coercion for type and trait implementations in three cases:

- From `&T` to `&U` when `T: Deref<Target=U`
- From `&mut T` to `&mut U` when `T: DerefMut<Target=U`
- From `&mut T` to `&U` when `T: Deref<Target=U>`

The first two cases work the same way, one just handles immutable references, and the other mutable references. The third case involves coercion from a mutable reference to an immutable one. Note that the reverse is not possible because of borrowing rules.

## The `Drop` Trait

`Drop` customizes what happens when a value is about to go out of scope. The `Drop` trait can be implemented on any type, but it is typically used when implementing smart pointers.

```
struct CustomSmartPointer {
    data: String,
}

impl Drop for CustomSmartPointer {
    fn drop(&mut self) {
        println!("Dropping CustomSmartPointer with data `{}`!", self.data);
    }
}

fn main() {
    let c = CustomSmartPointer { data: String::from("my stuff") };
    let d = CustomSmartPointer { data: String::from("other stuff") };
    println!("CustomSmartPointers created.");
}

```

This illustrates how `drop` is handled. Typically the cleanup code needed by the data type would be specified instead of a print message.

### Dropping a Value Early with `std::mem::drop`

There is no straightforward way to disable the automatic `drop` functionality. Luckily this is something you would rarely need to do. One potential use is a smart pointer that manages locks where you want to force the `drop` that releases the lock to run so other code in the same scope can acquire the lock. To do that, you need to call the `std::mem::drop` function.

## `Rc<T>`

Ownership is usually clear, but there are cases when a single value may have multiple owners. For example, in a graph multiple edges might point to the same node. In an instance like this a node shouldn't be cleaned up unless there are no edges pointing to it.

`Rc<T>` enables multiple ownership that is short for *reference counting*. It keeps track of the number of references to a value to determine if that value is still in use. When the number of references to that value drops to zero the value is cleaned up without any references becoming invalid.

`Rc<T>` is only for single-threaded scenarios. For multithreaded programs there are other ways to handle reference counting.

## `RefCell<T>`

*Interior mutability* is a design pattern in Rust that mutates data even when there are immutable references to that data. Normally this action is disallowed by the borrowing rules. The pattern uses `unsafe` code inside a data structure to bend the rules.

### Enforcing Borrowing Rules at Runtime with `RefCell<T>`

`RefCell<T>` represents single ownership over the data it holds. Unlike the `Box<T>` type, `RefCell<T>` enforces borrowing rules at runtime rather than at compiling time.

Generally it is preferable to check borrowing rules at compiling time so errors are caught sooner. However there are some memory-safe operations that are possible at runtime that would be disallowed by a compile-time check.

### Interior Mutability: a Mutable Borrow to an Immutable Value

There are situations where it is useful for a value to be able to mutate, but appear immutable to other code.

#### A Use Case for Interior Mutability: Mock Objects

A type used in place of another type during testing is known as a *test double*. *Mock objects* are specific types of test doubles that record what happens during a test so you can assert that the correct action took place.

## Reference Cycles Can Leak Memory

It's possible to create references where items refer to each other in a cycle. This creates memory leaks because the reference count of each item in the cycle will never hit 0, and so the references will never drop.

### Creating a Reference Cycle

Imagine a `List` enum acting as a linked list that takes an `i32` type along with a `RefCell` that contains a `Rc` that contains a `List`. First node a is inserted into the list, and it contains the values `5` and `Nil`. Then node b is added that contains the values `10` and a pointer to node a. Then node a is modified so that it points to node b instead of `Nil`. This creates a cycle so the head points to the tail and the tail points to the head.

A simple two node linked list like this creating a reference cycle is not a significant problem, but for more complex programs this has the potential to eat up a lot of memory.

Creating reference cycles isn't something that is easily done in Rust, but it can happen with these nested data structures with interior mutability.

### Preventing Reference Cycles: Turning an `Rc<T>` into a `Weak<T>`

It is possible to create a *weak reference* to the value within a `Rc<T>` by calling `Rc::downgrade`, this creates a smart pointer of type `Weak<T>`. The difference between a strong reference and a weak reference is that weak references don't need to be 0 for a `Rc<T>` instance to be cleaned up.

Strong references are how a `Rc<T>` instance can share ownership. Weak references do not express an ownership relationship. To do anything with with a weak reference you need to call the `upgrade` method which will return an `Option<RC<T>>`. If the `Rc<T>` value hasn't been dropped yet, it will return a `Some` result.

### Creating a Tree Data Structure: a `Node` with Child Nodes

A tree node needs to have children, and should share that ownership with variables so each node in the tree can be accessed directly. This can be accomplished with a vector with `Rc<Node>` elements.

```
use std::rc::Rc;
use std::cell::RefCell;

#[derive(Debug)]
struct Node {
    value: i32,
    children: RefCell<Vec<Rc<Node>>>,
}

fn main() {
    let leaf = Rc::new(Node {
        value: 3,
        children: RefCell::new(Vec![]),
    });

    let branch = Rc::new(Node {
        value: 5,
        children: RefCell::new(vec![Rc::clone(&leaf)],)
    });
}
```

`leaf` is a node with a value of 3 and no children, and branch is is a node with a value of 5 and leaf as one of its children. Because of the `Rc<Node>` clone the node in leaf has two owners: `leaf` and `branch`.

### Adding a Reference from a Child to Its Parent

This dual ownership doesn't really reflect a child parent relationship however. This is where a weak reference comes in handy. The tree node can be adjusted to include that:

```
struct Node {
    value: i32,
    parent: RefCell<Weak<Node>>,
    children: RefCell<Vec<Rc<Node>>>,
}
```

Now a node can refer to the parent without taking ownership of it.
