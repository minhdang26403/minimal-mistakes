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

Let $n$ be the number of elements in the set $R$, or $n = \lvert R \rvert$. Our goal is to design a system that can handle requests in $O(\lg n)$ time.

## Initial attempts

We can try solving this problem with our current known data structures:

- **Sorted linked list:** Appending or prepending a landing time to the list takes constant time, but sorting takes $\Theta(n \lg n)$ time. However, we can insert a new time rather than append or prepend and sort. The $k$ minutes check and insertion take $O(1)$ time because we only need to compare values and change pointers, but finding the insertion point is $\Theta(n)$.

- **Sorted array:** It is possible to do a binary search on a sorted array to find the insertion point in $O(\lg n)$ time. Using binary search, we find the smallest index $i$ such that $R[i] \ge t$ ($R[i]$ is the next larger element of $t$). Then, we compare $R[i-1]$ and $R[i]$ against $t$ and insert $t$ if it passes the $k$ minutes constraint. However, this process takes $\Theta(n)$ because inserting involves shifting elements to the right to make room for a new element.

- **Unsorted list/array:** We can add a new landing request to the end of the list/array in $O(1)$ time. However, performing the $k$ minutes check takes $O(n)$ time because we need to scan the entire array/list.

- **Min-heap:** Inserting a new item and maintaining the min-heap property takes $O(\lg n)$ time, but checking the constraint takes $O(n)$ time.

- **Dictionary or Python Set:** Inserting takes constant time, but the $k$ minutes check takes $\Omega(n)$ time.

Our current known data structures can check the constraint fast but insert slow, or vice-versa. Therefore, we need to devise a new data structure that supports fast checking and insertion.


References
---
[^1]: Srini Devadas, S. 6.006 Introduction to Algorithms: Lecture 3, Fall 2011.
[^2]: Thomas H. Cormen, Charles E. Leiserson, Ronald L. Rivest, Clifford Stein: Introduction to Algorithms, 3rd Edition. MIT Press 2009, ISBN 978-0-262-03384-8, pp. 151-164.