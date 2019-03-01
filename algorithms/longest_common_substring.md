# Longest Common Substring

Finding the longest substring common to string A and B

```
def get_substring_set(str):
    n = 1
    map = set([str])
    while n is not len(str):
        for i in range(0, len(str) - n + 1):
            sub = str[i:i+n]
            map.add(sub)
        n += 1
    return map


def compare(str1, str2):
    map1 = get_substring_set(str1)
    map2 = get_substring_set(str2)
    max = ''
    for i in map2:
        if i in map1 and len(i) > len(max):
            max = i
    return max
```
