## Linked Lists and Arrays

### Linked Lists

A linked list is one where each item in the list stores the address of the next item. This means that the items can be stored anywhere in memory. However it also means that to find item n in a list, you have to first locate item n - 1.

### Arrays

The idea of arrays come from mathematics where an array is a set of numbers or objects placed along rows or columns. Implementation details of arrays differ considerably among programming languages. Does the array have a defined length? Do the items in the array consist of the same type or are they heterogeneous elements? A consistent feature of arrays across programming languages is that they are all indexed lists of items.

### Comparisons

The items in an array exist "adjacently" in memory, whereas linked list nodes do not have to be stored in adjacent positions. Instead the node simply points to where the next item lives.

Because of these differences in memory usage, it is more efficient to read arrays rather than linked lists, but it is more efficient to insert new items (or delete items) into linked lists than arrays. In Big O notation reading an array is O(1), and linked list is O(n), and an insertion/deletions for arrays is O(n), and O(1) for a linked list.
