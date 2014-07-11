---
layout: post
title: Graph Theory Part 2 - Graph Traversal
---
![](/public/img/g2/traversal.jpg "graph traversal")

*Traversing every edge and vertex in a graph* is one of the most fundamental graph problems. We need to care about couple of things:

* **Efficiency:** We need to make sure to visit each edge *at most twice*.
* **Correctness:** We must do the traversal in a systematic way. Thus, we don't miss anything (in other words, we'll explore everything).

You can think a graph as a **maze**, and we are trying to get out with the most *surefire* way, and we are trying to be as *quick* as possible. And we need to write an algorithm that gets us out of any arbitrary maze.

A few considerations:

* we will try every way
* we will know when to stop and try another way (e.g. don't turn around a cycle repeatedly)
* we will leave *breadcrumbs* so that we'll know where we have been.

## Marking Vertices
### Key Idea
We must mark each vertex when we first visit it, and keep track of what have not yet completely explored.

**_Each vertex will be in one of the three stages_** *(in this order)*:

* **Undiscovered:** we have never been here
* **Discovered:** we have been here, but not finished its all edges.
* **Processed:** we completely explored here, and we'll move on.

### To-Do List
We should maintain a structure like a to-do list to see what we need to process.
This structure will contain vertices that we have **discovered but not processed**.

Naturally, only a single vertex is discovered. Our *start* vertex. Since we are starting from somewhere, we are automatically discover that vertex.

To explore (process) a new vertex, we need to look at all edges *going out of it*.

For each edge which goes to an undiscovered vertex, mark it *discovered* and add it to the to-do list.

Regardless of the order we fetch the next vertex to explore, each edge is considered exactly twice at the end. Every edge and vertex is eventually visited.

There are two fundamental search/traversal algorithms:

* Breadth First Search (BFS)
* Depth First Search (DFS)

## Breadth First Search
Here, we have two fundamental operations.

* Visit a node (vertex)
* Gain access to its neighbors (edges!)

BFS begins at the root node and inspects all the neighbors. For each neighbors, it does the same (inspecting neighbors). We have a *queue* to track those. Queue has a function of to-do list. We add vertices that we discovered to our queue, then we visit all its edges, then we remove it from the queue.
Algorithm is as follows:

```
Enqueue the root node to Q
Dequeue a node and inspect
    (if found the target: return result)
    (Otherwise: enqueue undiscovered neighbors)
If Q is empty, that means we examined all the vertices. Quit search.
If Q is NOT empty, GOTO step 2
```

Let's write that in Java:

```
static void bfs(Graph g, int start) {
    Queue<Integer> q = new LinkedList<Integer>();
    int v, vNeighbor;
    List<Integer> p;
    q.add(start);
    discovered.put(start, true);
    while(!q.isEmpty()) {
        v = q.remove();
        System.out.printf("discovered vertex %d\n", v);
        p = g.getNeighbors(v);
        for (int e : p) {
            vNeighbor = e;
            if (!processed.get(vNeighbor)) {
                System.out.printf("processed edge (%d, %d)\n", v, vNeighbor);
            }
            if (!discovered.get(vNeighbor)) {
                q.add(vNeighbor);
                discovered.put(vNeighbor, true);
                parents.put(vNeighbor, v);
            }
        }
        processed.put(v, true);
        System.out.printf("processed vertex %d\n\n", v);
    }
}
```

![](/public/img/g2/bfs.gif "this is bfs")

## Depth First Search
*Wait for it..*
