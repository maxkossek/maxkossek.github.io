---
layout: post
title: Algorithm Design Manual (Skiena)
date: 2019-07-03
categories: [book]
tags: [computer science, mathematics]
author: "Max Kossek"
description: Notes on Skiena's book the Algorithm Design Manual.
sitemap:
    lastmod: 2019-10-21
---

## I Practical Algorithm Design

### 1 Introduction to Algorithm Design
Algorithms are a reproducible, general way of solving a problem.[^book] The goal is to minimize costs and make the algorithm as simple as possible. Heuristics usually do a good job, but don't guarantee correct results. To show correctness, we need to provide proofs. Counterexamples are the best way to disprove the correctness of heuristics. Induction and recursion are commonly used to demonstrate the correctness of an algorithm. Modeling the problem in terms of known structures and algorithms is the most important step towards a solution.


### 2 Algorithm Analysis
Efficiency of algorithms is evaluated using "big Oh" notation. We usually use worst-case analysis for run-time analysis. Constant factors are often ignored because as n grows, lower order terms get less relevant. Big-Oh notation states: f(n) = O(g(n)) if c\*g(n) is an upper bound on f(n) for all n >= n<sub>0</sub>. Similarly, f(n) = Ω(g(n)) states that c\*g(n) is a lower bound and f(n) = Θ(g(n)) states that c<sub>1</sub>\*g(n) is an upper bound and c<sub>2</sub>\*g(n) is a lower bound. 

Functions can be categorized into classes such as Θ(n). A faster growing set of functions is said to dominate a slower growing class of functions (f(n) = O(g(n)) or g>>f or "g dominates f"). The domination hierarchy is: n! >> 2<sup>n</sup> >> n<sup>3</sup> >> n<sup>2</sup> >> nlogn >> n >> logn >> 1. The sum of two functions is the dominant one: O(f(n)) + O(g(n)) = O(max(f(n),g(n))). Multiplying by a constant does not affect the asymptomatic behavior: O(c\*f(n)) = O(f(n)). Multiplying by a function does change the asymptomatic behavior: O(f(n)) * O(g(n)) = O(f(n)\*g(n)). 

Logarithms are the inverse exponential function: b<sup>x</sup> = y is equal to x = log<sub>b</sub>y. Logarithms simplify multiplication: a<sup>b</sup> = e(ln(a<sup>b</sup>)) = exp(b*ln(a)). Important property: log<sub>a</sub>(xy) = log<sub>a</sub>(x) + log<sub>a</sub>(y); and log<sub>a</sub>b = [log<sub>c</sub>b] / [log<sub>c</sub>a]. There are three primary bases of logarithms: 
1. lgx or 2: binary logarithm
2. lnx or e: natural logarithm;
3. logx or 10: common logarithm.


### 3 Data Structures
Contiguous data structures are allocated in one block of memory (arrays, matrices, heaps), while linked data structures are connected by pointers in memory (linked list, trees, graph adjacency lists). The advantages of contiguously allocated memory are: constant-time access by index, space efficiency, memory locality. The main disadvantage of contiguously allocated memory is it's fixed size. 

Containers are data structures that store and retrieve data independent of content. There are two main container data structures: 
1. Stacks - "Last in first out"
2. Queues - "First in first out"

Dictionary data types provide access to data items by content. A sorted dictionary is faster on most search tasks, but requires longer for insertion and deletion. This is because all elements have to be rearranged to maintain the sorted state.

A binary search tree has a root, a left subtree with x less than the root (x < root) and a right subtree where x is greater than the root (x > root). Most operations except traversal take O(logn) time on a balanced binary search tree. If the tree is not balanced, operations can take linear time at worst. Priority queues sort and evaluate items based on their importance. Humans naturally think in terms of priority queues (evaluating partners, scheduling). 

Hash functions map keys to integers. The value of the hash function stores the index in the array. When two keys hash to the same value, chaining with pointers can be used to resolve the collision.


### 4 Sorting and Searching
Sorting is the most common algorithm used in practice. It forms the bases for the strategies of many other algorithms. Many programming languages have sorting functions already built in their libraries (e.g `qsort(a,n,sizeof(int),intcompare);` in C). 

Selection sort simply looks through the list and deletes the smallest remaining element in O(n^2). Heapsort is similar to selection sort, but it uses a list that is already sorted into a binary tree or priority queue. This reduces the time complexity to O(nlogn). In a min-heap, each parent is smaller than it's children. We can store heaps in arrays and simply use a formula to index into a certain level or to access the child of a parent. The left child is in position 2k and the right child in position 2k+1. For heaps to work, they must always be filled top to bottom and left to right and can't have holes. A new item is always inserted at the n+1 position. The item is then moved up depending on if it's a min or max-heap. When deleting the first element, we replace it with the last element in the heap. Again, we move down the new first elements depending on max/min-heap requirements.

Mergesort recursively partitions elements into two groups and then interleaves the sorted groups back together in O(nlogn) time. Quicksort selects some pivot elements and then separates items into a low pile and a high pile. Then the low and high piles are sorted independently. If quicksort splits the piles into equal parts the best case running time is O(nlogn), however in the worst case it is O(n^2). On average quicksort is very good (2-3x faster than merge or heapsort) with average height after n insertions of 2ln(n). Randomization and random sampling can be used to increase the probability of a good pivot selection. 

Bucketsort distributes the elements into buckets, such as 26 for the starting letter of a word. Bucketing is effective when the distribution of the data is roughly uniform. Binary search repeatedly determines if the element is in the bottom or top half of an array, taking O(logn).


### 5 Graph Traversal
Graph G = (V,E) consists of vertices V and vertex pairs or edges E of ordered or unordered pairs of V. Properties of graphs:
- Undirected vs Directed: If edge (x, y) ∈ E implies that (y, x) is also in E then the graph is undirected (lanes in both directions). If direction matters then the graph is directed (one-lane road).
- Weighted vs Unweighted: Weighted graphs assign numerical values or weights to each edge (for example length, drive-time, speed limit for a road network graph).
- Simple vs Non-simple: Non-simple graphs contain self-looping edges (x,x)->(x,x) or multi-edge edges (edges that occur more than once in a graph).
- Sparse vs Dense: Sparse graphs have a small fraction of vertex pairs with defined edges. Sparse graphs typically have linear number of edges vs while dense graphs have quadratic number of edges.
- Cyclic vs Acyclic: Acylic graphs have no cycles (e.g. trees)
- Embedded vs Topological: Embedded graphs have vertices and edges with geometric positions. Topological graphs are defined by some other outside boundary.
- Implicit vs Explicit: Implicit graphs are built as we use them, instead of being explicitly constructed and traversed.
- Labeled vs Unlabeled: Labeled graphs assign a unique name or identifier to each vertex. In unlabeled graphs no distinction between nodes is made.

Adjacency matrices represent G = (V,E) as a matrix where M(i,j) is 1 if it is an edge, and 0 if is not. Adjacency lists use linked lists to store the neighbors adjacent to each vertex (not explicitly stating non-connections). Adjacency lists are more advantageous for most applications and especially sparser graphs.

A key idea in graph traversal is to mark each vertex on the first visit to keep track of what hasn't been explored. Breadth-first search assigns a direction to each edge (parent -> child). Each node has exactly one parent expect the root. Depth-first search uses a different container data structure than Breadth-first search to store the discovered vertices:
- Breadth-First search uses a queue: Explore the oldest unexplored vertices first. Exploration radiates out from the starting vertex.
- Depth-First search uses a stack: Explore new vertices when available, and go back to older vertices only when surrounded by discovered vertices. Exploration wanders away from the starting vertex.

Articulation vertices or cut-nodes are single vertices that individually connect different components of a graph.


### 6 Weighted Graph Algorithms
No optimized algorithm exists for the traveling salesman problem.

The spanning tree of a graph G = (V,E) is the subset of edges from E that form a tree connecting all vertices of V. Minimum spanning trees minimize the sum of edge weights. Prim's minimum spanning tree algorithm starts from one vertex and grows to the rest of the tree until all vertices are included. It then selects the smallest weight edge that enlarges the number of vertices in the tree. This algorithm is O(n^2). 

Kruskal's Algorithm is also greedy, but starts by building up connected components of vertices instead of a particular vertex. It repeatedly considers the lightest remaining edge and tests whether it's endpoints lie within the same connected component. If it is, then the edge is discarded, else it is inserted. This algorithm takes O(mn). Maximum spanning trees can be found by negating all the weights of the edges, and running Prim's algorithm.

A Path is a sequence of edges connecting two vertices. The shortest path from s to t in an unweighted graph can be constructed by using breadth-first search from s. In weighted graphs breadth-first search does not work. 

Dijkstra's Algorithm can be used to find the shortest path in a weighted graph. Given starting vertex s, it finds that path from s to every other vertex in the graph. The algorithm proceeds in rounds, each time establishing the shortest path from s to some new vertex. x is the vertex that minimizes: dist(s,v_i) + w(v_i,x), over all unfinished 1 <= i <= n, where w(i,j) is the length of edge and dist(i,j) is the length of the shortest path between them. This algorithm is similar to Prim's algorithm, but it also takes into account the weights of vertices. Running time is O(n^2).

Floyd's Algorithm solves the problem of finding the "center" vertex in a graph (the vertex that minimizes the longest or average distance to all other nodes). It uses an adjacency matrix, but instead of storing 1's for edges and 0 for non-edges, it initializes each non-edge to MAXINT. This way we test for it's presence, while automatically ignoring it in shortest-path. The Floyd-Warshall algorithm starts by numbering the vertices of a graph from 1 to n. W[i,j]^k is the length of the shortest path from i to j using only vertices numbered from 1,2,...,k as possible intermediate vertices. We perform n iterations, until we are left with only the first k vertices at the kth iteration. Running time is O(n^3).

Edge-weighted graphs can be thought of as a network of pipes, where the weight of each edge(i,j) determines the capacity of a pipe. Network flow problems want to determine the maximum amount of flow which can be sent from vertex s to t, while respecting the maximum capacities of each pipe. Matching in a graph G = (V,E) is the subset of edges E' such that no two edges E' share a vertex. Every vertex is in at most one pair (one connection). Bipartite matching for a graph G divides the vertices into two sets L and R, such that all edges have one vertex in L and one vertex in R. 

Edmonds-Karp algorithm finds the maximum flow in a network graph. It uses the method of augmenting paths, where a path of positive capacity from s->t is repeatedly added to the flow. Flow through a network is optimal, if and only if it contains no augmenting path. Each augmentation adds to the flow, so eventually we find the global maximum. Running time is O(n^3).


### 7 Combinatorial Search and Heuristic Methods
Backtracking is a systematic way of iterating through all possible configurations of a search space. At each step the backtracking algorithm tries to add another element to a partial solution a = (a_1,a_2,...a_k). After extending, we check whether it is a possible solution; if yes then print it or count it, else test whether the partial solution can still be used to find the complete solution.

There are 2^n subsets of n elements (e.g. for n=2, {} {0,1} {1,0} {1,1}). To construct all 2^n subsets, we can use an array/vector of n cells where the value of a_i signifies whether the ith item is in the subset. For permutations there are n distinct choices for the first element. Once a_i is fixed, there are n-1 candidates remaining for the second position (all values except a_i). There are a total of n! distinct permutations. For the enumeration of all simple s to t paths we start with s in the first position. The second position includes all the vertices v such that (s,v) is an edge of the graph. Generally S_k+1 is the set of all vertices adjacent to a_k that have not been already used in the partial solution.

Pruning is a technique that restrict the set of next elements to those that make legal moves from the current configuration. As soon as a partial solution cannot be extended into a full solution it is cut off. Combinational searches alone can find the solution to small optimization problems of size 10<=n<=14. Pruning increases this range to about 15<=n<=50 items. Looking ahead to eliminate possibilities is often the best way to prune a search.

Heuristic search methods require some way to represent the solution space and a cost function to evaluate the quality of each element in the solution space. Examples of Heuristic Search Methods:
1. Random Sampling or Monte Carlo method: Repeatedly construct random solutions and evaluate them, stopping as soon as the solution is good enough. True random sampling selects elements from the solution space uniformly at random. Applications: (1) Problems with a high proportion of acceptable solutions; (2) If there's no coherence in the solution space, so that there is no way to tell if one is getting closer to a solution.
2. Gradient-descent or local search: Uses the local neighborhood around every element in the solution space. We start form an arbitrary element and then scan the neighborhood for a favorable transition to take. Applications: (1) High coherence in the solution space; (2) When the cost of incremental evaluation is much cheaper than global evaluation.
3. Simulated annealing: Allows occasional transitions leading to more expensive (and inferior solutions). It is a way to avoid getting stuck in local optima.

Genetic algorithms maintain a "population" of solution candidates. Two parent candidates are picked and combined to allow them to "reproduce". The probability that an element is chosen to reproduce depends on it's fitness, or how close it is to the desired outcome. Genetic algorithms are usually less efficient than stimulated annealing.

Parallel processing makes use of cluster computing and multicore processes to run multiple distinct processes. Drawbacks to parallel processing:
1. Smaller upper bound on how much efficiency can be gained by parallelizing. Great performance gains mostly come from finding a better algorithm.
2. Speedup doesn't mean much: more processors and linear speedup can eventually beat any sequential algorithm. But most good algorithms beat parallelized code and perform much less work
3. Tough to debug, since the processes must communicate with each other. 

Parallel processing should only be used when sequential solutions of a problem are too slow. When parallelizing a problem try to reduce the need for communication among processes and split the problem evenly.


### 8 Dynamic Programming
Dynamic programming combines methods from greedy and exhaustive algorithms. It searches all possibilities (to guarantee correctness), but stores results to avoid recomputing (to be more efficient). Dynammic programming is usually a good method for left-to-right objects such as character strings, rooted trees, polygons and integer sequences.

To compute the Fibonacci sequence we can cache the results of each computation in a table structure indexed by parameter k. We can avoid recomputing all the recursive values down to 0 and 1, by first checking for the value in the table. Explicitly storing return values only works for recursive functions that have recurring parameter values. Similarly, binomial coefficients can be calculated by storing the results of the recursive additions in Pascal's triangle. 3 steps for solving a problem by dynamic programming:

1. Formulate the answer as a recurrence relation or recursive algorithm.
2. Show that the number of possible parameter values taken on is bounded by a polynomial.
3. Specify an order of evaluation for the recurrence so that partial results are available when you need them.

Dynamic programming algorithms are only correct if they are based on sound recurrence relations. In general, dynamic programming can be applied to problems that observe the principle of locality: meaning partial solutions can be extended with regard to the state after the partial solution, rather than the specifics of the solution itself. Future decisions are made based on the consequences of previous decisions, not the actual decisions themselves.

The running time of dynamic programming algorithms depends on how many partial solutions need to be kept track off and how long it takes to evaluate each partial solution.


### 9 Intractable Problems and Approximation Algorithms
NP-completeness allows us to save time by proving that there is no efficient solution for an algorithm. It also helps us identify what properties make a problem hard. To prove NP-completeness we use reductions between pairs of problems to show that they are equivalent.

Algorithmic problem - General question, with parameters for input and conditions on what is a satisfactory solution or output. An instance is a problem with the inputs specified. For example:
```
Problem: The Traveling Salesman Problem   
Input: Weighted Graph G   
Output: Which tour {v1,v2,...,vn} minimizes Σ(i=1)to(n-1) d[v_i,v_i+1] + d[v_n,v_1]?
Instance: Any weighted graph
```

Reductions translate instances from one type of problem to instances of another such that the answers are preserved. They are a way of showing that two problems are identical: finding a fast algorithm (or not) for one of the problems, implies there is a fast algorithm (or not) for the other. If we can translate the input for a problem we want to solve into the input for a problem we know how to solve, we can compose the translation and the solution into an algorithm for our problem. It's important to specify the correct direction of reduction: we must transform every instance of a known NP-complete problem into an instance of the problem we are interested in. To use reduction, we should choose problems of the same class. Decision problems are the simplest class of problems, since their answers return only true/false.
```
Bandersnatch(G):
Translate input G to instance Y of Bo-billy problem
Call sub-routine Bo-billy on Y to solve instance
Returns answer of Bo-billy(Y) as answer to Bandersnatch(G)
```

The Hamiltonian cycle is a hard graph theory problem that seeks to find a tour that visits each vertex in an unweighted graph G without repetition. It differs from the Traveling Salesman Problem because it works on unweighted graphs. We perform a translation from unweighted to weighted graphs in O(n^2) time, which ensure that the answers of the two problems are identical. Since we have a hardness proof of Hamiltonian cycles, this implies that the Traveling Salesman Problem is hard as well.
```
HamiltonianCycle(G = (V,E)):
Construct a complete weighted graph G' = (V',E') where V' = V
n = |V|
for i = 1 to n do
	for j = 1 to n do
		if (i,j) ∈ E then w(i,j) = 1 else w(i,j) = 2
Return answer to Traveling-Salesman-Decision-Problem(G',n)
```

Similarly we can reduce the independent set problem to a Vertex Cover. Independent sets are subsets of vertices with no edges, while vertex covers are subsets of vertices that represent every edge. Since vertex cover is hard, independent set finding must be hard as well.
```
Problem: Vertex Cover   
Input: Graph G = (V,E) and integer k <= |V|   
Output: Is there subset of S of at most k vertices such that ever e ∈ E contains at least one vertex in S?

Problem: Independent Set   
Input: Graph G and integer k <= |V|   
Output: Does there exist an independent set of k vertices in G?

VertexCover(G,k)
G' = G
k' = |V| - k
Return the answer to IndependentSet(G',k')
```

Satisfiability is the problem that is the first link in the chain of reductions. Satisfiability is the mother of all NP-complete problems because it is absolutely, certifiably and undeniably hard. You need 3 literals per clause to turn the problem of satisfiability from polynomial to hard.
```
Problem: Satisfiability   
Input: Set of Boolean Variables V and set of clauses C over V   
Output: Does there exist a satisfying truth assignment for C; is there a way to set variables [v1,...,v_n] to true or false so that each clause contains at least one true literal?

Example: C = [[v1,!v2],[!v1,v2]] over variables V = [v1,v2] can be satisfied by either setting both v1,v2 to true or both to false.   
Example: C = [[v1,v2],[v1,!v2],[!v1]] has no satisfying assignment for variables V.

Problem: 3-Satisfiability (3-SAT)   
Input: Collection of clauses C where each clause contains 3 literals, over set of Boolean variables V.   
Output: Is there a truth assignment to V such that each clause is satisfied.
```

Skiena's advice for proving hardness:
1. Look in textbooks that list the several hundred problems that are known to be NP-complete.
2. Make the source problem as simple (restricted) as possible.
3. Use 3-SAT rather than full satisfiability for proves.
4. Make the target problem as hard as possible.
5. Select the right source problem for the right reason: 3-SAT for most problems, integer partition for problems requiring large numbers, vertex cover for graph problems with selection, Hamiltonian path for graph problems with ordering.
6. Amplify penalties for making the undesired selection.
7. Alternate between finding an algorithm and a reduction.

Algorithms of class P have polynomial-time algorithms to solve them (e.g. shortest path, minimum spanning tree). Some algorithms can be verified in polynomial-time, but can't be solved in P (e.g. traveling salesman, satisfiability, vertex cover). NP is the class of problems that is not necessarily polynomial-time. If you prove that satisfiability is polynomial-time, then all other NP problems can join the class of P problems as well. This is because all NP problems are reductions of the satisfiability problem. 

Although we can't conclusively prove that satisfiability isn't hard, for all practical purposes P != NP. A problem is NP-hard if it is at least as hard as any problem in NP (e.g. satisfiability). NP-complete is a problem that is NP-hard and also in NP itself. Most NP-hard problems are also NP-complete. To deal with problems that are NP-complete:

1. Find Algorithms that are fast in the average case. For example backtracking and pruning.
2. Heuristics such as simulated annealing or greedy approaches can find good enough solutions in a fast way.
3. Approximation algorithms can be used to get close to the optimal answer with some level of guarantee.


[^book]: Skiena, S. S. (1998). The algorithm design manual: Text (Vol. 1). Springer Science & Business Media.