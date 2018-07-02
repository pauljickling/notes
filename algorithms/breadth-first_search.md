## Breadth-First Search

A breadth-first search is a graph algorithm that is used to find if a path exists between two points, and if so the shortest distance between them.

### About graphs

Graphs model a set of connections. These models consist of nodes and connections (connections are called *edges* in graph-theory-speak).  Nodes that are adjacent via their edges are called neighbors. Graphs can be considered directed or undirected depending on how the edges work. In a directed graph edges form a directional flow from one node to the next, whereas in an undirected graph this directional flow is not prescribed. With directed graphs you can perform a *topological sort* that makes an ordered list based off of the graph information.

### Utility of graphs

Here are some examples of useful graphs:

1. Mapping social media relationships
2. Family trees
3. Creating an ordered list that considers dependencies

The family tree example is worth taking note of, because in graph-theory *trees* are a special subset of graphs where the directional flow of edges is always in one direction. 

### Breadth-First Search interaction with the Graph

The breadth-first search answers its two questions by checking the neighbors of the starting node to see if it satisfies the condition of the search. If not, only then will it move onto nodes that are two steps away from the starting node. By searching in this sort of order, the breadth-first search algorithm forms a *queue*.
