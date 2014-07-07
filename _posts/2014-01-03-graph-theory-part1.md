---
layout: post
title: Graph Theory Part 1
---

Graph theory is one of the fundamental studies in discrete mathematics. It's the study of graphs. Here, I will talk about graph types, basic data structures and traversal algorithms for graphs, which will help you to come up with a solution to basic graph problems

## What is a graph then?
A *graph* is basically a set of vertices connected by edges. More formally, a graph *G = (V,E)* consists of a set of *vertices V*, together with a set of *edges E*.

## What makes it important?
Graphs can be used to represent *practically any relationship*. They are the most fundamental and flexible way of representing any kind of a relationship. That's what makes graph theory an important area of study.
Graph theory provides a *language* for talking about the properties of relationships, and many seemingly complex problems have a *simple* description and solution in terms of fundamental graph properties.

## Types of Graphs
It is often useful to define a graph with different degrees of generality. This helps for a better understanding of our design considerations. For example, we can define a Twitter-like relationship as "*directed simple finite graph*". Now we are speaking in graphs!

### Undirected or Directed
[picture needed]
An undirected graph which its edges has no orientation. The edge (a, b) is identical to the edge (b, a).
A directed graph is the opposite (an ordered pair). a is connected to b with *directed edges*.
For example, in Facebook, *if* **X is a friend of Y**, *then*, **Y is a friend of X**. So, if *X -> Y*, then *Y->X*
But in Twitter, *if* **X is a follower of Y**, that's it. There is no rule for that condition. It's just *X -> Y*. Of course, Y could follow X back, but there is no default rule ties them together.

### Weighted or Unweighted
[picture needed]
Unweighted graph has no cost distinction between various edges and vertices. All edges have the same value, or no value.
Weights can make sense in some contexts. Think a road network, for instance. To show distances between vertices (e.g. cities), weighted graph will be beneficial.

### Simple or Non-Simple
[picture needed]
If there is a self-loop or multi-edge on the graph, it's a _non-simple graph_.
Otherwise, we call that graph _simple_.
Think a function-call graph for a compiler. If a function have recursion, this will have a self loop and therefore, it is a non-simple graph.

### Cyclic or Acyclic
[picture needed]
A graph is cyclic if it has a cycle. For example, __trees__ are basically connected acyclic undirected graphs.


> Design graphs, not algorithms
