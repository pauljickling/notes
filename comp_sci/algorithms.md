# 1. Algorithmic Thinking, Peak Finding

## Overview

The goal here is efficient procedures for solving problems on large inputs. Large inputs could consist of the U.S. highway system, the human genome, the internet, etc.

This is why scalability is important. It is critical to track how an algorithm grows in size.

Classic data structures will be covered, as well as classical algorithms like sorting and matching. This will involve real implementations of these data structures in Python.

## Peak Finding

One dimensional version is an array of numeric variables.

`[a, b, c, d, e, f, g, h, i]`

We want to find a peak. What is a peak?

Position 2 is a peak is a peak if and only if:

`b >= a and b >= c`

The start and end only have to look at one side of course.

### Statement:

`Find a peak if it exists.`

### First pass:

Start from the beginning of the index, and examine each element.

The worst case complexity is O(n).

### Next pass:

Use a divide and conquer strategy. Have a recursive algorithm examine the middle element of the array (n/2), and examine the adjacent elements.

```
x = n / 2
if x < x - 1:
    # only examine elements to the left
elif x < x + 1:
    # only examine elements to the right
else:
    # x is a peak
```

This reduces the instruction size in half. Complexity O(log<sub>2</sub>n).

**Important note:** this algorithm only finds a peak. It does not guarantee finding the maximum peak in an array. That task would require O(n) complexity. This demonstrates the importance of making sure you are solving the actual problem at hand.

### Dimensional Complexity

Now imagine a 2D matrix for this problem where you are solving for the global maximum. It has n rows and m columns.

This is solved with the *greedy ascent algorithm*. Start at an arbitrary midpoint. Then make decisions about the default traversal choices. Otherwise this problem would have O(nm) complexity, and if m = n you would have O(n<sup>2</sup>).

So we would select a middle column j = m / 2. Then find a one dimensional peak at (i, j).

Unfortunately this doesn't necessarily result in the correct solution. The problem is that a 2D peak might not exist on the given row.

#### Second Attempt

1. Select a middle column (j = m/2)
2. Find a global max on column j at (i, j)
3. Compare (i, j - 1), (i, j), (i, j + 1)
4. Select left columns if (i, j - 1) > (i, j) or right if (i, j) <= (i, j + 1)
5. Also if (i, j) >= (i, j - 1) || (i, j + 1) then (i, j) is the peak.
6. Then solve the new problem with half the number of columns.
7. Base case: when you have a single column, find the global maximum.

The complexity of this algorithm is T(n, m) = T(n, m/2) + O(n).
T(n, 1) = O(n)
T(n, m) = O(n) + O(n) = O(n log<sub>2</sub>m)

# 2. Models of Computation, Document Distance

What is an algorithm? What is an algorithm allowed to do? How do we measure running time?

The word algorithm comes from al-Khwārizmi, a.k.a. "the father of algebra". He wrote a book on how to write linear and quadratic equations. At a high level an algorithm is a computational procedure for solving a problem. So we think of algorithms as having some input, and it produces some output.

So an algorithm is a mathematical analog of a computer program, and a computer program is built on top of a programming language. The mathematical equivalent of a programming language is pseudocode a.k.a. structured natural language. The programming language runs on a computer, and the mathematical equivalent of the computer is the model of computation.

## Model of Computation

It sets the rules about what can be done in constant time. It specifies what operations you can perform in an algorithm, and what they cost (i.e. what is the time?)

### Two Models of Computation

1. Random Access Machine (RAM). This is modeled on the Random Access Memory. It's made up of random machine words (in w-bits) with indexes that specify the location of things. In constant time, it can load constant words, in constant time perform operations, and then store words in constant time. There is also a constant number of registers. This is how assembly programming works. Notwithstanding caching, this is how the actual computer operates.

2. Pointer Machine. This is a higher level abstraction that defines object-oriented programming. There are dynamically allocated objects, and an object has a constant number of fields. A field is either a word (i.e. integer), or a pointer. A pointer is something that points to another object or has a value of null. Sometimes pointers are called *references*.

Consider this doubly linked list with elements 5 and 1:

|Node1|Values|
|---|---|
|Data|5|
|Prev|1|
|Next|1|

|Node2|Values|
|---|---|
|Data|1|
|Prev|5|
|Next|5|

This could be a structure in the pointer machine. In Python this could be a named tuple, or just an object with three attributes, with one attribute that is a word, and two attributes that are pointers.

## A Reasonable Model in Python

These models of computation date back to the 70s and 80s. Python is a bit more contemporary. It offers both these perspectives. However it also has a lot of other operations like `.sort()`, `.append()`, etc. and each method has a cost associated with them.

1. In Python dynamically sized arrays are called confusingly called a list. It has no resemblance to a linked list, which is not part of the standard library of Python. They can be accessed in O(1) time.

2. There are objects with O(1) attributes. So `x = x.next` is constant time.

3. How much does `L.append(x)` cost? It takes an array and adds an element at the end of the list. The answer to this question depends on how the append method is implemented. If it were to copy a list, and generate a new list with element x at the end, that would be an O(n) operation. However that is not what Python does. It performs *table doubling*, which is performed in near constant time.

4. What about concatenating two lists (`new_list = list1 + list2`)? This is a non-destructive operation. So this would be O(n).

5. What is you check if element x is in a list (`x in L`)? It is O(n).

6. What about `len(L)`? This is actually constant time since Python will store the length of a list, and therefore it can be accessed in constant time, it doesn't need to check the length of the list, which would be O(n).

7. What about `L.sort()`? It uses a comparison sort which results in O(n log n).

8. What about dicts? It is a hash table, it results in constant time which is one of the most important data structures in computer science. Many of these methods are constant time unless it is a method that applies to a lot of dictionaries.

9. Long integers `long`, if they are to be added, is O(|x| + |y|).

10. In the standard library there is also the `heap`.

From these base costs we can better understand how to calculate the costs of algorithms written in Python.

## The Document Distance Problem

There are two documents (D1 and D2). Calculate the distance between them. The very first and obvious question to ask is, what does it mean to calculate the distance between two documents? Lets say you have two very similar documents, you might not want to catalog both these documents since they are so similar. So you need a way to calculate this difference.

We define a document as a sequence of words. A word is a string of alphanumeric characters. So a document is similar if they have a lot of shared words.

We can also think of a document as a vector. So now our formula is:

D[W] = number of occurrences of words in a document.

D1 = "the cat"
D2 = "the dog"

There are three words, so we can create three axes, one for each word. D1 has two words, so we plot that, and then we do the same for D2. Unfortunately this doesn't scale properly because imagine two 500 word documents that are identical, and two documents with a million words that only have 50% the same. The second set will have a higher score than the first. We could try dividing the answer by the number of words of a document to compensate for this. But what we really want is the arccos of D1 and D2. If it's 0 degrees, they're identical, and if it's 90 degrees, they are very different.

So now we have a way to proceed with the problem.

1. Decompose the document into words. We can do this in Python by running through the string, and every time you don't see a character that counts as a valid word, you splice the position, and create a new word.

```
re.findall(r'\w+', doc')
```

Be careful with `re` methods, that is, regular expressions because sometimes their runtime is bad.

2. Compute word frequencies. We iterate through the words, and put it in a dictionary. It takes O(n).

```
from collections import Counter
for word in doc:
    Counter([word])
```

3. Compute the dot product

```
for word, count1 in doc1:
    if word, count2 in doc2:
        total += count1 * count2

if words equal:
    total += count1 * count2
elif word1 <= word2:
    advance list1
else:
    advance list2
```

# 3. Insertion Sort, Merge Sort

Insertion sort is the simplest sort method, involves a few lines of code. Merge sorts is better and uses a divide & conquer strategy.

Sorting is important because it makes it easier to lookup information in a list. There are interesting problems that become easy once a list becomes sorted, like finding a median, which becomes n / 2. That means you can find it in constant time. It also makes a binary search possible, which is a very efficient search method.

There are also some non-obvious applications for sorting like data compression. Using sorting it makes it easy to find duplicate pieces of data that can be removed. So sorting becomes a sub-routine of data compression. Computer graphics also makes use of sorting because the computer doesn't want to render items that are occluded by some object, but you need to track it if it has transparent values.

## Insertion Sort

In pseudocode:

```
for i = 1, 2, ... n
    insert A[i] into sorted array A[0: i-1]
    by pairwise swaps to the correct position
```

In Python:

```
def insert_sort(arr):
    for i in range(1, len(arr)):
        j = i
        while j > 0 and arr[j] < arr[j - 1]:
            arr[j], arr[j - 1] = arr[j - 1], arr[j]
            j = j - 1
    return arr  
```

Insertion sort is extremely inefficient. It has a runtime of O(n<sup>2</sup>) because it has to perform n comparisons, and perform n swaps. If compares are more expensive than swaps, what should you do to improve the algorithm? Replace the swaps with binary search. This is called a binary insertion sort, and it allows compares to be O(n log n), but swaps are still O(n).

## Merge Sort

Merge sort uses a standard recursion algorithm. It takes array(A), and splits it into two sub-arrays: left(L) and right(R). It then creates additional sub-arrays, until you have n sub-arrays. It then merges these sub-arrays into a sorted list. It has O(n log n) complexity, so this is significantly better than insertion sort.

## Analysis

What is an advantage of insertion sort over merge sort? Merge sort requires a lot of auxiliary space. An in place sort has O(1) auxiliary space. If you're sorting a list with a billion elements, this could be a consideration. There is an in-place merge sort, but the running time is much worse than a standard merge sort. It is beyond the scope of this class though.

### Run Times in Practice

Merge sort in Python is 2.2 n log n microseconds (when tested back in 2011 or whatever).

Insertion sort in Python is 0.2n<sup>2</sup> microseconds.

In C insertion seconds is about 20 times faster compared to Python.

# 4. Heaps and Heap Sort

The heap can be used for a priority queue, and also used to create a heap sort that is different from other sorts.

A *priority queue* implements a set (S) of elements, and each element is associated with a key. These will have various methods like adding and removing elements from the queue, changing the priority order, etc.

The set of operations we'd like for a priority queue:

**insert(S, x):** inserts element x into set S

**max(S):** returns element of S with the largest key

**extract_max(S):** returns element of S with the largest key, and removes it from S

**increase_key(S, x, k):** increases the value of x's key to the new value k

The heap is what we need to perform these operations in an official way.

A heap is an implementation of a priority queue. It is an array structure visualized as a nearly complete binary tree. Imagine an array with 10 elements, with values of random numbers. It's not a complete binary tree, which would need 15 elements. Index 1 will be the root of the tree. Index 2 and 3 are the children. Index 4, 5, 6, 7 are the children of 2 and 3. Index 8, 9, and 10 are the children of 4 and 5.

The value of this structure is it has a root, which is the first element which is i= 1. The parent is i /2. The left element is 2i, and the right element is 2i+1.

The max-heap property is the key of a node is greater than or equal to the key of its children. This means that the root tree of a heap with the max-heap property will have a greater value than its children. The min-heap property is the inverse of it.

The max-heap property means the easiest operation to perform is max(S). However extract_max() is more complicated since this impacts the structure of the tree. So the question is how do we maintain the heap structure while performing an operation like extract_max()?

It is also possible to have heaps that have neither the max-heap property, nor the min-heap property. For example, the array [2, 4, 1]. The root is 2, which is less than 4. But 1 is less than 2. So how we get to a sorted heap is an interesting question.

So we have some heap operations we need to establish to have the operations for our priority queue:

**build_max_heap(arr):** produces a max heap out of an unsorted array

**max_heapify(arr, i):** corrects a *single* violation of the heap property in a subtree's root

The precondition for max_heapify() is that the trees rooted at left(i) and right(i) are max-heaps. Because it is performing individual swaps between a parent and child node, what is the runtime complexity? It is O(log n) because of the near complete binary tree structure, and the assumption that there is only a single violation.

So now we want to take max_heapify(), and use it to build build_max_heap(), which will then structure the heap such that it can be used as a priority queue.

So we need to convert array [1 .. n] into a max-heap.

In pseudo-code:

```
build_max_heap(arr):
    for i = n/2 down to 1.
        do max_heapify(arr, i)
```

This works because elements arr[n/2+1 .. n] are all leaves, and these are good to work with because by definition they will work with the max_heapify() necessary condition of only having a single violation. The complexity of build_max_heap() is O(n). For each leaf you are performing one operation. When you move up to the parent node, you are performing 2 operations per node. The next level up you perform 4 operations per node. Etc. So even though you have reduced the list in half, max_heapify takes O(1) for nodes that are one level above the leaves, and in general O(len) time for nodes that are 1 level above the leaves.

So going back to building a priority queue. We now have a clear path forward.

1. Run build_max_heap() from an unordered array.
2. Find the max element (i.e. arr[0])
3. Swap elements arr[n] with arr[0]. Now the max_element is at the end of the array.
4. Discard arr[n] from the heap by decrementing the heap-size.
5. The new root may violate may violate the max-heap-property, but the children fit the structure, therefore we can perform max_heapify() to fix the heap.

These steps allow us to perform the extract_max() operation we described earlier.

# 5. Binary Search Trees, Binary Search Tree Sort

Binary search trees are a useful data structure for divide & conquer solutions.

Imagine a scheduling problem with a runway reservation system. There is an airport with a single runway. There are various safety regulations that this reservation system has to account for. Reserve requests specify a landing time of t. t is added to the set R if no other landings are scheduled within k minutes. Lets assume k = 3 min.

Under these conditions we have a constraint that needs to be checked before an insertion into the set takes place. There also needs to be a remove() method from set R after a plane lands. How do we perform these operations in O(log n) time?

For example, assume we are at time unit 37, and there are scheduled landings at 41.2, 49, and 56.3. A request is received at 53. If k=3 this is ok, but 44 would not be allowed because it is too close to 41.2, and 20 would also not be allowed since that is in the past.

Here are possible (flawed) data structure implementations:

- An unsorted array. Insertions will be O(1), but it results in O(n) lookups, as well as nearly any other operation.

- A sorted array. Lookups can use binary search so that will be O(log n). So that's good. However an insertion operation means every element after the inserted element has to shift to the right. Worst case that is O(n).

-A sorted linked list with pointers fixes the insertion issue facing the sorted array, but it comes at the cost of traversing the list is O(n). In other words, as is always the case with linked lists and arrays, they suffer from the inverse problem.

- A min or max-heap structure. The problem with these structures is they have a weak invariant. If you're looking for a k-element check there is no easy method for traversal, and they will take O(n) elements.

-Dicts/hash tables suffer from the same problem as the heap.

## Binary Search Tree to the Rescue

Going over these issues we now have a clearer idea of the problem. We want some sort of sorted structure that allows for more efficient insertion operations.

A binary search tree(BST) has a node x: with a key(x).

Unlike a heap, a BST also has pointers for the parent, the left child, and the right child.

A BST has an ordering of the key values that satisfies the invariant that for all nodes x, if y is in the left subtree of x then key (y) <= key(x), and if y is in the right sub-tree of x then key(y) >= key(x).

With that in mind, why are BST's a good data structure for this runway problem?

Imagine an empty BST. You insert 49 and it becomes the root. Then you insert 79, check it, and it becomes the right child. Then you insert 46, and it becomes the root's left child. You insert 41 and it becomes the left child of 46.

Now imagine an insertion that is a violation of the k=3 rule. If you try inserting 42, it will check the root first (49) and it is okay, then you check 46, and that is fine too, but then you check 41 and it fails. So if h is the height of the tree, that means that insertion operations perform in O(h) time. So the advantage of the BST is that you can do insertion operations efficiently, and furthermore you can augment the insertion operation without impacting its efficiency.

There are other operations that can be performed in O(h) or O(1) time.

find_min() and find_max() will be O(h)

next_larger(x) just checks the right element. O(h).

An *augmented BST* has more data in them beyond just pointers.

Imagine a new requirement called Rank(t) that specifies how many planes are scheduled to land at times <= t? How do we satisfy this without increasing the runtime complexity?

To augment the BST structure we're going to add to the node structure a data attribute that looks at all the nodes below it (plus the current node itself).

To implement this attribute we will need to perform insert, delete, and modify size numbers.

So how do you compute ranked(t) from the subtree?

1. Walk down tree to find the desired time. This is just a lookup.
2. Add in the nodes that are smaller.
3. Add in the subtrees size to the left.

There is a problem with the BST structure that depends on invariant used. If it is too low a number such that each additional insertion to the tree is always inserted to the right, then you have a list, not a tree structure. At that point the efficiency of this unbalanced BST is O(n), not O(h).

# 6. AVL Trees, AVL Sort

BSTs support the following operations: insert(), delete(), min(), max(), next_larger(), next_smaller(). It performs all these operations in O(h) time. When evaluating O(h) it is important to consider to how balanced the tree is. h is the length of the longest path from the root to the leaf. Unbalanced amounts to O(n). A balanced tree is O(log n). In fact that is how we can define a balanced tree, namely, if it is O(log n).

The height of a node in a tree is another property to consider. When discussing node height we are talking about the level of the node i.e. the length of the longest path from the node to the leaf. Thus a leaf's height is 0, and it's parent leaf is height 1. In instances where a node has one child that is a leaf, and another child node that also has a child node that is a leaf, you take the maximum path, thus it's height would be 2.

*Data structure augmentation* is an important piece of methodology for maintaining data structure integrity. Insertion operations for instance need to be able to reorganize the elements of the data structure. In the case of an array, when you insert an element, all the other elements to its right need to be reindexed. Similarly binary trees also have to be able to adjust the calculation of its height, for instance.

An *AVL tree* requires heights of the left and right children of every node to differ by at most += 1. So the height of a left-subtree and right subtree should be h<sub>l</sub> - h<sub>r</sub> <= 1

An AVL tree is easy to maintain in O(log n) time. This means AVL trees are balanced. The worst case is when one sub-tree has a height of 1 more than its counterpart sub-tree for every node. N<sub>h</sub> is the minimum number of nodes in an AVL tree of height h. It's efficiency can be demonstrated recursively. N<sub>h</sub> = 1 + N<sub>h - 1</sub> + N<sub>h - 2</sub>. With this formula 1 is the root node, and the subtrees are the other Ns. This is similar to the Fibonacci formula. So N<sub>h</sub> > F<sub>h</sub>.

So AVL trees are efficient data structures. The question then is how do you maintain the properties of an AVL tree? You rely on the insert() operation. An AVL insert has the following steps:

1. Perform the typical BST insert operation

2. Fix the AVL property from the changed node up. How is this fixed? By performing a rotation. It switches the position of node x with child y. It can be a left-rotate where the node rotates to the left, or a right-rotate where the node rotates to the right. There are also some instances where you need to perform a double rotation to get a tree balanced to fit the AVL property.

Suppose x is the lowest node violating the AVC property. After its rotation, you need to walk up the tree to the root to check for additional violations.

You can use this to sort. So you insert n items, and this performs an in-order traversal that takes O(n) time, and the insertion takes O(n log n).

An *Abstract Data Type* is a specification of what your data structure is supposed to do. In Java this is called an interface, but essentially it is the operations that support the data type: insert(), delete(), min(), sucessor(), predecessor(), etc. Some data structures support some of these operations like insert() and delete(), however the nice thing about AVL trees is they support all these operations.

# 7. Counting Sort, Radix Sort, Lower Bounds for Sorting

Topics:

- Linear-time sorting: when is it possible, and when is it not possible?

- Sorting usually requires O(n log n) time, but there are instances when it takes O(n) time depending on your computational model.

## Comparison Model

This model has all input items as black boxes, a.k.a. abstract data types (ADTs).  The only operations allowed are comparisons (i.e. <, <=, >, >=, ==). The time cost is just going to be the number of comparisons.

Knowing that, any comparison algorithm can be viewed as a tree of all possible comparisons, their outcomes, and the resulting answers for any particular value of n (i.e. the size of your problem)

So if we have a binary search where n=3 we always know that it will check if A[1] < x, and it produces two possible outcomes. It will then either ask if A[0] < x or if A[2] < x. Then it will produce some sort of answer depending on that.

This means that every internal node in the decision tree corresponds to a binary decision in the algorithm (i.e. the comparison operation). It means a leaf in this tree represents a determined answer. A root-to-leaf path represents an execution of the algorithm. The length of the path is the cost of the algorithm's running time, that is, the height of the decision tree. So it makes it easy to understand what the worst-case running time is.

## Searching the Lower Bound

For searching given n preprocessed items, then finding a given item among them in comparison operations requires O(log n) in the worst case. The proof for this proposition is that a decision tree contains binary responses to any operation, and it has to contain all possible answers, then the height will have to be at least log n, and the height is the worst case running time.

## Sorting Lower Bound

Sorting has a lower bound of O(n log n). Just like searching, it will involve a binary decision tree. So it will ask if A[i] < A[j]. You will then end up with leaves that look like A[5] <= A[7] <= A[1] <=A[0]... etc.

The proof for this is that the decision tree is binary, and the number of leaves has to be at least the number of possible answers, and that number is n factorial (n!). This means that the height >= log(n!).

## Linear-Time Sorting (Integer Sorting)

Assume n keys sorting are integers and each fits in a word. This means you can do more than comparisons, and this helps us with the efficiently performing operations on data structures. For k you can sort in O(n) time.

### Counting Sort

Counting sort performs no comparison operations. It counts all the items. So imagine this list = [3, 4, 5, 7, 5, 5, 3, 6]. It counts each k, and increments the counter for that item. So then how does this algorithm output items? One way might be to create a 2D array where the original value becomes an array within the array of n length, that increases with each increment.

Psuedocode:

```
L = array of k empty lists
for j in range(n):
     L[key(A[j])].append(A[j])
output = []
for i in range(k):
    output.extend(L[i])
```

The first step takes k time. The append in initial for loop is O(1), totaling to O(n). The final for loop is O(n + k).

### Radix Sort

Radix sort uses counting sort as a subroutine. Imagine each integer as a base of columns (b). If you know the maximum value is k then you know the number of digits (d) = log<sub>b</sub>k + 1

The algorithm then sorts by the least significant digit, sort by the next least significant digit... sort by the most significant digit. Each of these sorts happens by using counting sort. Normally this takes O(n + k) time, but this time it takes O(n + b). So the total time is O((n + b) * d) which is equal to O((n + b) log<sub>b</sub>k). This will be minimized when b = O(n). So this comes out to O(n log<sub>n</sub>k). That becomes linear. So if k <= n<sup>c</sup> then O(nc).

# 8. Hashing with Chaining

Hashing is the most important data structure in computer science. In Python it is a *dictionary*, i.e. an abstract data type (ADT). It stores items, inserts items, looks items up, and deletes items. Each item has a key, and that key must be unique. Python maintains this by allowing insert operations to overwrite existing keys.

## Python Notation

Search: `D[key]`
Insert: `D[key] = value`
Delete: `del D[key]`

Item = (key, value)

## Motivation for Using Hash Tables

Document distance uses dictionaries to count items. They are the fastest and easiest way to implement many different operations. They are used in many databases. Almost all databases use either hashing or search trees. Network routers use hash tables. There are also some subtler uses, like substring searches, grep, string commonalities, and file and directory synchronization as is used in cloud storage tools like Dropbox. Finally, cryptography also makes use of hash functions to securely move information around the internet.

## Simple Approach to Implementation

The simplest implementation is a direct access table. Picture an array with the numeric index as the key. The problems with this implementation are:

1. Keys must be non-negative integers.

2. It consumes an enormous amount of memory if the size of the table is very large.

### Solution to Direct Access Table Implementation

1. What if keys aren't integers? This is sometimes called *prehashing*. A prehash function maps whatever keys you have to non-negative integers.

`hash key -> nonnegative integer -> value`

In theory, keys are finite and discrete. Anything on the computer can be written as a string of bits, so it is easy to perform this hash conversion.

However in actual practice, like in Python for example, there is a function called hash(x) (should be called prehash) that performs a mapping of x to integer.

For example:

`hash('\0B') = hash('1010C') = 64`

Ideally `hash(x) = hash(y)` and thus `x = y`. Again, not quite what happens, but we have a close theoretical model for understanding the implementation.

2. This leads to the question of how do we reduce space(memory)? This is *hashing*. This involves taking all possible keys and reducing them down to the smallest amount of possible integers.

We can call all possible keys "keyspace".  Then we have a hash function that converts it to an array, which we can call hash table(T). So:

`keyspace -> h() -> T`

Within keyspace there is a subset of keys that we actually want to map to our hash table(T). The hash function will then map them to the hash table, and we want the number of keys in the dictionary to be around O(n). How do we define this function h()? Furthermore, because our hash table has reduced space, there are going to be multiple keys that exist in the same position in the table. This is referred to as a *collision*. Collision is solved with chaining.

Chaining involves creating a linked list. Thus each key is a node in the linked list, and the linked list is the element of the hash table.

So why does this work? That is, why would you expect this to be efficient? The worst case scenario is that your hash function maps every key to the same element of your hash table, and so in practice all you end up with is a linked list.

In a typical implementation a hash function randomly selects the insertion point of your key into the hash table. It turns out this works really well.

*Simple uniform hashing* is the idea that each key is equally likely to be hashed to any slot of the table, independent of where other keys are hashed. In reality, this proposition doesn't really hold up, but it isn't too far off from what actually happens.

To analyze this proposition we want to know the expected length of the chain. For n keys, of m slots the probability is n/<sub>m</sub>. This is sometimes referred to as alpha, or the load factor of a table. This results in O(1) if m=O(n). The worst case scenario is O(1 + length of your linked list chain).

So this leads back to the question of how the hash function is implemented. Consider these three examples, two actually implemented, and one theoretical:

1. The division method. `h(k) = k % m`. This is considered a problematic implementation, but easy to understand.

2. The multiplication method. `h(k) = [(a*k) % 2<sup>w</sup>] >> (w - r)` where w refers to a w bit machine. `a` is a random integer within `w integers` where `r` is a subset of the leftmost bits of the rightmost bits of `w`. This is difficult to follow and understand, but works well in practice. In general `a` should be an odd number, and not close to the power of 2.

3. The universal hashing method is theoretical. `h(k) = [ak+b % p] % m` where `a` and `b` are random numbers between 0 and p - 1 where p is a prime number the size of the universe (!!!). In this implementation for worst case keys where k<sub>1</sub> and k<sub>2</sub> the probability is h(k<sub>1</sub>) = h(k<sub>2</sub>) is 1/<sub>m</sub>.

# 9. Table Doubling, Karp-Rabin

## Table Doubling

We have now covered nearly all the necessary aspects for implementing a hash table. The only missing piece of the puzzle is how should we grow the table that the hash keys reside inside? In other words, what should the length of the table (i.e. `m`) be? We'd like m to be about the length of the linked lists (i.e. `n`), but we don't know the size of `n`.

The idea to solve this is that we will start small, say `m = 8`, and then grow and shrink as necessary. We can have a rule for our insertion algorithm and say `if n > m: then grow_table()`. Our `grow_table()` function can be something like `m -> m'` (`m'` = m prime). In other words it will have the following steps:

1. Make table size `m'`

2. Build new hash function

3. Implement rehash:

```
rehash:
    for item in T:
        T'.insert(item)
```

The question now is how much time does our `grow_table()` function take? Generally speaking, it will be O(n + m + m').

Now the question is how much bigger should `m'` be? To correctly answer this we have to consider the cost of n inserts. So if `m' = m + 1` that would result in O(n<sup>2</sup>). On the other hand, if `m' = 2m` then the cost of n inserts is O(log n). Setting `m' = 2m` is called *table doubling*.

The cost of the regrowth function is important because it is a relatively expensive operation. Whereas most hash table operations only take O(1) time, when we regrow the table, we have to reimplement our hash function, and reinsert every item in our hash table, so at a minimum that will necessarily take O(n) time. However if the table only has to grow infrequently then most of the time we are working with the hash table we are dealing with fast operations, and we have minimized expensive operations.

This leads to the concept of *amortization*. Imagine an operation takes T(n) amortized time.

```
if k operation:
    then total O(<= kT(n)) time
```

In other words, we consider the cost of algorithms not as a one time transaction, but over a length of time. You can think of it as meaning a running time take "T(n) on average" where the average over all operations. This concept only makes sense for data structures. So for table doubling it has k inserts that take O(k) time along with O(1) amortized inserts and deletions.

Because our hash table has also has delete operations, we need to consider instances when we need to shrink the size of our table. There is a subtle difference from how the `grow_table()` function is implemented. Here we say `if m = n/4 then shrink to m/2`, and this takes O(1) time. Note that it could have been `n/3`, then only rule is it needs to be a constant greater than the number used to increase the size of the table.

## Karp-Rabin

Consider a string-matching problem. You are searching for a search pattern (your substring) inside of your document (the string). Does this substring occur in your string?

The most obvious approach to this problem is to define the length of your substring, and check each position of your string for that length and the characters in it.

```python
any(s==t[i:i + len(s)]
    for i in range(len(t) - len(s)))
```

Quick Python notes: `any` is [a built-in function](https://docs.python.org/3.5/library/functions.html#any). The `:` is used for string splicing.

`s` is our substring, and `t` is our document. It returns true or false depending on if s is equal to our substring of t and our for loop runs through the entire document. The running time is O(|s| * |t|). With hashing however we can reduce this problem down to O(|t|).

*Rolling hashes* is an abstract data type (ADT) where given a a rolling hash value `r.append(c)` where `r` maintains a string, and `c` is a character which causes r to append c while deleting the first character. This provides the hash value of the current string x = h(x). This will essentially allow us to check the document in constant time. This is called the *Karp-Rabin algorithm*.

```
for c in s:
    rs.append(c)
for c in t[:len(s)]:
    rt.append(c)
if rs() == rt():
    pass
for i in range(len(s). len(t)):
    rt.skip(t[i - len(s)])
    rt.append(t[i])
    if rs() == rt():
        check if s == t[i - len(s)  1:i + 1]
        if equal:
            found match
        else:
            happens with probability <= 1/|s|
```

If this is true then we have O(1) expected time. Note that in the instance of rolling hashes, you can actually use the division method for your hash function, and it will work fine. As a reminder, the method is `h(k) = k % m` so this is nice and simple to implement.

# 10. Open Addressing, Cryptographic Hashing

## Open Addressing

Open addressing is the notion of using a simple array to create a hash table. However there are some careful considerations that need to happen.

We can define open addressing as having the following features:

1. No chaining. So it is simply an array structure with items. There is one item per slot such that m >= n

2. Probing. Tries to insert an item into the array, and if it is unable to do so it reimplements the hash function such that we have a slightly different array structure by specifying the order of slots to probe for a key for insert, search, and delete operations. So our hash function (h) takes the universe of keys (U) and the trial count.

`h: U * {0, 1, ... m - 1} -> {0, 1, ... m - 1}`

### Example

Suppose an empty array table. You start to insert items into it. You try to insert the int element 586. So you have `h(586, 1) -> 1`. Then you might insert `h(133, 1) -> 2`, `h(204, 1) -> 4`, etc. (more on the 2nd parameter in a moment). And because there are empty slots available, everything is successful. Then you try to insert `h(496, 1) -> 4` and it fails. So now it tries `h(496, 2) -> 4`, and it will keep trying this incrementing the trial count until it works.

Searching will work more or less the same way as the insert operation. As long as the slots encountered are occupied by keys. Keep probing until k is encountered, or an empty slot is found. If there is an empty slot that means that the key was not encountered, and therefore the item is unavailable.

How about the delete operation? If you remove an item from your array doesn't that break your search operation? It will fail incorrectly by finding a None or null value before the location of the item being searched. Therefore the deleted item needs to be replaced with a different flag than None. In other words now you have some unique key that indicates a deleted item. The insert and search operations will treat this key differently. The insert operation will treat it as equivalent to None, null, etc. but the search will not. Note that deleting every item in your table without rehashing would be bad since your search operations would now be slowly checking a bunch of empty tables. This is why rehashing your tables are important.

### Probing Strategies

*Linear probing* is `h(k, i) = (h'(k) + i) % m` where `h'` is some ordinary hash function. The problem with linear probing is it results in clustering, i.e. the opposite of load balancing. You're likely to end up with lots of empty arrays, which is inefficient for your operations as well as memory.

*Double hashing* is where you have `h(k, i) = (h<sub>1</sub>(k) + i * h<sub>2</sub>(k)) % m)` where `h<sub>1</sub>` and `h<sub>2</sub>` are ordinary hash functions. This works pretty well.

## Uniform Hashing Assumption

This is not the same as simple uniform hashing which refers to the mapping of keys to slots.

The uniform hashing assumption says that each key is equally likely to have any one of the m factorial permutations as its probe sequence. This is very hard to implement in practice, double hashing comes the closest.

Open addressing requires less memory than chaining functions because they don't use pointers, however this comes at the cost of having to perform table resizes more often than you would see in chaining functions. Open addressing tables rely on a probabilistic assumption of when to resize where it evaluates the cost of an insert operation as <= 1/<sub>1 - alpha</sub> where alpha is equal to n/<sub>m</sub>, and n effectively being the total number of trial counts. Therefore you can think of alpha as the expected number of probes. This number for this insertion cost operation is always going to be somewhere between 0 and 1, and as the number approaches around .5 or .6 it starts to become very slow and inefficient.

## Cryptographic Hashes

Cryptographic hashes are used for, among other things, password storage. In any Unix system you type in a password, and you have to check that string. The dumb way to do that is store these password values in a text file somewhere and look it up.

A better method would be to use a hash method that has a one-way property. This means that given h(x) it is very hard to find x. So if h(x) = Q then in your database you might have something like `username: x12467` where the value is x'. So it then takes h(x') = h(x) = Q.

# 11. Integer Arithmetic, Karatsuba Multiplication

What happens when you want to compute numbers that exceed 64-bits? Performing a precision calculation of the weight of a neutrino for example, involves at least one hundred decimal places. In other words, how do we deal with irrational numbers?

The first irrational number discovered was the square root of 2. Pythagoras described this type of number as "speechless". The problem was it isn't clear how to compute these numbers since they are infinitely non-repeating.

## Catalan Numbers

Catalan numbers have a recursive definition. They represent the cardinality of Set P of balanced parentheses strings where strings are recursively defined as:

1. Lambda belonging to Set P where lambda is the empty string.

2. If alpha, and beta belong to Set P then (alpha) beta belongs to set P.

Every nonempty balanced parentheses string via rule 2 from a unique alpha beta pair. For example if you have a string like: '(())()()' then you obtain alpha equal to (()) and beta equal to ()().

*Enumeration:* C<sub>n</sub> is the number of balanced parentheses strings with exactly n pairs of parentheses.

C<sub>n + 1</sub> = ?

C<sub>0</sub> = 1 empty string

C<sub>1</sub> = 1 ()

C<sub>2</sub> = C<sub>0</sub>C<sub>1</sub> + C<sub>1</sub>C<sub>0</sub> = 2

C<sub>n + 1</sub> = C<sub>k</sub>C<sub>n - k</sub> where k= 0 and n >= 0

Trivia: Note that 42 is a Catalan number.

## Newton's Method

Suppose you have y = f(x) for calculating a parabola. We can find the root of f(x) = 0 through successive approximation. So you can have f(x) = x<sup>2</sup> - a. To compute the tangents you need to solve you need the formula y = f(x<sub>i</sub>) + f'(x<sub>i</sub>)(x - x<sub>i</sub>)

The significance of this is you have quadratic convergence with the results of this formula. The number of digits of precision doubles with each iteration.

## High Precision Multiplication

High precision multiplication is an interesting problem. Multiplying a 50 digit number by a 100 digit number is difficult for a human to perform. Computers have no such limitations however. On the other hand, multiplying large numbers faces the same problems that any other algorithm faces: the rate of computational growth. Standard multiplication takes O(n<sup>2</sup>) time. In other words, it doesn't handle growth well. However alternative methods have been developed for performing high precision calculations with large numbers.

## Karatsuba Multiplication

z<sub>0</sub> = x<sub>0</sub> * y<sub>0</sub>
z<sub>2</sub> = x<sub>2</sub> * y<sub>2</sub>
z<sub>1</sub> = (x<sub>0</sub> + x<sub>1</sub>) * (y<sub>0</sub> + y<sub>1</sub>) - z<sub>0</sub> - z<sub>2</sub>

This gives you asymptotic complexity of T(n) = 3T(n/2) + O(n). Therefore you end up with something like O(n<sup>1.5848625...</sup>).

Now imagine a circle with a circumference of one trillion "units" with a center C and a radius A and radius B. So the radius is half a trillion. Now connect a point running from B to somewhere in between C and A where the end point is D. What is AD? AD = AC - CD. So it will be half a trillion minus the square root of half a trillion squared minus one. This will necessarily provide an irrational quantity. What does it look like? It ends up being strings of zeroes with Catalan numbers popping up now and again.

# 12. Square Roots, Newton's Method

`pass`

# 13. Breadth-First Search

Breadth-First Search can, in principle, be used to solve problems like the rubik's cube.

A *graph search* is all about exploring a graph. That sounds like a tautology so lets break it down. It can involve simply finding a path from one node to another, it can be exploring all nodes, it can be exploring all edges.

A graph can be stated as G=(V, E) where V is a set of Vertices, and E are Edges. The vertices are every node in the graph. Edges are either unordered or ordered pairs. An unordered pair means an undirected type, and ordered pair means a directed graph. So an unordered graph with nodes A, B, C, and D could be expressed this way:

V = {a, b, c, d}
E = {{a, b}, {b, c}, {c, d} ...}

How do you represent a graph like this for an algorithm? Using an array would be extremely inefficient because of how you lookup items in the array to figure out if a particular traversal is possible.

Graph searches have many applications:

- web crawling
- social networking
- network broadcast
- garbage collection
- model checking
- checking mathematical conjectures

## Hypothetical Problem

Imagine a rubik's cube that is 2x2x2.

The configuration graph will have a vertex representation for each possible state of the cube. The number of vertices is 8! * 3<sup>8</sup> (i.e. about 264 million).

It will be an undirected graph since any move can be undone which means you can move from a new state to the prior state.

There is a single state for the solved state. Then there are all the possible configurations that are adjacent to that state, and then the configs that are two edges away, etc. until you exhaust the graph. That is 11 moves. So in the worst case, an optimal algorithm can always solve this cube in 11 moves. This optimal algorithm is the *breadth-first search*.

2 x 2 x 2 = 11

3 x 3 x 3 = 20

n x n x n = O(n<sup>2</sup>/<sub>log n</sub>)

## Graph Representations

There is one standard representation called an adjacency list. You have an array `Adj` of |V|. Each element in the array is a pointer to a linked list. For each vertex of u belonging to set V you have Adj[u] as a hash table that stores the neighbors. So you might have something like:

Adj[b] = {a, c}
Adj[a] = {c}
Adj[c] = {b}

This requires O(V + E) time. This is fairly optimal, it is essentially O(n).

There are lots of ways to implement adjacency lists. An object-oriented fashion might be to have V.neighbors = Adj[v]. Object-oriented implementations are good when you are only dealing with a single graph. However if you have multiple related graphs where a particular vertice has various connections you are better off using the prior mentioned hash table implementation.

There is also the implicit representation where Adj[u] is a function, or v.neighbors() is a method of the vertex class. This uses less space, so it's useful. This is great when you are dealing with incomprehensibly large graphs.

## Breadth-First Search

A breadth-first search wants to explore as many nodes as possible. The goal is to visit all nodes reachable from given set of V. We want O(V=E) time, and we want to look at the nodes that are reachable in 0 moves {s}, 1 move Adj[s], 2 moves Adj[s'], etc. while avoiding cycles.

In Pythonish:
```
BFS(s.Adj):
    level = {s: 0}
    parent = {s: None}
    i = 1
    frontier = [s]
    while frontier:
        next = []
        for u in frontier:
            for v in Adj[u]:
                if v not in level:
                    level[v] = i
                    parent[v]
                    next.append(v)
        frontier = next
        i += 1

```

The function parameter is the starting node. Frontier is every node that can be reached in i - 1 moves, and next is what can be reached in i moves. The while loop ends once frontier becomes empty. The parent variable isn't strictly necessary, but it can helpful if for some reason your search needs to retreat from the current position. They can be useful for determining the shortest path from node x to node x'.

## Shortest Path

V <- parent[v]
 <- parent[parent[v]]
 <- etc.
S <-

Shortest in this context is the number of edges. For weighted graphs this involves a different calculation.

# 14. Depth-First Search, Topological Sort

A breadth-first search examines a graph layer by layer, which is nice for locating the shortest path. A Depth-First Search (DFS) is used for exploring the whole graph. A DFS uses recursion to explore a graph, backtracking as necessary.

To explore all nodes reachable from node s we write the following in Pythonish:

```
parent = {s: None}
DFS_Visit(V, Adj, s):
    for v in Adj[s]:
        if v not in parent:
            parent[v] = s
            DFS_Visit(V, Adj, v)
```

`s` in `parent[v] = s` stands for "seen node".

And if we want to explore the entire graph we create an additional function that includes our DFS_Visit function:

```
DFS(V, Adj):
    parent = {}
    for s in V:
        if s not in parent:
            parent[s] = None
            DFS_Visit(V, Adj, s)
```

The running time of this algorithm is linear time, O(V+E). You visit every vertex in the outer loop once. As for the recursion, it is only called whenever we know that the parent hasn't been set before. So it is also called once per vertex v.

So BFS and DFS have identical runtimes. So why do we care which one we use? It has more to do with the problem we are trying to solve. DFS won't find shortest paths for you, BFS does that. However DFS allows us to create edge classification.

## Edge Classification

- A tree edge is a parent pointer where we visit a new vertex via that edge.

- A forward edge is an edge that moves forward along the tree to a descendant.

- A backward edge is one that moves to ancestor nodes.

- A cross edge is one that moves to a sibling node or other non-ancestor related node such as moving from one tree in the graph to another.

How do you compute these structures? You need to start inserting statements in the DFS algorithm to mark which nodes and edges are already in the recursion stack.

Edge classification is useful for two problems: cycle detection, and topological sort.

## Cycle Detection

G has a cycle if and only if a DFS of G has a backward edge. Cycle detection is easy to implement in an undirected graph, but is more involved for directed graphs since you have to account for edge directions.

## Topological Sort

Topological sort can be thought in terms of *job scheduling*. That is we are given a directed acyclic graph, and we want to order vertices so that all edges point from lower order to higher order. In other words, there are some jobs that are prerequisites to other jobs, so they are considered lower order tasks that lead to higher order tasks. For example, I import a library that I use, before I call a function from within that library. This is called a topological sort because it is a sort by classifications rather than some other considerations. It involves running a DFS, and output reverse of finishing times of vertices. So every time a vertex is finished, it is added to a list. Then that order is reversed, and it is a topological order.

To summarize the topological sort algorithm:

1. Run DFS algorithm
2. Return list
3. Reverse list

# 15. Single-Source Shortest Paths Problem

Weighted graphs pose a much more challenging and interesting set of problems to solve.

Our weighted graph can be expressed as G(V, E, W).

There are two relevant algorithms for shortest-path problems:

1. Dijkstra's algorithm that assume non-negative weighted edges with a complexity O(V logV + E). Note that you can think of E = O(V<sup>2</sup>).

2. The Bellman-Ford algorithm is more complex, but is able to handle weighted edges that include negative values. It has a complexity of O(VE).

## Definitions

V = Vertices (for example, street intersections)

E = Edges (streets, roads, a directed edge is a one way street)

W(u, v) = Weight of edge from u to v

path p = < V<sub>0</sub>, V<sub>1</sub> ... V<sub>k</sub> >

(V<sub>i</sub>, V<sub>i + 1</sub>) is an element of E for O <= i < k

w(p) = the summation of k - 1 where i =0 is w(v<sub>1</sub>, v<sub>i + 1</sub>)

Find p with minimum weight.

A weighted graph can be expressed as:

V<sub>0</sub> -> V<sub>k</sub> where V<sub>0</sub> is a path from V</sub>0</sub> to V<sub>0</sub> of weight 0.

## Example

You have a starting node of S, and it connects to two sets of nodes:

1. Set A, C, and E

2. Set B, D, and F.

Where each node in the set connects to the subsequent node of its set.

Additionally, nodes A and B are connected. A also connects to E. D connects to C. Finally, E connects to D and F.

Edge list:

Path | Weight
--- | ---
S -> A | 1
S -> B | 2
A -> C | 5
A -> B | 3
A -> E | 2
C -> E | 3
E -> D | 1
E -> F | 4
B -> A | 1
B -> D | 1
D -> C | 2
D -> F | 1

## Analysis

We start with a weight of 0 from our source node since we have not travelled any distance yet. With each edge traversal we add to that weight. So travelling from S to A gives us a weight of 1, and S to B gives us a weight of 2. Moving from A to C then gives us a weight of 6, etc. You will have to consider every path to arrive at the least weight since some paths add more weight from one point to the other.

## Negative Weights

Why consider negative weights? Weights can be more than just physical distances. It could be the cost of a particular action. So if we're considering a cost analysis in a graph structure we can have edges that earn money and edges that cost money. So we can perform a cost/benefit analysis in this way.

One always has to be mindful of cycles when considering graph problems, but this is especially true for weighted graphs that contain negative values where a cycle can create non-terminating conditions for evaluating the weight of a path. It makes a shortest path length indeterminate for every node that is connected via a negative cycle. Therefore we need a termination condition that marks these nodes as indeterminate.

## General Structure of Shortest Path Problems

1. Initialize for all u belonging to V where d[v] is infinity and Pi[u] is nil.

2. Repeat select some edge(u, v)

3. Relax edge (u, v) until all edges have d[v] <= d[u] + w(u, v)

Reminder that weight is the value of node u to node v.

Relaxing refers to the process where you evaluate the weights of the various paths to a node, and select the optimal route. So lets say you have Nodes A, B, and C. Nodes A and B connect to Node C with a weight of 3 and 2 respectively. When you arrive at Node C you evaluate the weight from Node A and have a weight 3, and then when you evaluate the path from Node B to Node C you see that the weight of that edge is 2 so you set your weight from 3 to 2.

Note that we don't actually want to implement this algorithm (we will use Dijkstra and Bellman-Ford), this is just a general way to think about this problem. An actual implementation of this will have asymptotic exponential growth.

We have to consider optimum substructure. Subpaths of a shortest path are also shortest paths.

# 16. Dijkstra

Note: Pronounced "Dike-stra".

## DAG Solutions

Directed Acyclic Graphs (DAGs) are the easiest graphs to solve.

1. Topologically sort the DAG. Path from u to v implies that u is before v in the ordering.

2. One pass left to right over the vertices in topologically sorted order relax each edge that leaves each vertex. You will now have a shortest path available for every vertex.

## Dijkstra's Algorithm

Dijkstra's algorithm doesn't work for graphs with negative edges. However it can work for graphs with cycles. So a DAG with negative edges can't use Dijkstra's algorithms.

Dijkstra's algorithm is nice because it is a greedy algorithm. It finds the shortest path among its neighbors, and then the next, and the next, etc. until it has explored every vertex.

Pseudocode:

```
dijkstra(G, W, s):
    init(G, s):
        S <- 0
        Q <- V[G]
        S.add(Q)
        while Q != 0:
            extract u (min value) from Q, and delete u from Q
            S <- S U {u}
            for each vertex v belonging to Adj[u]:
                relax(u, v, w)
```

### Complexity Analysis

O(v) inserts into the priority queue
O(v) extract min value operations
O(E) decrease key operations

There are some choices for how you implement your priority queue, and so this has some implications for the complexity analysis. If you use an array you have O(v) for the extraction operation, and O(1) for the decrease key operation. So that means O(V.V + E.1) = O(V<sup>2</sup> + E) = O(V<sup>2</sup>). Alternatively you could use a binary min-heap structure for your priority queue. That has O(log V) time for the extract min operation, and for its decrease key operation. That total is O(V log V + E log V). There is also a Fibonacci heap, which has not been covered in this course. It has O(log V) for the extract min operation, and O(1) for the decrease key operation, with a total of O(V log V + E).

# 17. Bellman-Ford

We need to think about negative cycle detection. Our algorithm would consider previously visited nodes where the returning edge has a negative value.

Recall the generic shortest path problem. There is the initialization where you define your vectors (i.e. nodes) within a set. Then there is the main portion of the algorithm, and the relaxation technique.

There are two problems with the generic algorithm.

1. The complexity could be exponential, even for positive edge weights.

2. Graphs with negative cycles that are reachable from the source will create an infinite loop.

Dijkstra's algorithm solves the first problem with the generic algorithm, and Bellman-Ford solves the second problem.

## Bellman-Ford Algorithm Specifications

G = Graph
W = Weight
s = source
u = starting point
v = finishing point

```
def BellmanFord(G, W, s):
    init:
        #same as generic case
    for i=1 to |v| - 1: # number of passes you make
        for each edge (u, v):
            Relax(u, v, w)
            if d[v] > d[u] + w(u, v)
            d[v] = d[u] + w(u, v)
    for each edge (u, v) in the graph: # checks for negative cycles
        if d[v] > d[u] + w(u, v)
            then report negative cycle exists
```

Bellman-Ford is slower than Dijkstra which can have linear complexity, whereas Bellman-Ford can have a cubed complexity, but if you have negative cycles then you are stuck with it.

## Solving for the Shortest Simple Path

The shortest simple path with negative weight edges is considered a NP problem. Bellman-Ford can't solve it. It aborts calculations with negative weights, but that means you can't really process for the shortest path. It's equivalent to the longest path problem in that both are NP complexity.

# 18. Speeding Up Dijkstra

Sometimes you can't improve the asymptotic worst case scenario, however you can still optimize your algorithm by improving the average case scenario. It isn't easy, or in some instances possible, to write out a formal proof demonstrating this, but it is born out in practical, empirical results. For example, we can optimize Dijkstra's algorithm when you are looking for a single/specific target.

A bi-directional BST is a way to improve that sort of shortest path solution.

There is also the notion of all pairs shortest paths, not covered here.

## Dijkstra

Dijkstra pseudo code:
```
Initialize()
Q <- V[G]
while Q != 0:
    do u <- EXTRACT-MIN(Q) (stop if u = t!)
    for each vertex v belonging to Adj[u]:
        do RELAX(u, v, w)
```

In other words, by knowing our target we can create a terminating condition that improves the performance of our algorithm.

## Bi-Directional Search

In Bi-Directional Search you have Node S and Node T and some nodes in between them. So you have a search running from S, and a backwards search from T. You alternate between going forward from S and backwards from T until you arrive at a solution. However that stopping condition has some subtleties associated with it. What is the stopping condition exactly? If some vertex (W) has been processed in the frontiers of both searches. Once it has been deleted from the priority queues of both searches we can terminate.

After termination how do we find the shortest path? One possible claim for how to do this.

**Claim:** if Node W is extracted first from both queues, then find the shortest path from the forward search to go from Node S to Node W. Then we find the shortest path from Node T to Node W. Now we can combine the two paths.

As it turns out this claim is not quite correct. The shortest path may not be through the node that executed the terminating condition.

To resolve this we need to examine the results of our relaxation methods for our forward and backwards searches. When frontiers collide it will happen at the path that involves the fewest edges, rather than the path that will have the shortest weight path.

To resolve this issue our algorithm needs an additional check. We want to find an x that is possibly different from w that has a minimum value of forward_search(x) + backward_search(x). If x < w use that.

## Heuristics to Modify a Graph Optimally

A Goal Directed Search modifies the edge weights so that you move "downhill" towards the shortest path.

# 19. Dynamic Programming I: Fibonacci, Shortest Paths

Dynamic Programming (DP) is a general, powerful algorithm design technique. It is generally for optimization problems. You can think of it as an exhaustive search. You can think of it as "careful brute force", and you end up with polynomial time instead of exponential time. It doesn't always work (i.e. NP problems).

Dynamic programming is famously a nonsense phrase intended as a bit of misdirection from Richard Bellman (of the Bellman-Ford).

A basic idea of dynamic programming is to divide a problem into subproblems, solve them, and then reuse these subproblems to solve the larger problem.

## Using Dynamic Programming to Solve Fibonacci Numbers

Take Fibonacci numbers. The naive recursive algorithm is as follows:

```
def fib(n):
    if n <= 2:
        f = 1
    else:
        f = fib(n - 1) + fib(n - 2)
        return f
```

This is exponential time. We can do better by using *memoization*.

```

memo = {}
def fib(n):
    if n in memo:
        return memo[n]
    else:
        if n <= 2:
            f = 1
        else:
            f = fib(n - 1) + fib(n - 2)
            memo[n] = f
            return f
```

We create an empty dictionary call memo, and before we perform any calculations we check the memo to see if it contains the key. By performing this check we save an entire recursive iteration of calculations. `fib(k)` only recurses the first time it's called. The memoized calls cost O(1). The number of non-memoized calls is n. The nonrecursive work per call is O(1). So the time is O(n).

Side note: the very best fibonacci algorithm is O(log n), so it's possible to do even better than this.

In general in dynamic programming, you memoize data so you can reuse them. The big challenge of dynamic programming is to identify useful sub-problems. So they have to be smaller than the actual problem, and they should be able to help you solve your actual problem.

So in the instance of the fibonacci algorithm dynamic programming is simply memoization plus recursion.

The runtime for any dynamic programming will be equal to the number of sub-problems multiplied by the amount of time you spend in time per subproblem.

### A Bottom-Up Definition of DP Algorithms

```
fib = {}
for k in range(1, n + 1):
    if k <= 2:
        f = 1
    else:
        f = fib(k - 1) + fib(k - 2)
        fib[k] = f
    return fib[n]
```

In general, the bottom-up computation does the exactly same computation as the memoized version. Essentially it is a topological sort of the subproblem dependency DAG (directed acyclic graph). That is, you can think of n, n - 1, n - 2, as nodes in a graph.

This can often be used to save storage space.

## Shortest Path

What does the naive shortest path algorithm look like? We can use guessing as a useful tool. The algorithmic process is to try every guess. i.e. brute force it.

So now we can formulate DP as recursion, memoization, and guessing.

Subproblem dependencies need to be acyclic to avoid infinite loops.

That means if you have a cyclic graph you need to make it acyclic.

# 20. Dynamic Programming Part 2: Text Justification, Blackjack

Recap: Dynamic programming can be thought of as carefully applying brute force. The three main details are guessing, recursion, and memoization. Recursion is typically O(n<sup>2</sup>) time but memoization shaves off time to polynomial time. Another way to think about dynamic programming is that it is computing shortest paths in a directed acyclic graph. We can think of it that way since you can almost always think of problems using graph theory. The time of dynamic programming can be broken down as the number of subproblems being solved, multiplied by the time divided by the subproblem where we get to think of recursive calls O(1) thanks to memoization.

## Five general steps for dynamic programming

Note these steps are not necessarily in sequence.

1. Define subproblems

2. Guess (part of solution)

3. Relate subproblems solutions with recursion.

4. Use recursion and memoization, or else use a bottom up approach, i.e. build a table.

5. Solve the original problem

## Text Justification

The problem: Split text into "good lines".

The text is a list of words. We will define a quantity called "badness". Badness uses words[i:j] as a line. It can be formulated as (page width - total width)<sup>3</sup>. If it's close to zero that's good, and the higher it gets that's better.

A greedy algorithm packs as many words into a line as possible, but that potentially makes the next line worse. So dynamic programming is a great way to solve this.

How should we define this sub-problem? A brute force solution simply tries every single partition. Next we need to guess where the second line begins (we know where the first line begins). The number of choices for the guess is <= n - 1. Naturally we next want to know where the third line begins. And so on. The sub-problem can be thought of as suffix words (i.e. word[i:]). The number of sub-problems is n.

The recursive relation is def foo(i). We want a for loop that does: `for j in range(i + 1, n+1)`. We then want to find the min value.

## Blackjack

How to cheat in Blackjack. This is difficult to execute in a casino environment because it requires perfect information. In our programs however we can create any environment we like. Think of the deck as a sequence of cards. `deck = c0, c1, c2, ... cn - 1`. There is 1 player vs dealer. $1 bets per hand. No special rules like splitting, etc.

The guess is whether or not to hit or stand. We can actually quantify this question, that is, how many times should you hit?

The sub-problem is suffix c[i:]. The number of sub-problems is n.

The recurrence is:
```
 def foo(i):
     max(outcome belongs to a set of {+1, 0, -1} + def foo(i + number of cards used)
     for number of hits in 0, 1, ...:
         if valid play:
             add to set
         else:
             break)
 ```

 i is equal to the number of cards left in the deck. The max function is calculating how much money is made from the various plays made. The running time is cubic in the worst case since it also has to factor in the dealer's plays.

# 21. Dynamic Programming Part 3: Parenthesization, Edit Distance, Knapsack

## Subproblems for Strings

The hardest part of dynamic programming is identifying the subproblem. Some general tips for defining subproblems that involve strings or sequences:

- Suffixes x[i:] up to len(i) O(n)

- Prefixes x[:i] up to len(i) O(n)

- All substrings x[i:j] up through i <= j O(n<sup>2</sup>)

## Parenthesization

Given an associative expression you need to evaluate it in some order. Consider the problem of matrix multiplication. Suppose you have a column vector, a row vector, and another column vector. By adding parenthesis we can multiply these values in several different ways. Different configurations will have different runtime complexities. Dynamic programming can figure out which is a more efficient ordering.

What should we be guessing for an optimal solution? We want to figure out what is the final multiplication operation we want to perform.

The subproblem will be the optimal evaluation of A<sub>i</sub> ... A<sub>j - 1</sub>.

The recursion will be
```
def para(i, j) = min(
    for k in range(i + 1, j):
            para(i, k) + para(k, j) + cost of (A<sub>i:k</sub>) * (A<sub>k:j</sub>)
    )
```

The total time for this is going to be the number of subproblems O(n<sup>2</sup> * n) = O(n<sup>3</sup>) i.e. polynomial time.

## Edit Distance

Given two strings, x and y, what is the cheapest possible sequence of character edits to turn string x into string y? We're allowed to insert, delete, and replace a character anywhere in the string. The different string operations will have different "costs" associated with them.

This is useful for spell checks, checking DNA sequences, and many other types of problems.

There is also the longest common subsequence problem.

HIEROGLYPHOLOGY is an English word.

MICHAELANGELO is a valid name.

What is the longest common subsequence of these two strings? We can think of this as an edit distance problem where the cost of insert and delete operations is 1 and replace is 0 if c = c' or else infinite.

(The answer for this is "HELLO")

The dynamic programming approach is to take suffixes of x and y. In other words, the subproblem is to edit distance on x[i:] and y[j:]. The number of sub-problems is n<sup>2</sup>.

For guessing, we want to look at first characters, because we want to remove initial characters so we are left with subsequences. There are one of three possibilities, and they define all possibilities. We either replace x[i] with y[j], we can insert y[j], or we can delete x[i].

The recursion will simply be a max of these three options. So:

```
def edit(i, j) = min (
    (cost of replace x[i] -> y[j])
    edit(i + 1, j + 1),
    (cost of insert y[j])
    edit(i, j + 1),
    (cost of delete x[i])
    edit(i + 1, j)
  )
```

The topological order with suffixes moves from the end to the beginning. The runtime is O(|x| * |y|).

## The Knapsack Problem

You have a list of items, and each of them has a cost (size, weight, whatever) and an associated value (how much is it worth to you).

The knapsack has a size (S) for holding items. You want to maximize the sum of values for a subset of items whose size is <= S.

The subproblem will be the suffixes of items (i.e. item(i:) of items).

First we need to formulate a guess. The guess is item i included or not?

Recursion:

```
def knapsack(i, X): max(
    knapsack(i + 1, X),
    knapsack(i + 1, X - S<sub>i</sub>) + v<sub>i</sub>
    )
```

This is not polynomial time unfortunately. It is pseudo-polynomial time. It is the problem size and the numbers in the input.

# 22. Dynamic Programming Part 4: Guitar Fingering, Tetris, Super Mario Bros.

There are two types of guessing. One type of guessing technique is you don't know what direction to go, so you try both. For example, do you go left or right at a fork? But there are other types of guessing. For example, what type of subproblem to use to solve a bigger problem? When you define your subproblem, you can add more subproblems so you can "remember" more features of the solution.

## Piano and Guitar Fingering

Given a sequence of n notes that you want to play, find a fingering for each note. Fingers are a list from 1 to F where it will typically be 5 or 10 depending on whether you are using one hand or two. Then there is a difficulty measure (d) where the parameters are a note p on finger f where we want to transition to note q with finger g. As it turns out there is quite a bit of literature on this for piano. For example there is a weak finger rule where there are certain transitions that are more difficult for those fingers than others. There are many rules like that, and you can create a table that establishes these rules.

So what are the subproblems here? We have a sequence of notes. So we need to create "sub-sequences" to evaluate the easiest pattern. So we have to guess which finger to use for note i.

So now the question is how we can create a recursion. This is a trap though, there's actually no way to write a recursion for this problem. The reason is even if we know notes i and i+1 that is insufficient information because the next note could change our calculations. So we need additional subproblems to solve this problem.

So we want to know how to play notes[i:] when use f for notes[i]. In other words, we have a root finger we begin with, and this helps limit our possibility space. So what are we guessing now? Finger g for notes[i + 1].

So now we actually have a recursion:

*Where for note (i) and finger (f) we find the min value of (i+1, g) + (notes[i], f, notes[i+1], g) for g in 1 ...F.*

Creating a topological order is slightly tricky since there are two parameters. So we have something like:

```
for i in reversed(range(n)):
    for f in 1...F
```

The original problem is we to solve is the finger to use that provides the min for the initial note.

This algorithm has n * F subproblems. Each subproblem takes O(F) time. So it takes O(nF<sup>2</sup>). Since F is usually small (5 or 10), this is a pretty good algorithm. Of course it gets more complicated if you take chords into account where you are playing multiple notes with multiple fingers. Furthermore a guitar fretboard complicates things where you have multiple options to play the same note. So we have to generalize what counts as a "finger" is a finger plus a string. But this changes the time to O(nF<sup>2</sup>S<sup>2</sup>) when accounting for the multiple fingers and strings.

## Tetris

This is more like a Tetris puzzle rather than proper Tetris, because we are going to know the sequence of n pieces. It must drop from the top. Also full rows don't clear. So can you survive with n pieces? Also the width w is small. The board is level 1 (i.e. empty initially). Without these constraints Tetris is a NP complete problem.

The subproblem is the suffix pieces [i:]. Like our music problem, this subproblem is insufficient. Given our constraints, all we care about is how high the board is because we just want to survive as long as possible. Different columns will have different heights, so that is what we need to track. (h + 1)<sup>w</sup> will be the number of subproblems.

## Super Mario Bros

There are some nice features to SMB. As you move to the right, everything to the left is gone.

Given the entire level consisting of n bits of information, with a small screen w*h screen.

We can optimize for certain things: speedrunning, maximizing coins, etc. How much info do we have to store? It will be c<sup>w*h</sup>. We need to track Mario's velocity, which thankfully is a fixed number of choices. And score and time. These can be large integers unfortunately. We also need to track where the screen is relative to the final position of the level.

You can enumerate these values.

# 23. Computational Complexity

Sometimes you can't solve problems in polynomial time. Consider the following three complexity cases: P, EXP, and R.

- P = the set of all problems that can be solved in polynomial time. Nearly every problem in this lecture series covers this. O(n<sup>c</sup>) or better.

- EXP = the set of all problems that can be solved in exponential time. i.e. O(c<sup>n</sup>)

- R = the set of all problems that are solvable in finite time. For some reason R stands recursive, which meant something different back in the 1930s when this idea of R problems was coined.

Beyond that there are problems beyond R which are unsolvable.

Example of problems:

- Negative-weight cycle detection is a P problem.

- n*n chess. You have a set of pieces, and you ask which side is going to win based on this board configuration? This is a EXP problem, and does not exist in P. Lots of games are in this category.

- Tetris is also a EXP problem, and it is probably not in P.

- The Halting Problem is given a computer program (Python, Java, etc.) does it ever halt/stop running? This problem is not in R.

- Most decision problems are not computable in R. This is provable via set theory. A program consists of a binary string which consists of a natural number in Set N. Meanwhile a decision problem is a function that has an input which is a binary string that belongs in N, and it produces a binary response: yes/no, True/False, 1/0. It can be an infinite string of bits, whereas a program is a finite string of bits. The decision problem is a real number that exists in Set R. Set R is larger than Set N. That means there are more problems than there are solutions, so almost every problem is unsolvable by any program.

Between P and EXP there is NP. i.e. the set of all problems that can be solved in nondeterministic polynomial time. It often consists of decision problems that are solvable in polynomial time only via a "lucky" algorithms. A "lucky" algorithm, unlike dynamic programming, only guesses once, and it makes the correct guess. This is called a nondeterministic model. It's not something you can actually write on a computer. But essentially, the idea is if you are looking for a YES, then the lucky algorithm will just find it.

Tetris is a NP problem.

Another way of thinking about NP problems is they are decision problems with "solutions" that can be "checked" in polynomial time. In other words, we might not be able to find a solution in polynomial time, but if we do have a solution in front of us, it can be confirmed in polynomial time.

Since computers can't seem to engineer lucky algorithms, we say that P != NP. Although this hasn't been formally proven.
