## Graphs

Graphs model a set of connections. These models consist of nodes and connections (connections are called *edges* in graph-theory-speak).  Nodes that are adjacent via their edges are called neighbors. Graphs can be considered directed or undirected depending on how the edges work. In a directed graph edges form a directional flow from one node to the next, whereas in an undirected graph this directional flow is not prescribed. With directed graphs you can perform a *topological sort* that makes an ordered list based off of the graph information.

Graphs can also be categorized as either weighted graphs or unweighted graphs. A breadth-first search is useful for unweighted graphs, whereas Dijkstra's algorithm is generally more useful for weighted graphs.

Graphs can also have cycles. A cycle is when the edges of the graph form a ring of nodes.

### Utility of graphs

Here are some examples of useful graphs:

1. Mapping social media relationships
2. Family trees
3. Creating an ordered list that considers dependencies

The family tree example is worth taking note of, because in graph-theory *trees* are a special subset of graphs where the directional flow of edges is always in one direction.
