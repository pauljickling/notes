# LRU Cache

LRU stands for least recently used. A LRU cache will have a size n, and any items added to the LRU cache that exceed that size will remove the item that was least recently used.

A typical implementation of a LRU Cache will combine a hash table and linked list data structure. The hash table is used for efficient key, value insertions and lookups, while the linked list is used so that the head of the list will always be the least recently used, and it is updated when any of the LRU's methods like `set()` or `get()` are used.
