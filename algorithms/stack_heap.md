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

Some heaps have a max-heap or min-heap property. For a max-heap a node must be greater than or equal to the child nodes. Heaps with a min-heap property work inversely.

Heaps are useful for building a priority queue. A priority queue features operations such as the insertion of nodes, returning a max or min value, extracting the max or min value from the heap and removing it, and increasing the value associated with a particular node. To maintain such a structure there also need to be operations that can build a max or min-heap structure out of an unsorted array, and an operation that can correct any instances where there is a single violation of the heap's max or min property from a subtree's root.
