## Binary Search

A binary search is a more efficient means of looking through a sorted list than a simple search. It searches a list by repeatedly diving the list in half until it finds the correct item.

The efficiency of a binary search is O(log n).

### Implementation

1. A function implementing a binary search needs two parameters: the list it is sorting through, and a value for the item of the list.
2. The function needs to define the low(0) and high values(the list length - 1) of the list.
3. While the low value is less than or equal to the high value, the mid value is defined as the sum of the low and high value divided by two.
4. The function makes a guess that uses the mid value as the index of the list.
5. If the guess is equal to the function's item parameter return it.
6. If the guess is greater than the item than the high value is equal to the mid value - 1.
7. If the guess is less than the item than the low value is equal to the mid value + 1.
8. The function should return a value that indicates instances where the item parameter for the function is a value that does not exist in the list.
