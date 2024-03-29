# Minimum Spanning Tree
http://web.eecs.utk.edu/~jplank/plank/classes/cs302/Notes/MST/

### Spanning Tree: A graph with edges that connect all the vertices but dont have cycles

### Minimum Spanning Tree: A spanning tree with the minimum weight of edges

Example: the graph below on the left is a spanning tree and the graph on the right is the minimum spanning tree of the left graph

![](http://web.eecs.utk.edu/~jplank/plank/classes/cs302/Notes/MST/Ex1.jpg)


There are two main algorithims that can be used to find the minimum spanning tree: **Prim's Algorithim** and **Kruskal's Algorithim**

# Prim's Algorithim
Running time (Identical to Dijkstras Algorithim): O( E log(V))

Prim's Algorithim is a modified version of Dijkstras Algorithim. You basically just work through the graph from the starting node to the end by going through the least weighted paths

Example:

1. ![](http://web.eecs.utk.edu/~jplank/plank/classes/cs302/Notes/MST/G5-2.jpg)
2. ![](http://web.eecs.utk.edu/~jplank/plank/classes/cs302/Notes/MST/G5-8.jpg)
3. ![](http://web.eecs.utk.edu/~jplank/plank/classes/cs302/Notes/MST/G5-9.jpg)


# Kruskal's Algorithim
Running time: O( E log(V))

Works like Prim'ss Algorithim but you just sort the nodes fom smallest to largest. if it spans two components and wont cause a loop, add it
