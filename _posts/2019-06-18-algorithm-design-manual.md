---
layout: post
title: Algorithm Design Manual (Skiena)
date: 2019-07-03
categories: [book]
tags: [computer science, mathematics]
author: "Max Kossek"
description: Notes on Skiena's book the Algorithm Design Manual.
---


## I Practical Algorithm Design


### 1 Introduction to Algorithm Design
Algorithm is a reproducible, general way of solving a problem [^book]. Goal is to minimize costs and make the algorithm as simple as possible. Heuristics usually do a good job, but don't guarantee correct results such as Algorithms. To show the correctness, we need to provide proofs. Searching for counterexamples is the best way to disprove the correctness of heuristics. Induction and recursion are commonly used to demonstrate the correctness of an algorithm. Recursion has to have a base case that it approaches.

There are two basic classes of summation: (1) Arithmetic - S(n,p) = <sub>i</sub>Σ<sup>n</sup> i<sup>p</sup> = θ(n<sup>p+1</sup>); (2) Geometric - G(n,a) = <sub>i=0</sub>Σ<sup>n</sup> a<sup>i</sup> = a(a<sup>n+1</sup> - 1)/(a - 1). Modeling the problem in terms of known structures and algorithms is the most important step towards a solution.


### 2 Algorithm Analysis
Efficiency of algorithms is evaluated with "big Oh" notation. We usually use worst-case analysis for run-time analysis. We ignore specifics such as constant factors because as n grows, these get less relevant. Big-Oh notation states: f(n) = O(g(n)) if c\*g(n) is an upper bound on f(n) for all n >= n<sub>0</sub>. Similarly, f(n) = Ω(g(n)) states that c\*g(n) is a lower bound and f(n) = Θ(g(n)) states that c<sub>1</sub>\*g(n) is an upper bound and c<sub>2</sub>*g(n) is a lower bound. 

Functions are put in classes such as Θ(n), and a faster growing set of functions is said to dominate a slower growing class of functions (f(n) = O(g(n)) or g>>f or "g dominates f"). The domination hierarchy is: n! >> 2<sup>n</sup> >> n<sup>3</sup> >> n<sup>2</sup> >> nlogn >> n >> logn >> 1. The sum of two function is the dominant one: O(f(n)) + O(g(n)) = O(max(f(n),g(n))). Multiplication by a constant does not affect the asymptomatic behavior: O(c\*f(n)) = O(f(n)). Multiplying by a function changes the asymptomatic behavior: O(f(n)) * O(g(n)) = O(f(n)*g(n)). 

Logarithm is inverse exponential function: b<sup>x</sup> = y is equal to x = log<sub>b</sub>y. Logarithms simplify multiplication operations: a<sup>b</sup> = e(ln(a<sup>b</sup>)) = exp(b*ln(a)). Important property: log<sub>a</sub>(xy) = log<sub>a</sub>(x) + log<sub>a</sub>(y); and log<sub>a</sub>b = [log<sub>c</sub>b] / [log<sub>c</sub>a]. There are three primary bases of logarithms: 
1. lgx or 2 - binary logarithm;
2. lnx or e - natural logarithm;
3. logx or 10 - common logarithm.


## 3 Data Structures
Contiguous data structures are allocated in one block of memory (arrays, matrices, heaps), while linked data structures are connected by pointers in memory (linked list, trees, graph adjacency lists). Contiguously-allocated memory advantages - constant-time access by index, space efficiency, memory locality; disadvantages - fixed size. 

Containers are data structures that store and retrieve data independent of content. Two main container data structures: (1) Stack - Last in first out; (2) Queues - First in first out. Dictionary data type provides access to data items by content (search, insert, delete). A sorted dictionary is faster on most tasks, but requires longer for insertion and deletion, because all the elements have to be rearranged to maintain the sorted state.

A binary search tree has a root, and a left subtree with x < root and a right subtree where all x > root. Most operations except traversal take logn time on balanced binary search trees. If the tree is not balanced, operations can take linear time at worst. Priority Queues sorts and evaluated items based on their importance. Humans naturally think in terms of priority queues (evaluating partners, scheduling). Hash functions map keys to integers. The value of the hash function stores the index in the array. When two keys hash to the same value, chaining with pointers is used for collision resolution.


## 4 Sorting and Searching
Sorting is the most common algorithm used in practice. It forms the bases for the strategies of many other algorithms. There are sorting algorithms that have reduced time complexity from O(n^2) to O(nlogn). Many programming languages have sorting functions already built in their libraries (`qsort(a,n,sizeof(int),intcompare);` in C). 

Selection sort simply looks through the list and deletes the smallest remaining element in O(n^2). Heapsort is just selection sort, using a list that is already sorted through a binary tree or a priority queue. This way we can reduce the time complexity to O(nlogn). In a min-heap, each parent is smaller than it's children. We can store heaps in arrays and simply use a formula to index into a certain level or a child of a parent (i.e [1,2,2,3,3,3,3] would mean 2 and 2 are children of 1). The left child is in position 2k and the right child in 2k+1. For heaps to work, they must always be filled top to bottom and left to right and can't have holes. A new item is always inserted at the n+1 position. The item is then moved up depending on if it's a min or max-heap. When deleting the first element of a heap, we replace if with the last heap. Again, we move down the new first elements depending on max/min-heap requirements.

Mergesort recursively partitions elements into two groups and then interleaves the sorted groups back together in O(nlogn) time. Quicksort selects some pivot elements and then separates items into a low pile and a high pile. This way we can sort the low pile and high pile independently. If quicksort splits the piles into equal the best case is O(nlogn), however in the worst case it is O(n^2). On average quicksort is very good (2-3x faster than merge or heapsort) with average height after n insertions of 2ln(n). Randomization and random sampling can be used to increase the probability of a good pivot selection. 

Bucketsort distributes the elements into buckets, such as 26 for the starting letter of a word. Bucketing is effective when the distribution of the data is roughly uniform. Binary search repeatedly determines if the element is in the bottom or top half of an array, taking O(logn).



## 5 Graph Traversal
Graph G = (V,E) consists of vertices V and vertex pairs or edges E of ordered or unordered pairs of V. Properties of graphs:
- Undirected vs Directed: If edge (x, y) ∈ E implies that (y, x) is also in E then the graph is undirected (lanes in both directions). If direction matters then the graph is directed (one-lane road).
- Weighted vs Unweighted: Weighted if each edge is assigned a numerical value or weight (for example length, drive-time, speed limit for a road network graph).
- Simple vs Non-simple: Non-simple if contains self-looping edges (x,x)->(x,x) or multiedge edges (edges that occur more than once in a graph).
- Sparse vs Dense: Sparse if small fraction of vertex pairs have edges defined for them. Sparse graphs typically have linear number of edges vs quadratic for dense graphs.
- Cyclic vs Acyclic: Acylic graph has no cycles (e.g. trees)
- Embedded vs Topological: Embedded if vertices and edges have geometric positions. Topological is when the graph is defined by some other outside boundary.
- Implicit vs Explicit: Implicit graphs are built as we use them, instead of explicity constructued and traversed.
- Labeled vs Unlabeled: Labeled if each vertex is assigned a unique name or identifier. In unlabeled no distinction is made.

Adjacency matrix represent G = (V,E) as a matrix where M(i,j) is 1 if it is an edge, and 0 if is not. Adjacency lists use linked lists to store the neighbors adjacent to each vertex (don't explicitly state non-connections). Adjacency lists are more advantegous for most applications and especially sparser graphs.


[^book]: Skiena, S. S. (1998). The algorithm design manual: Text (Vol. 1). Springer Science & Business Media.
