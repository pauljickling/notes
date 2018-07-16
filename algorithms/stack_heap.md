## Stacks and Heaps

### Stacks

A stack is a simple data structure that contains multiple items. New items can be pushed to the stack (added to the end), and items at the end of the stack can be removed.

#### Queues

Queues are like stacks. They only have enqueue(push) and dequeue(pop) methods available to them. However their ordering is different. With a stack the order is Last In, First Out, whereas queues follow a First In, First Out order.

Common metaphors to help understand the difference between stacks and queues is to think of stacks like a stack of dishes that need to be cleaned, and to think of queues like ticket orders at a restaurant. With the stack of dishes they are placed one on top of the other, and in order to remove them you clean the dish that is on the top of the stack (i.e. the most recent plate). With a ticket order system, by contrast, it is the earliest ticket is the one that gets resolved first.

#### The Call Stack

The call stack is a stack data structure that stores information about active subroutines of a computer program. It is sometimes called "the stack" for short.

The call stack allows for functions within functions to operate. When a function is run every piece of data that is handled by that data is stored in the call stack. As other functions are run, the operations of the original function are paused, and additional data from the new function is added to the call stack. Once that is complete the original function resumes.

### Heaps

Heaps are also simple data structures that contain multiple items. Heaps are used for data where the size is unknown (for example, arrays that do not have fixed-lengths). The operating system returns a pointer with the address for the heap. The pointer, being a fixed size, is located on the stack.
