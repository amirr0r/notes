# Programming

**Programming**/**Coding** is really just manipulating data in order to accomplish something.

An **algorithm** is a set of instructions that we need to follow to achieve a goal (a bit like a recipe).

**Data structures** are different ways to organize and manage data.

## Summary

1. [Data structures](#1-data-structures)
    - [Data types](#data-types)
    - [Arrays](#arrays)
    - [Linked lists](#linked-lists)
        + [Singly Linked List](#singly-linked-list)
        + [Doubly Linked List](#doubly-linked-list)
    - [Hash tables](#hash-tables) (dictionaries/objects)
    - [Stacks](#stacks)
    - [Queues](#queues)
    - [Graphs](#graphs)
        + [Directed Graph](#directed-graph)
        + [Connected Graph](#connected-graph)
        + [Cyclic Graph](#cyclic-graph)
    - :evergreen_tree: [Trees](#evergreen_tree-trees)
2. [Algorithms](#2-algorithms)
    - [Complexity analysis](#complexity-analysis) (Time/Space complexity, Big O Notation)

[Useful links](#useful-links)

## 1. Data Structures

### Data types

> **Bit** &rarr; binary digit (`0` or `1`). All data stored in a computer are represented in bits.

> **Byte** = 8 bits (256 values)

- Boolean: `true`, `false`
- Number: `int`, `float`
- Characters: `char`, `strings` &rarr; _array of `char`_

> 32-bit integer (4 bytes); 64-bit integer (8 bytes)

___

### Arrays

Collection of data accessible by numbered index starting at 0. 

> An array can contain another array so we talk about 2D, 3D ... arrays.

**Example**: `[ 1, 2, 3 ]` &rarr; The value `1` is accessible at the index `0`, `2` at the index `1` and so on and so forth. 

> **Note**: array elements are stored back to back in memory. 

#### Arrays standard operations complexities

- **Accessing a value at a given index**: `O(1)`
- **Updating a value at a given index**: `O(1)`
- **Inserting a value at the beginning**: `O(n)`
- **Inserting a value in the middle**: `O(n)`
- **Inserting a value at the end**:
    + amortized O(1) when dealing with a **dynamic array**
    + O(n) when dealing with a **static array**
- **Removing a value at the beginning**: `O(n)`
- **Removing a value in the middle**: `O(n)`
- **Removing a value at the end**: `O(1)`
- **Copying the array**: `O(n)`

___

### Linked Lists

> _"Liste (doublement) chaînées" en français._

> **Note**: linked list elements are not stored back to back in memory.

#### Singly Linked List

Each **node** of the list has a `value` and a pointer to the `next` node.

**Example:** `1 -> 2 -> 3 -> 4 -> null`

#### Doubly Linked List 

Each **node** of the list has a `value`, a pointer to the `next` node and a pointer to the `previous` node.

**Example:** `null <- 1 <-> 2 <-> 3 <-> 4 -> null`

> There are different types of linked lists like the circular ones for instance.  

#### Standard operations complexities

- **Accessing the head**: `O(1)`
- **Accessing the tail**: `O(n)`
- **Accessing a middle node**: `O(n)`
- **Inserting / Removing the head**: `O(1)`
- **Inserting / Removing the tail**: `O(n)` to access + `O(1)`
- **Inserting / Removing a middle node**: `O(n)` to access + `O(1)`
- **Searching for a value**: `O(n)`

___

### Hash tables

Also known as dictionary in Python or object in Javascript, hash tables allows us to store data with **key/value pairs**.

**Example:** `{ "name": "amir", "age": 23 }`

> Under the hood, a hash table uses a **dynamic array** of **linked lists** to store key/value pairs. When inserting a key/value pair, the hash of the key (a number) becomes an index in the dynamic array, and the value is added to the linked list at that index in the dynamic array (+ a reference to the key).

#### Hash tables standard operations complexities

- **Inserting a key/value pair**: `O(1)` on average; `O(n)` in the worse case
- **Removing a key/value pair**: `O(1)` on average; `O(n)` in the worse case
- **Looking up a key**: `O(1)` on average; `O(n)` in the worse case

___

### Stacks

A stack is data structure whose elements follow the **LIFO** order: **L**ast **I**n **F**irst **O**ut.

![stack](https://upload.wikimedia.org/wikipedia/commons/thumb/2/29/Data_stack.svg/220px-Data_stack.svg.png)

#### Stack standard operation complexities

- **Pushing an element onto the stack**: `O(1)`
- **Popping an element off the stack**: `O(1)`
- **Searching for an element in the stack**: `O(n)`

___

### Queues

A queue is the opposite of a stack whose elements follow the **FIFO** principle: **F**irst **I**n **F**irst **O**ut.

#### Queue standard operation complexities

- **Enqueuing an element into the queue**: `O(1)`
- **Dequeuing an element out of the queue**: `O(1)`
- **Searching for an element in the queue**: `O(n)`

___

### Graphs

Collection of nodes that may or may not be connected together.

> Example in real life: social medias.

Nodes are called **vertices**. Every node is a **vertex** in the graph.
Relations between them are called **edges**.

#### Directed Graph

Edges can have a direction, then we talk a **directed graph**. Otherwise the graph is **undirected**. 

> Undirected edges mean that we can go from a vertex to another in both directions.

#### Connected Graph

If there is some path between any two nodes/vertices in the graph, we say that the graph is **connected**.

If this isn't case, the graph is **disconnected**. 

#### Cyclic Graph

A **cycle** is a closed path (loop) that do not pass through several nodes/vertices.

If a graph has at least a cycle, then we call it a **cyclic graph**.
If it doesn't have any cycles, the graph is an **acyclic graph**.

> Example in real life: wikipedia links (graph -> data structures -> complexity analysis -> graph) 

___

### :evergreen_tree: Trees

A **tree** is a **connected acyclic graph**.

A tree has a **root node** and may or may not has **child nodes**.

The nodes with no child nodes are referred as **leaves** _(**leaf node**)_ :leaves:.

> **Note**: Each node can only have one parent.

> Example in real life: hierarchy in a company
 
#### K-ary tree

- A **binary tree** is a tree whose **nodes can have up to two child nodes**. 

```
      9
    /   \
   4   -26
  / \   / \
73   6 0   12
    /
   1
```

- A **ternary tree** is a tree whose **nodes can have up to three child nodes**, and so on and so for. 

We can generalize this as **k-ary trees**.

> A path between two nodes is called a **branch**.

> **BST** (Binary search trees) are special types of binary trees, where every node satisfies a specific BST property.

> A **binary tree** is **balanced** if it maintains roughly a `O log(N)` time complexity when traversing the tree by peeking a subtree at a time. Advanced types of trees: Red-black trees, AVL trees can rebalance themselves.

A **binary tree** is **complete** if every level is filled up with nodes (except for the last level which has to be filled up from left to right).

A **full k-binary tree** whose nodes have either exactly k child nodes or zero child nodes.

A **perfect binary tree** whose all of the leaf nodes have the same depth _(number of levels/height)_.

##### Heaps & Ties

- **Min-Heaps** and **Max-Heaps** are special types of binary trees, where every node satisfies the Min or Max Heap property.

- **Tries** is a tree-like data structure that typically stores characters in a string and allows to do some fancy string manipulation.

___

## 2. Algorithms

### Operations

- Conditions: `if`, `else if/elif`, `else`
- Loop: `while`, `for ... in ...`
- Elementary operations: `+`, `-`, `*`, `/`, `%` &rarr; remainder of a division

___

### Complexity analysis

A problem can have multiple solutions.
All these solutions are not equal, some are better than the others. 

They're more optimized, they require less resources (computation power, memory...).

Two concepts help programmers to analyze algorithms:

1. **Time complexity**: how fast an algorithm runs.
2. **Space complexity**: how much memory/space an algorithm uses up.

#### Big O notation

We care about how fast an algorithm runs as the size of the input increase.

We're interested in the **worst-case scenario** as well as the **average case scenario**. 

- **Constant**: `O(1)`
- **Logarithmic**: `O(log(n))`
- **Linear**: `O(n)`
- **Log**: `O(nlog(n))`
- **Quadratic**: O(n<sup>2</sup>)
- **Cubic**: O(n<sup>3</sup>)
- **Exponential**: O(2<sup>n</sup>)
- **Factorial**: `O(n!)`

____

### Sorting algorithms

#### Bubble sort 
#### Insert sort 
#### Selection sort 
#### Quick sort 
#### Heap sort 
#### Merge sort 

___

### Searching algorithms

#### Binary search

#### Graph search

##### BFS (Breadth first search)
##### DFS (Depth first search)

___

> Object-oriented programming (OOP), Functional programming, Threads, mutex, Metaprogramming etc.
___

## Useful links

- [**CS Dojo**: Data Structures and Algorithms](https://youtube.com/playlist?list=PLBZBJbE_rGRV8D7XZ08LK6z-4zPoWzu5H)
- [**HackerRank**: Data Structures](https://www.youtube.com/playlist?list=PLI1t_8YX-Apv-UiRlnZwqqrRT8D1RhriX)
- [**HackerRank**: Algorithms](https://www.youtube.com/playlist?list=PLI1t_8YX-ApvMthLj56t1Rf-Buio5Y8KL)