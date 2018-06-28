## Quicksort

Quicksort is a sorting method that utilizes the divide & conquer technique. The base case for a sorting method is going to be array with either one item, or no items at all. Then there are three steps for the recursive case:

1. Pick a pivot. A pivot is an element from the array that is used as a comparison with other elements from the array.

2. Divide the array into two sub-arrays: elements less than the pivot, and elements greater than the pivot.

3. Use the quicksort method on the sub-arrays.

The quicksort method has an efficiency of O(n log n).
