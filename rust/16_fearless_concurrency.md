# Fearless Concurrency

*Concurrent programming* is where different parts of a program execute independently, and *parallel programming* is where different parts of a program execute at the same time. In Rust these concepts are discussed together as concurrency problems because the ownership system helps resolve both these issues.

## Using Threads to Run Code Simultaneously

When code is executed, it is run in a *process*. A process can also have multiple independent sub-processes that run simultaneously called *threads*.

Multithreaded programming can improve performance, but adds programming complexity. Since there is no guarantee about the order the code runs in the following potential problems can arise:

- Race Conditions
- Deadlocks
- Hard to reproduce bugs

### Creating a New Thread with `spawn`

A new thread can be created by calling the `thread::spawn` function, and passing the code that will be run as a closure.

The `thread::sleep` function with a `Duration` parameter can be used to delay with a thread is executed, although how this is scheduled will depend on the operating system.

### Waiting for All Threads to Finish Using `join` Handles

Spawned threads not getting to run to completion can be fixed by saving the return value from `thread::spawn` in a variable. The return type is `JoinHandle`, an owned value that, when the `join` method is called on it, will wait for its thread to finish.

There are some subtle considerations with where the `join` method is called that will affect whether the threads run simulateously or not.

### Using `move` Closues with Threads

The `move` closure is frequently used with `thread::spawn` since it allows data from one thread to be used in another.

## Using Message Passing to Transfer Data Between Threads

*Message Passing* is where threads communicate with ech other by passing messages containing data. Rust achieves message-sending concurrency with the *channel*, which is included in the standard library.

The channel consists of a *transmitter* and a *receiver*. If either the transmitter or receiver is dropped, the channel is said to be *closed*

```
use std::thread;
use std::sync::mpsc;

fn main() {
    let (tx, rx) = mpsc::channel();    

    thread::spawn(move || {
        let value = String::from("hi");
        tx.send(value).unwrap();
    });

    let received = rx.recv().unwrap();
    println!("Got {}", received);
}
```

`mpsc` stands for *multiple producer, single consumer*. In other words, this channel has multiple transmitters, but a single receiver for messages. The `mpsc::channel` function returns a tuple that contains a transmitter value (`tx`) and a receiver value (`rx`).

The spawned thread moves the transmitter into a closure so that it has ownership of it.A spawned thread needs to own the transmitter portion of the channel to send messages through the channel.

The transmitter send method returns a `Result<T, E>` type. The receiver has two useful methods: `recv` (receive), and `try_recv`. `recv` blocks the main thread's execution, waiting until a value is sent through the channel. `try_recv` doesn't block the main thread, and returns a `Result` immediately.

### Channels and Ownership Transference

Without the ownership rules errors in concurrency are much more likely. When a value moves from a transmitter to a receiver, the receiver takes ownership of it, and so if there is some expression that attempts to use that value, the compiler will complain.

### Sending Multiple Values and Seeing the Receiver Waiting

It is possible to send a vector (or other compound type) of data to the receiver, and ti will act as an iterator instead.

### Creating Multiple Producers by Cloning the Transmitter

Since a channel can have multiple producers in Rust, you can clone the transmitter in order to have multiple message senders.

`let tx1 = mpsc::Sender::clone(&tx);`

## Shared-State Concurrency

Message channels allow for concurrent threads to operate without running afoul of ownership rules. On the other hand, shared memory concurrency is like values having multiple ownership where each thread can access the same memory location at the same time. Smart pointers can make this possible, but it makes managing ownership more complex.

### Using Mutexes to Allow Access to Data from One Thread at a Time

*Mutex* is an abbreviation for *mutual exclusion*. A mutex allows only one thread to access some data at any given time. To access the data in a mutex, a thread must signal that it wants access by asking to acquire the mutex *lock*. The mutex lock is a data structure that tracks of what currently has exclusive access to the data.

There are two rules to mutexes:

1. The lock must be acquired before using the data
2. Once finished, the data must be unlocked so other threads can acquire the lock

#### The API of `Mutex<T>`

```
use std::sync::Mutex;

fn main() {
    let m = Mutex::new(5);

    {
        let mut num = m.lock().unwrap();
        *num = 6;
    }

    println!("m = {:?}", m);
}
```

In this example a new mutex, `m` is created that contains a value of 5. Then the variable num acquires the lock for m, and transforms the value into 6.

The call to lock returns a smart pointer called `MutexGuard` that implements `Deref` to point to the inner data. `MutexGuard` also has a `Drop` implementation that releases the lock when it goes out of scope. That's why the mutable `num` variable was scoped the way it was. It prevents accidentally forgetting to release the lock after it has been acquired.

#### Sharing a `Mutex<T>` Between Multiple Threads

One complication with sharing a mutex between multiple threads is the compiler will complain when the mutex's lock value is moved into the closure for the spawned thread.

#### Multiple Ownership with Multiple Threads

Recall that the smart pointer `Rc<T>` is used to give a value multiple owners by creating a reference counted value. One might think that the `Mutex<T>` can be wrapped in a `Rc<T>`, then the `Rc<T>` can be cloned before moving ownership to the thread. Unfortunately if you try this the compiler will inform you that your reference counter cannot be sent between threads safely because the trait bound `Send` is not satisfied. `Send` is a trait to ensure that types used with threads are meant for concurrency. `Rc<T>` lacks this trait. The way it changes the reference count doesn't take into account getting interrupted by other threads, and this could lead to bugs that could result in memory leaks or an incorrect value assigned. Such an operation is actually handled with `Arc<T>`.

#### Atomic Reference Counting with `Arc<T>`

`Arc<T>` stands for *atomically reference counted*. Atomics are a kind of concurrency primitive that you can read up on in the standard library documentation. The thread safety that `Arc<T>` provides comes at a performance cost, so it should really only be used when you need that kind of concurrency in your program.

### Similarties Between `RefCell<T>` / `Rc<T>` and `Mutex<T>`/`Arc<T>`

`Mutex<T>` provides interior mutability, as does `RefCell<T>`. Just as `RefCell<T>` is used to mutate contents inside `Rc<T>`, `Mutex<T>` is used to mutate contents inside `Arc<T>`.

It is possible to create *deadlocks* with `Mutex<T>`, where an operation needs to lock two resouirces and two threads have acquired one of the locks, causing them to wait forever. Rust can't protect from that kind of logical error in programming.

## Extensible Concurrency with the `Sync` and `Send` Traits

Most concurrency features in Rust come from the standard library, not the language intself. The two concepts that are embedded in the language are the `std::marker` traits `Sync` and `Send`.

### Allowing Transference of Ownership Between Threads with `Send`

The `Send` marker trait indicates that ownership of the type with the `Send` implementation can be transferred between threads. Most Rust types have the `Send` trait, but there are some exceptions like `Rc<T>`. Raw pointers also lack the `Send` trait.

### Allowing Access from Multiple Threads with `Sync`

The `Sync` marker trait indicates that it is safe for the type with the `Sync` implementation to be referenced from multiple threads. Any type `T` is `Sync` if `&T` is `Send`.

For the same reason that it is not `Send`, `Rc<T>` also lacks the `Sync` trait.

### Implementing `Send` and `Sync` Manually is Unsafe

Since types that are made up of `Send` and `Sync` traits automatically also have the other trait, these traits do not need to be implemented manually. As marker traits they don't have any methods to implement, they're just useful for enforcing invariants related to concurrency. Manual implementation of these traits involves unsafe Rust code.
