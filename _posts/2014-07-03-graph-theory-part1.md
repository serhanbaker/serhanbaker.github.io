---
layout: post
title: Graph Theory Part 1 - Graph Types and Data Structures
---
![](/public/img/g1/title.PNG "a graph")

Graph theory is one of the fundamental studies in discrete mathematics. It's the study of graphs. Here, I will talk about graph types and basic data structures, then in part 2, I'll talk about traversal algorithms for graphs, which will help you to come up with a solution to basic graph problems. Part 3 will be about weighted graph algorithms.

## What is a graph then?
A *graph* is basically a set of vertices connected by edges. More formally, a graph *G = (V,E)* consists of a set of *vertices V*, together with a set of *edges E*.
![](/public/img/g1/v_e.PNG "vertex and edge")

## What makes it important?
Graphs can be used to represent *practically any relationship*. They are the most fundamental and flexible way of representing any kind of a relationship. That's what makes graph theory an important area of study.
Graph theory provides a *language* for talking about the properties of relationships, and many seemingly complex problems have a *simple* description and solution in terms of fundamental graph properties.

## Types of Graphs
It is often useful to define a graph with different degrees of generality. This helps for a better understanding of our design considerations. For example, we can define a Twitter-like relationship as "*directed simple finite graph*". Now we are speaking in graphs!
![](/public/img/g1/twttr.PNG "twitter: directed simple finite graph")

### Undirected or Directed
![](/public/img/g1/undirected_directed.PNG "undirected vs. directed")
An undirected graph which its edges has no orientation. The edge (a, b) is identical to the edge (b, a).
A directed graph is the opposite (an ordered pair). a is connected to b with *directed edges*.
For example, in Facebook, *if* **X is a friend of Y**, *then*, **Y is a friend of X**. So, if *X -> Y*, then *Y->X*
But in Twitter, *if* **X is a follower of Y**, that's it. There is no rule for that condition. It's just *X -> Y*. Of course, Y could follow X back, but there is no default rule ties them together.

### Unweighted or Weighted
![](/public/img/g1/unw_w.PNG "weighted vs. unweighted")
Unweighted graph has no cost distinction between various edges and vertices. All edges have the same value, or no value.
Weights can make sense in some contexts. Think a road network, for instance. To show distances between vertices (e.g. cities), weighted graph will be beneficial.

### Simple or Non-Simple
![](/public/img/g1/simple_nonsimple.PNG "simple vs. nonsimple")
If there is a self-loop or multi-edge on the graph, it's a _non-simple graph_.
Otherwise, we call that graph _simple_.
Think a function-call graph for a compiler. If a function have recursion, this will have a self loop and therefore, it is a non-simple graph.

### Cyclic or Acyclic
![](/public/img/g1/cyclic_acyclic.PNG "cyclic vs. acyclic")
A graph is cyclic if it has a cycle. For example, __trees__ are basically connected acyclic undirected graphs.


## Graph Data Structures
![](/public/img/g1/ds1.PNG "a graph")
Selecting the right data structure can have a huge impact on performance. Two basic choices are: *adjacency matrices* and *adjacency lists*. Let's assume our graph *G = (V,E)* contains *n* vertices and *m* edges. The time and space complexities of a graph algorithm are usually expressed as a function of the *number of vertices and edges*.

### Adjacency Matrix
![](/public/img/g1/adjm.PNG "adjacency matrix representation")
The adjacency matrix representation uses a n x n boolean matrix indexed by vertices, with a 1 indicating the presence of an edge, and 0 indicating the absence of an edge.

### Adjacency List
![](/public/img/g1/adjl.PNG "adjacency list representation")
In the adjacency list representation, each vertex v, has a list of vertices to which it has an edge.

## Which one is better?
Let's look at the relative performance differences between adjacency lists and adjacency matrices.

<table>
  <thead>
    <tr>
      <th>Representation</th>
      <th>Storage</th>
      <th>Add Vertex</th>
      <th>Add Edge</th>
      <th>Remove Vertex</th>
      <th>Remove Edge</th>
      <th>Query</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Adjacency Matrix</td>
      <th>O(|V|^2)</th>
      <th>O(|V|^2)</th>
      <th>O(1)</th>
      <th>O(|V|^2)</th>
      <th>O(1)</th>
      <th>O(1)</th>
    </tr>
    <tr>
      <td>Adjacency List</td>
      <th>O(|V|+|E|)</th>
      <th>O(1)</th>
      <th>O(1)</th>
      <th>O(|V|+|E|)</th>
      <th>O(|E|)</th>
      <th>O(|V|)</th>
    </tr>
  </tbody>
</table>

Generally, adjacency lists are the right data structure for most applications of graphs. The list below is completely taken from the **Algorithm Design Manual**
by __*Steven Skiena*__.
<table>
  <thead>
    <tr>
      <th>Comparison</th>
      <th>Winner</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Faster to test if (x, y) is in graph?</td>
      <td>Adjacency Matrix</td>
    </tr>
    <tr>
      <td>Faster to find the degree of a vertex?</td>
      <td>Adjacency List</td>
    </tr>
    <tr>
      <td>Less memory on small graphs?</td>
      <td>Adjacency List (|V|+|E|) vs. (|V|^2)</td>
    </tr>
    <tr>
      <td>Less memory on big graphs?</td>
      <td>Adjacency Matrix (a small win)</td>
    </tr>
    <tr>
      <td>Edge insertion or deletion?</td>
      <td>Adjacency Matrix O(1) vs. O(|E|)</td>
    </tr>
    <tr>
      <td>Faster to traverse the graph?</td>
      <td>Adjacency List Θ(|V|+|E|) vs. Θ(|V|^2)</td>
    </tr>
    <tr>
      <td>Better for most problems?</td>
      <td>Adjacency List</td>
    </tr>
  </tbody>
</table>


## </> The Code

My implementation (*Java*) uses adjacency lists as the primary data structure to represent graphs. First, I'm going to define a Node class.

```
public class Node {
    int val;
    Node next;

    public Node(int val) {
        this.val = val;
        next = null;
    }
}
```

That's it! I'll just add a toString() method to make it look more understandable:


```
public String toString() {
    StringBuilder sb = new StringBuilder();
    sb.append(val);
    sb.append("-> ");
    Node p = this.next;
    while (p != null) {
        sb.append(p.val);
        sb.append("-> ");
        p = p.next;
    }
    sb.append("X");
    return sb.toString();
}
```

This is our *Node* class. Yes, like a linked list! Now, look at the *Graph* class

```
public class Graph {
    /* Maximum possible vertex count. Yes, a bit hacky */
    static final int MAXV = 1000;

    /* I want to ignore 0, so it's 1 to MAXV */
    Node[] adjList = new Node[MAXV + 1];

    /* outdegree of each vertex */
    int[] degrees = new int[MAXV + 1];

    int nEdges; // # of E
    int nVertices; // # of V
    boolean directed;

    /* initialize */
    public Graph() {
        nEdges = 0;
        nVertices = 0;
        for (int i = 1; i < MAXV; i++) {
            degrees[i] = 0;
            adjList[i] = null;
        }
    }
}
```

As you can see, *adjList* array holds the Nodes and their neighbors.
I also keep number of vertices and edges, directed/undirected flag, and also out-degrees of each vertex.
We can hold other information such as weight, simple/nonsimple flag, and so on..


```
public void readGraph(boolean directed) {
    int x, y;
    Scanner in = new Scanner(System.in);
    nVertices = in.nextInt();
    /* # of given connections between vertices */
    int m = in.nextInt();
    for (int i = 1; i <= m; i++) {
        x = in.nextInt();
        y = in.nextInt();
        insertEdge(x, y, directed);
    }
}
```

I read graph as integers here, it can be changed but this is just a quick and dirty implementation :) First, I read number of vertices and number of edges. Then, I insert pairs of vertices with a connection between them. I also send the directness flag to *insertEdge* as a parameter.


```
void insertEdge(int x, int y, boolean directed) {
    Node p = new Node(y);
    p.next = adjList[x];
    adjList[x] = p;
    degrees[x]++;
    if (!directed)
        insertEdge(y, x, true);
    else
        nEdges++;
}
```

I insert new edges (think vertex pairs) into *adjList* as I read them. If it's an undirected graph, this function also inserts the reversed pair to the list.
For example, the code reads *"5 6"*. Then it adds a *Node* with value of 6 to the 5th position at list. If it's an undirected graph, again, it adds a *Node* with value of __*6*__ to the __*5th* position__ at list.


```
void printGraph() {
    for(int i = 1; i < adjList.length; i++) {
        Node p = adjList[i];
        if (p != null)
            System.out.println(i + ": " + p);
    }
}
```

And we print. It's actually pretty straightforward if you take a look at that.

Input format should be like this:

```
6 7
1 2
1 5
1 6
2 5
2 3
3 4
4 5
```

## A Better Implementation
Wouldn't that be better if we had a more *flexible* graph? And without creating a Node class?
It would. And this one is ~10 lines shorter. Yay!
I think the code is self explanatory, I'd even say it's easier to read.

Here we go:

```
import java.util.*;
class Graph {
    HashMap<Integer, LinkedList<Integer>> adjList;
    boolean directed;

    public Graph(boolean directed) {
        this.directed = directed;
        adjList = new HashMap<Integer, LinkedList<Integer>>();
    }

    public void addEdge(int v1, int v2) {
        adjList.get(v1).add(v2);
        if (!directed)
            adjList.get(v2).add(v1);
    }

    public void readGraph() {
        int x, y;
        Scanner in = new Scanner(System.in);
         /* # of given connections between vertices */
        int m = in.nextInt();
        for (int i = 1; i <= m; i++) {
            x = in.nextInt();
            y = in.nextInt();
            if (!adjList.containsKey(x))
                adjList.put(x, new LinkedList<Integer>());
            if (!adjList.containsKey(y))
                adjList.put(y, new LinkedList<Integer>());
            addEdge(x, y);
        }
    }

    public void printGraph() {
        for (Map.Entry<Integer, LinkedList<Integer>> entry : adjList.entrySet()) {
            Integer key = entry.getKey();
            System.out.println(key + "-> " + getNeighbors(key));
        }
    }

    public List<Integer> getNeighbors(int v) {
        return adjList.get(v);
    }
}
```

See? It's easy! See you at part 2.
