---
title: "Heaps - Heapsort"
categories: Sorting
toc: true
toc_label: ""
toc_sticky: true
---

# Introduction

In this post, we will discuss a new sorting algorithm that works based on an abstract data type (ADT): a heap, and so we call this algorithm heapsort. The running time of heapsort is $O(n \lg n)$, which is asymptotically faster than bubble sort and insertion sort. Although heapsort has the same time complexity as mergesort, the main advantage of heapsort over mergesort is sorting in place. 

In previous posts, we have seen how quicksort and mergesort work based on the divide-and-conquer paradigm. This post will introduce a new algorithm design technique: using data structures to store and manipulate data.

We will start by briefly discussing the properties of heaps and basic operations on this data structure. Then, we explore the main procedures of the heapsort algorithm. Lastly, we will look at the application of heaps, which is to implement a priority queue.

# Heaps

A binary heap is often constructed as a nearly complete binary tree from an array object where each node of the binary tree represents an element of the array. We build the heap by placing each element in the array into a binary tree from left to right, level by level. By constructing the tree in this way, we can ensure that it is filled up all levels except possibly the lowest. The tree with this layout is called a nearly complete binary tree, which is shown below.

|![Heaps]({{ site.url }}{{ site.baseurl }}/assets/images/heaps-heapsort/heaps.png)|
|:---:|
|A heap built from an array [^1]|

An array $A$ representing a heap is an object with two attributes: $A.length$, which returns the number of elements in the array $A$, and $A.heap\mathrm{-}size$, which gives the number of elements of the heap stored in the array $A$. Elements in the array $A$ are not always in the heap, so $0 \le A.heap\mathrm{-}size \le A.length$. If we use the one-based indices, the root of the tree (the heap) is $A[1]$. Also, knowing the index $i$ of a node, we can easily compute the indices of its parent, left child, and right child:

$$
\begin{align}
\mathrm{parent}(i) &= \left\lfloor\frac{i}{2}\right\rfloor \\
\mathrm{leftChild}(i) &= 2i \\
\mathrm{rightChild}(i) &= 2i + 1 \\
\end{align}
$$

These operations should take constant time to compute. 

There are two types of binary heaps: max-heaps and min-heaps. Each kind of heaps has a specific ***heap property*** that every node of the heaps needs to satisfy. In a max-heap, every node, except the root node, has a value smaller than the value of its parent. Hence, the largest element in a max-heap is always stored at the root.

***Max-heap property***: $A[\mathrm{parent}(i)] \ge A[i]$

We define a min-heap in a similar way, where every node $i$ of it other than the root node satisfies:

***Min-heap property***: $A[\mathrm{parent}(i)] \le A[i]$

Based on this property, the smallest element of a min-heap is at the root of the binary heap.

In this post, we will use max-heaps for the heapsort algorithm. Viewing a heap as a tree, we define the ***height**** of a node in a heap to be the number of edges on the longest simple path from the node to a leaf and the ***height*** of the heap to be the height of its root. We can also define the ***depth*** of a node as the length of the path from the heap root to that node. Hence, the ***height*** of a heap is equal to its ***depth***. We can see that the number of nodes at depth $h$ is roughly $2^h$, which is trivially true for the root node because the depth of the root is $0$ and $2^0 = 1$. Thus, the total number of nodes in a complete binary tree is around $\sum_{i=0}^{h} 2^i = 2^{h+1} -1 $. 

Visualizing a heap as a nearly complete binary tree, we see that the number of nodes of the heap is between the number of nodes of a nearly complete binary tree depth $h-1$ with one additional node at the last level and the number of nodes of a perfect binary tree depth $h$. In other words,

$$
\begin{align}
(2^{h} - 1) + 1 &\le n \le 2^{h+1} - 1 \lt 2^{h+1} \\
\Rightarrow 2^{h} &\le n \lt 2^{h+1} \\ 
\Rightarrow h &\le \lg n \lt h + 1
\end{align}
$$

We can write the above result as $ h = \lg n + \alpha$, where $0 \le \alpha \lt 1$. Thus, 

$$h = \lfloor\lg n\rfloor = \Theta(\lg n)$$

Hence, the height of a heap built from an input array size $n$ is $\Theta(\lg n)$. This observation is essential for us to analyze the running time of operations on the heap.

# Maintaining the heap property




References
---
[^1]: Thomas H. Cormen, Charles E. Leiserson, Ronald L. Rivest, Clifford Stein: Introduction to Algorithms, 3rd Edition. MIT Press 2009, ISBN 978-0-262-03384-8, pp. 151-164.