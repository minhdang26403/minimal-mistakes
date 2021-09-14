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

Consider we need to sort the array [6, 2, 5, 3, 9] in ascending order. In each step, elements that are being compared are written in bold.

**First pass**

- [**6**, **2**, 5, 3, 9] $ \rightarrow $ [**2**, **6**, 5, 3, 9], The algorithm compares 6 and 2, and swaps them because 6 > 2
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

The array is already sorted after the second pass, but the algorithm does not know whether it is completed. In this case, as we specify *four* passes for the array of *five* elements, the algorithm needs two more passes to complete without changing anything in the array. 

The **redundant passes** through the list motivate us to think about how to **eliminate** them. In fact, we can optimize this algorithm by adding an extra variable `swapped` inside a loop. If there occurs swapping of elements, `swapped` will be set to **true**. Otherwise, it is set to **false**

## Implementation

### Python code for Bubble Sort algorithm

```python
def bubble_sort(arr):
    n = len(arr)
    for i in range(0, n - 1):
        for j in range(0, n - i - 1): #Last i elements are already in place
            if arr[j] > arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]
```

### Optimized Bubble Sort 

```python
def bubble_sort(arr):
    n = len(arr)
    swapped = False
    for i in range(0, n - 1):
        for j in range(0, n - i - 1): #Last i elements are already in place
            if arr[j] > arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]
                swapped = True
        if swapped == False:
            return
```

## Complexity Analysis

### Performance

#### 1. Time complexity 

To determine time complexity, we need to count the number of comparisons, which dominates the number of swaps (the algorithm always compare two adjacent elements but doesn't necessarily swap them)

| Pass | Number of comparisons |
| ---- | --------------------- |
| 1st  | n - 1                 |
| 2nd  | n - 2                 |
| 3rd  | n - 3                 |
| ...  | .....                 |
| last | 1                     |

Therefore, the total number of comparisons is
$$ (n-1) + (n-2) + (n-3) + ... + 1 = \frac{n(n-1)}{2} = \mathcal{O}(n^2) $$

- **Worst case and average case**: In both cases, the algorithm needs to do $N$ iterations. In each iteration, it still does the comparison and swapping if needed. Although the optimized version performs fewer swaps compared to standard one, it still needs to do the same number of comparisons. Therefore, the **time complexity** for both algorithms: $ \mathcal{O}(n^2) $

- **Best case (the array is already sorted)**: The **time complexity** of standard algorithm is still $ \mathcal{O}(n^2) $ . However, with the optimized algorithm, after one iteration over array, it will terminate because there are not any possible swaps. Therefore, the **time complexity** of optimized version will be $ \mathcal{O}(n) $