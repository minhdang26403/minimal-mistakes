---
title: "Bubble sort - Insertion sort"
categories: Sorting
toc: true
toc_label: ""
---
# Introduction

## Overview

This post will cover two simple sorting algorithms - bubble sort and insertion sort. They are simple, yet they perform poorly in real world as the number of elements needed to be sorted gets larger. Therefore, they are often used as educational tools for introductory courses to algorithms, and more importantly, as typical examples of inefficient algorithms. 

## Why is sorting necessary?

There are several obvious applications of sorting such as organizing posts by date or maintaining a telephone directory in alphabetical order. Besides that, some problems will become easier thanks to sorting, for example binary search, finding a median, and identifying statistical outliers.

# Bubble sort

## Definition

**Bubble sort** is a **comparison sort** that works by repeatedly stepping through the list, comparing each pair of adjacent items and swapping them if they are in the wrong order. If we have $n$ items, we need to iterate over the list for $n-1$ times. The algorithm is known as bubble sort because after every complete iteration, the largest element in the given array bubbles up towards the last place, just like the movement of air bubbles in the water that float to the surface and stay there. 

## Walk-through example

Consider we need to sort the array [6, 2, 5, 3, 9] in ascending order. In each step, elements that are being compared is italic.

**First pass**

- [**6**, **2**, 5, 3, 9] $ \rightarrow $ [**2**, **6**, 5, 3, 9], The algorithm compares 6 and 2, and swaps because 6 > 2
- [2, **6**, **5**, 3, 9] $ \rightarrow $ [2, **5**, **6**, 3, 9], Swap since 6 > 5
- [2, 5, **6**, **3**, 9] $ \rightarrow $ [2, 5, **3**, **6**, 9],
- [2, 5, 3, **6**, **9**] $ \rightarrow $ [2, 5, 3, **6**, **9**], No swap because these elements are in correct order (6 < 9)

**Second pass**

- [**2**, **5**, 3, 6, 9] $ \rightarrow $ [**2**, **5**, 3, 6, 9]
- [2, **5**, **3**, 6, 9] $ \rightarrow $ [2, **3**, **5**, 6, 9], Swap because 3 < 5
- [2, 3, **5**, **6**, 9] $ \rightarrow $ [2, 3, **5**, **6**, 9], No swap because these elements are in order
- [2, 3, 5, **6**, **9**] $ \rightarrow $ [2, 3, 5, **6**, **9**], 

**Third pass**

- [**2**, **3**, 5, 6, 9] $ \rightarrow $ [**2**, **3**, 5, 6, 9]
- [2, **3**, **5**, 6, 9] $ \rightarrow $ [2, **3**, **5**, 6, 9]
- [2, 3, **5**, **6**, 9] $ \rightarrow $ [2, 3, **5**, **6**, 9]
- [2, 3, 5, **6**, **9**] $ \rightarrow $ [2, 3, 5, **6**, **9**]

The array is already sorted after the second pass, but the algorithm does not know whether it is completed. In this case, as we specify *four* passes for the array of *five* elements, the algorithm needs one more pass to complete.