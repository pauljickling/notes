## Dijkstra's Algorithm

Dijkstra's algorithm solves for the fastest path along a directed acyclic graph where the edges provide different quantities of time (called *weights*). Dijkstra's algorithm evaluates the following criteria:

1. Finds the node that involves the smallest weight
2. Updates the cost for neighbors
3. Repeats for every node in the graph
4. Calculates the path

### Assumptions

Because of the first step, Dijkstra's algorithm only works if there are no negative weight edges. Negative weight edges create a case where you cannot properly evaluate the costs of the initial nodes without considering future node costs, which is not accounted for by this algorithm. To solve graphs that involve negative weight edges you need to use the Bellman-Ford algorithm.
