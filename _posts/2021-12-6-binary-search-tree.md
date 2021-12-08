---
title: "Runway reservation system and Binary Search Trees"
categories: Trees
toc: true
toc_label: ""
toc_sticky: true
---

# Introduction

In this post, we will discuss a real-world problem, which is scheduling times for flights to land on an airport. We often encounter this problem when we want to design a large-scale system that can efficiently handle requests from flights that are going to land. With our current known data structures, we can only solve this problem in $O(n)$ time, where $n$ is the current number of landing times we have scheduled. However, we want to solve this problem in $O(\lg n)$ time, and the new data structure - binary search tree will help us achieve this running time.

# Overview

We will first define the scheduling problem by stating our constraints and desired goals. Then, we will elaborate on why current data structures, such as arrays, linked lists, and heaps, cannot do better than linear time. Lastly, we introduce binary search trees and basic operations on the trees. These operations shall help us design a runway reservation system with a running time of $O(\lg n)$.

# Runway reservation system

## Definition

Given an airport with a very busy single runway, we need to design a runway reservation system for that airport to handle future landing requests. Specifically, each reservation request comes with a landing time of $t$. If there are no other landings within $k$ minutes of requested time $t$, we accept the landing request, meaning that we add $t$ to the set of scheduled landing times. Otherwise, we decline the request and do not add $t$ to our reservation set. When a plane lands safely, we remove its reserved landing time from the scheduling set.

This scenario happens when an airport with multiple runways but only one runway can be used due to weather conditions, maintenance, etc. In addition, the $k$ minutes check models the case where one plane cannot land right after another because of safety reasons. Therefore, we need to design a system given these constraints.

## Example 

|![Runway Reservation System Example]({{ site.url }}{{ site.baseurl }}/assets/images/bst/timeline.png)|
|:---:|
|The timeline of all currently scheduled landings [^1]|

Let $R$ denote the set of reserved landing times: $R = \\{ 41, 46, 49, 56 \\}$ and $k = 3$. If a plane requests for landing at $53$, we accept the request because $53$ is four larger than $49$ and three smaller than $56$. However, if a request comes for landing at $43$, it is declined because it violates the $k$ minutes check. In addition, we do not add a landing time $t$ that is smaller than the minimum of the set $R$ because it is already past.

Our goal is to design a system that can handle requests in $O(\lg n)$ time, where $n$ is the number of elements in the set $R$, or $n = \lvert R \rvert$. 

## Initial attempts

We can try solving this problem with our current known data structures:

- **Sorted linked list:** Appending or prepending a landing time to the list takes constant time, but sorting takes $\Theta(n \lg n)$ time. However, we can insert a new time rather than append or prepend and sort. The $k$ minutes check and insertion take $O(1)$ time because we only need to compare values and change pointers, but finding the insertion point is $\Theta(n)$.

- **Sorted array:** It is possible to do a binary search on a sorted array to find the insertion point in $O(\lg n)$ time. Using binary search, we find the smallest index $i$ such that $R[i] \ge t$ - in other words, $R[i]$ is the next larger element of $t$. Then, we compare $R[i-1]$ and $R[i]$ against $t$ and insert $t$ if it passes the $k$ minutes constraint. However, this process takes $\Theta(n)$ because inserting involves shifting elements to the right to make room for a new element.

- **Unsorted list/array:** We can add a new landing request to the end of the list/array in $O(1)$ time. However, performing the $k$ minutes check takes $O(n)$ time because we need to scan the entire array/list.

- **Min-heap:** Inserting a new item and maintaining the min-heap property takes $O(\lg n)$ time, but checking the constraint takes $O(n)$ time.

- **Dictionary or Python Set:** Inserting takes constant time, but the $k$ minutes check takes $\Omega(n)$ time.

Our current known data structures can check the constraint fast but insert slow, or vice-versa. Therefore, we need to devise a new data structure that supports fast check and insertion.

# Binary Search Trees (BST)

## Definition

A **binary search tree**, also called an **ordered** or **sorted binary tree**, is a rooted binary tree. We can represent a binary search tree by a linked data structure where each node is an object. In addition to a *key* and satellite data, each node contains attributes *left*, *right*, and *parent* that point to the nodes corresponding to its left child, its right child, and its parent, respectively. If a child or the parent is missing, the respective attribute contains value $\text{None}$. The root node is the only node in the tree whose parent is $\text{None}$.

In a binary search tree, the key of each node must satisfy the ***binary-search-tree property***:

Let $x$ be a node in a binary search tree. For every node $l$ in the left subtree of $x$, then $l.key \le x.key$. For every node $r$ in the right subtree of $x$, then $r.key \ge x.key$.

|![Binary Search Tree Example]({{ site.url }}{{ site.baseurl }}/assets/images/bst/bst.png)|
|:---:|
|An example of binary search trees|

As we can see above, the root of our binary search tree has a key of $51$. All the nodes in the left subtree have keys no larger than $51$, and all the nodes in the right subtree have keys no smaller than $51$.

The binary-search-tree property allows us to print out all the keys of a binary search tree in sorted order by using an algorithm called ***inorder traversal***. The algorithm has this name because it prints the root of a subtree between printing the values in its left subtree and printing those in its right subtree. Similarly, a ***preorder traversal*** prints the root before the values in either subtree, and a ***postorder traversal*** prints the root after the values in both subtrees. Below is the Python implementation of the inorder traversal algorithm. To use this procedure on a binary search tree $T$, we call $\text{inorder_traversal}(T.root)$.

```python
def inorder_traversal(x):
    if x is not None:
        inorder_traversal(x.left)
        print(x.key)
        inorder_traversal(x.right)
```

This algorithm goes through all nodes in a binary search tree, so intuitively it takes $\Theta(n)$ time to traverse a binary search tree with $n$ nodes.

**Proof**. Let $T(n)$ be the time taken by $\text{inorder_traversal}$ when it is called on the root of a $n$-node subtree. Because $\text{inorder_traversal}$ visits all $n$ nodes of the subtree, we have $T(n) = \Omega(n)$. The remaining part of our proof is to show that $T(n) = O(n)$.

If a subtree is empty, the algorithm only needs to do the conditional check `x is not None`, which takes constant time. Hence, $T(0) = c$ for some constant $c \gt 0$.

For $n \gt 0$, suppose that $\text{inorder_traversal}$ is called on a node $x$ whose left subtree has $k$ nodes and right subtree has $n - k - 1$ nodes. The running time of $\text{inorder_traversal}(x)$ is bounded by $ T(n) \le T(k) + T(n-k-1) + d$, where $d$ is a positive constant that reflects an upper bound on the running time of the body of $\text{inorder_traversal}(x)$, but not including the time complexity of recursive calls inside the body.

We will use the substitution method to show that $T(n) = O(n)$ by proving that $T(n) \le (c+d)n + c$.
- For $n=0$, we have $(c+d)\cdot 0 + c = c = T(0)$
- For $n \gt 0$, we have

$$
\begin{align}
T(n) &\le T(k) + T(n-k-1) + d \\
&\le (c+d)k + c + (c+d)(n-k-1) + c + d \\
&= c + (c+d)n - (c+d) + c + d \\
&= (c+d)n + c
\end{align}
$$

Hence, we have $T(n) = \Theta(n)$, meaning that we can go through all the nodes of a binary search tree in linear time. This running time is the same as that of the data structures mentioned at the beginning of the post, which is good news because there is no trade-off here.

## Querying a binary search tree

Besides scheduling a landing time, we can also design a runway reservation system that has more information about scheduled landings, such as the most recent scheduled landing time or the least recent landing time. Using a binary search tree, we can support queries: $\text{search}$, $\text{minimum}$, $\text{maximum}$, $\text{successor}$, $\text{predecessor}$. Also, we will examine these operations and discuss how to make each one run in $O(h)$ on any binary search tree of height $h$.

### Searching

We use the $\text{search}$ algorithm to search for a node with a key $k$ in a binary search tree. Parameters to this function are a pointer to the root of the tree and a key $k$. The function returns a pointer to a node with a key $k$ if one exists; otherwise, it returns $\text{None}$.

```python
def recursive_search(x, k):
    if x is None or k == x.key:
        return x
    elif k < x.key:
        return recursive_search(x.left, k)
    else:
        return recursive_search(x.right, k)
```

The procedure begins its search at the root and traces a simple path downward in the tree. For each node $x$ it encounters, it compares key $k$ with $x.key$. If the two keys are equal, the search terminates and returns a pointer to node $x$ where $x.key = k$. If $k$ is smaller than $x.key$, the search continues on the left subtree of $x$ because the binary-search-tree property proves that $k$ could not be stored in the right subtree. Symmetrically, if $k$ is larger than $x.key$, the search continues on the right subtree. In the worst case, we go from the root to a leaf of the tree and may or may not find a node with key $k$. Hence, the running time of the searching procedure is $O(h)$, where $h$ is the height of the tree.

This procedure can be implemented iteratively by using a **while** loop. On most computers, the iterative version is more efficient because computers do not need to allocate additional memory for the call stack.

```python
def iterative_search(x, k):
    while x is not None and k != x.key:
        if k < x.key:
            x = x.left
        else:
            x = x.right
    return x
```

Below is the image of the path that the algorithm goes through to find a key.

|![Search for a node in BST]({{ site.url }}{{ site.baseurl }}/assets/images/bst/bst_search.png)|
|:---:|
|To search for the key $45$ in the tree, we follow the path $51 \rightarrow 43 \rightarrow 46 \rightarrow 45$ from the root|

### Minimum and maximum





References
---
[^1]: Srini Devadas, S. 6.006 Introduction to Algorithms: Lecture 3, Fall 2011.
[^2]: Thomas H. Cormen, Charles E. Leiserson, Ronald L. Rivest, Clifford Stein: Introduction to Algorithms, 3rd Edition. MIT Press 2009, ISBN 978-0-262-03384-8, pp. 151-164.