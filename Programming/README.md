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
    - [Hash tables](#hash-tables)
    - [Stacks & Queues](#stacks-queues)
    - [Graphs](#graphs)
    - [Trees](#trees)
2. [Algorithms](#2-algorithms)
    - [Complexity analysis](#complexity-analysis) (Time/Space complexity, Big O Notation)

## 1. Data Structures

### Data types

> **Bit** &rarr; binary digit (`0` or `1`). All data stored in a computer are represented in bits.

> **Byte** = 8 bits (256 values)

- Boolean: `true`, `false`
- Number: `int`, `float`
- Characters: `char`, `strings` &rarr; _array of `char`_

> 32-bit integer (4 bytes); 64-bit integer (8 bytes)

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

### Hash tables

### Stacks & Queues

### Trees

### Graphs

___

## 2. Algorithms

## Complexity analysis

A problem can have multiple solutions.
All these solutions are not equal, some are better than the others. 

They're more optimized, they require less resources (computation power, memory...).

Two concepts help programmers to analyze algorithms:

1. **Time complexity**: how fast an algorithm runs.
2. **Space complexity**: how much memory/space an algorithm uses up.

### Big O notation

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

## Operations

- Conditions: `if`, `else if/elif`, `else`
- Loop: `while`, `for ... in ...`
- Elementary operations: `+`, `-`, `*`, `/`, `%` &rarr; remainder or of a division

## Sorting algorithms

## Searching algorithms

## Greedy algorithms

___

> Object-oriented programming (OOP), Functional programming, Thread, mutex, Metaprogramming etc.
___

## Useful links

- [**CS Dojo**: Data Structures and Algorithms](https://youtube.com/playlist?list=PLBZBJbE_rGRV8D7XZ08LK6z-4zPoWzu5H)