# Kadane's Algorithm

Kadane's algorithm is used to calculate maximum subarray values within an array. It runs in O(n) time and O(1) space, so it is quite efficient.

Python implementation:

```
def max_subarray_sum(arr):
    max_ending_here = max_so_far = 0
    for x in arr:
        max_ending_here = max(x, max_ending_here + x)
        max_so_far = max(max_so_far, max_ending_here)
    return max_so_far
```

## Analysis

This algorithm is only useful for arrays that contain negative integers. An array that only contained unsigned integers would be trivial to solve since the maximum subarray would be the array itself.

The algorithm iterates through the array while evaluating a max value for two different variables.

`max_ending_here` evaluates between the maximum between the current array element and the prior sum of elements. Any instance where the current element is greater than the current sum means that a subarray has included a negative integer whose value is detrimental to a maximal value. `max_so_far` then acts as a value that compares various subarray maximums.
