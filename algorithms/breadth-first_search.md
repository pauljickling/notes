## Breadth-First Search

A breadth-first search is a graph algorithm that is used to find if a path exists between two points, and if so the shortest distance between them.

The breadth-first search answers its two questions by checking the neighbors of the starting node to see if it satisfies the condition of the search. If not, only then will it move onto nodes that are two steps away from the starting node. By searching in this sort of order, the breadth-first search algorithm forms a *queue*.

Here is a very rudimentary implementation of a breadth-first search that finds the shortest path in Python. Note that this implementation was written with brevity and clarity in mind, but the data structures used can cause some odd side effects so you would not want to use it in production.

```
graph = {}

graph['a'] = ['b', 'c']
graph['b'] = ['a', 'd']
graph['c'] = ['a', 'd']
graph['d'] = ['b', 'c', 'e']
graph['e'] = ['d']

def bfs(start, item):
    visited = set([start])

    if item in visited:
        count = 0
        return count

    queue = []
    search = graph[start]
    count = 1

    while search:
        node = search.pop()
        visited.add(node)
        if item in visited:
            return count
        queue.extend(graph[node])
        if search == [] and queue == []:
            return None
        if search == []:
            count += 1
            for queued_item in queue:
                if queued_item not in visited:
                    search.append(queued_item)
            queue.clear()
```

In this example a hash table is used for the graph, where keys act as a node pointing to a value of a list of adjacent nodes.

The function takes two parameters, the starting node, and the item, i.e. the node that you are looking for the shortest path for.

There are multiple data structures to take note of here.

1. The count is a number that is incremented as the search progresses, and its value is returned when the node we are searching for ("item") is found.

2. The search list, which starts with the values from the starting node in our graph hash table. When the search list is exhausted, the value of count is incremented, and we add the values from the queue list that are not in the visited set into the search.

3. A set of visited nodes. If the item is in one of the visited nodes, then we can find the shortest path. As nodes are added to the visited set, we also add the values of that node from our graph hash table into the queue list.

4. The queue list contains nodes that we might want to explore after the current search list has been exhausted. First we check if the items are in the visited set though. If they are there is no need to check them again.

Note that before we check if the search list has been exhausted and needs to have items from the queue added to it, we first check to see if the search and queue lists are both empty. If they are, it means we exhausted our search without finding the item parameter, so we return None.

The run-time complexity of a breadth first search is O(V + E) where V is the number of vertices in the graph, and E is the number of edges.
