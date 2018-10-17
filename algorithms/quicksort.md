## Quicksort

Quicksort is a sorting method that utilizes the divide & conquer technique. The base case for a sorting method is going to be array with either one item, or no items at all. Then there are three steps for the recursive case:

1. Pick a pivot. A pivot is an element from the array that is used as a comparison with other elements from the array.

2. Divide the array into two sub-arrays: elements less than the pivot, and elements greater than the pivot.

3. Use the quicksort method on the sub-arrays.

The quicksort method has an efficiency of O(n log n) for the average case scenario, and an efficiency of O(n<sup>2</sup>) for the worst case scenario. The efficiency of the quicksort method comes down to the pivot that is selected. Since this is an unsorted list, without knowing the contents of the list it is impossible to know what is the best pivot to select. Therefore, at a minimum, pivot selection should be random. One could  also improve pivot selection by selecting three values randomly, and selecting the middle value.

In Python:

```
def quicksort(arr):
    if len(arr) < 2:
        return arr
    else:
        pivot = arr[0]
        l_arr = [i for i in arr[1:] if i <= pivot] # left array
        r_arr = [i for i in arr[1:] if i > pivot] # right array
        return quicksort(l_arr) + [pivot] + quicksort(r_arr)
```
