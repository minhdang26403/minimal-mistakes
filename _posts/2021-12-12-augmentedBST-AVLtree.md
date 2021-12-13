---
title: "Augmented Binary Search Trees, AVL Trees"
categories: Trees
toc: true
toc_label: ""
toc_sticky: true
---

# Introduction

This post will continue from where [Runway reservation system and Binary Search Trees](https://minhdang26403.github.io/trees/binary-search-tree/) left off in designing a binary search tree that supports insertion and deletion in $O(\lg n)$ time. We first investigate how to augment a binary search tree and what information we can get from an augmented binary search tree. Then, we discuss operations to keep the tree balanced, which will allow us to insert or delete an element in $O(\lg n)$ time.

# Augmenting data structures

Some engineering situations require no more than a "textbook" data structure, such as a linked list, a binary search tree, and a hash table. However, many others require us to augment a textbook data structure by storing additional information in it. We rarely need to create an entirely new type of data structure. However, augmenting a data structure is not always straightforward because we have to design operations on it in a way such that they maintain the property of that data structure.

Suppose there is a new requirement for the runway reservation system: How many planes are scheduled to land at times $\le t?$. This new requirement necessitates a design amendment - storing the rank of a node in a binary search tree. We define the $i$th rank element in a binary search tree as the node in the tree with $i$th smallest key.

To compute the rank of a node, we will add an attribute $x.size$ to every node $x$ of the binary search tree. This attribute contains the number of nodes in the subtree rooted at $x$ (including $x$ itself), and we call it the size of the subtree. Hence, we have the following formula for calculating the subtree's size, where the size of node $\text{None}$ is $0$.

$$x.size = x.left.size + x.right.size + 1$$

|![An augmented binary search tree with node size]({{ site.url }}{{ site.baseurl }}/assets/images/avl_tree/bst_size.png)|
|:---:|
|An augmented binary search tree with node size|

Notice that the size and the rank of a node are different. The size is just a metric we use to compute the rank easier.

## Retrieving an element with a given rank

We will begin by looking at an operation that retrieves an element with a given rank. The $\text{select(x, i)}$ returns a pointer to the node containing the $i$th smallest key in the subtree rooted at $x$. To find the $i$th smallest key in an augmented tree $T$, we call $\text{select(T.root, i)}$.

```python
def select(x, i):
    r = x.left.size + 1
    if i == r:
        return x
    elif i < r:
        return select(x.left, i)
    else:
        return select(x.right, i - r)
```

Line 2 compute the rank of node $x$ within the subtree rooted a node $x$. Because $x.left.size$ gives the number of nodes that come before $x$ (according to the binary-search-tree property), $x.left.size + 1$ will give the rank of $x$ within the subtree rooted at $x$. If $i = r$, then $x$ is the $i$th smallest element, so we return $x$. If $i < r$, meaning that the $i$th smallest element resides the $x$'s left subtree, we continue to recurse on the left subtree of $x$. If $i > r$, then the $i$th smallest element resides the right subtree. Since there are $r$ elements that come before $x$'s right subtree, the $i$th smallest element in the subtree rooted at $x$ is the $(i - r)$th smallest element in the subtree rooted at $x.right$. Therefore, we call the $\text{select}$ function recursively on $x.right$.

**Analysis**: Since each recursive call to $select$ goes down one level of a tree, the worst case running time on a tree of height $h$ is $O(h)$.

## Determine the rank of an element

The previous operation gives a sense of working with an augmented data structure. Now, we will design an algorithm that allows us to answer the question: How many landing times $\le t$ are scheduled? This question basically asks us to compute the rank of a node with a key $t$ in a binary search tree.

Given a pointer to node $x$ in an augmented binary search tree $T$, the procedure $\text{rank(T, x)}$ returns the number of elements coming before $x$ (including $x$ itself) in an inorder traversal of $T$.

```python
def rank(T, x):
    r = x.left.size + 1
    y = x
    while y is not T.root:
        if y is y.parent.right:
            r = r + y.parent.left.size + 1
        y = y.parent
    return r
```

According to the binary-search-tree property, all elements in $x$'s left subtree are less than $x$, so they will come before $x$ in an inorder tree walk. Line 2 sets $r$ to be the rank of $x$ in the subtree rooted at $x$. We also create a variable pointing node $x$ for the sake of argument. If $y$ (also $x$) is the root of the entire tree, we return $r$ as the rank of $x.key$. Otherwise, we continue to go from $x$ upward to the root of the tree. If $y$ is the right child of its parent, all left child of its parent will precede $y$ in the inorder traverse walk. Hence, we add the size of the left subtree of $y$'s parent to $r$. The process ends until we reach the root of the entire tree.

**Analysis**: In the worst case, we go from the node at the bottom level up to the root, which takes $O(h)$ time, where $h$ is the height of a binary tree.

# AVL Tree

Knowing how to augment a data structure, we can now use that idea to design a runway reservation system that supports insertion and deletion in $O(\lg n)$ time. The key problem here is maintaining the binary search tree that stores landing times balanced.

## Definition

An AVL tree (named after inventors Adelson-Velsky and Landis) is a binary search tree that balances itself every time an element is inserted or deleted. In addition to the binary-search-tree property, each node of an AVL tree has the property that the heights of subtrees rooted at its left and right child differ at most $1$.

$$|\text{height(node.left)} - \text{height(node.right)}| \le 1$$

---
[^1]: Srini Devadas, S. 6.006 Introduction to Algorithms: Lecture 3, Fall 2011.
[^2]: Thomas H. Cormen, Charles E. Leiserson, Ronald L. Rivest, Clifford Stein: Introduction to Algorithms, 3rd Edition. MIT Press 2009, ISBN 978-0-262-03384-8, pp. 151-164.