## Depth-First Search

A depth-first search is a way of exploring the entirety of a graph space by exploring a set of connected nodes as far as possible. For this to work, this search must keep track of visited nodes. The steps for the algorithm are as follows:

1. Take as input a starting node in the graph.

2. Place the starting node in the set of visited nodes.

3. Place the sibling nodes of the starting node in a search queue.

4. While the search queue is not empty, remove a node from the search queue, and check if it has been visited.

5. If it has not been visited, add the node to the set of visited nodes, and add its sibling nodes to the stack. Repeat until the search queue is exhausted.

6. Return the set of visited nodes.

The run-time complexity for a depth-first search is O(VE) where V stands for the number of nodes (vertices as they are sometimes called), and E stands for the number of edges.

One thing to bear in mind is that in a directed graph a node that cannot be visited from the starting node will not appear in your visited set nodes.

A depth-first search is slightly simpler to implement than a breadth-first search because it doesn't have to track its "frontier". However a depth-first search cannot determine a shortest path in your graph, it can only tell you what nodes are connected to the starting node.
