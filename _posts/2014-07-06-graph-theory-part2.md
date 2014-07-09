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

A summary of what I have been describing here: 
![](/public/img/g2/bfs.gif "dope")
