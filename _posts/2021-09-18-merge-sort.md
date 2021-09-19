---
title: 'Merge sort'
categories: Sorting
toc: true
toc_label: ""
toc_sticky: true
---
# Introduction

The two sorting algorithms we have seen so far, bubble sort and insertion sort, have worst-case running times of $O(n^2)$. These two algorithms are inefficient as the size of the input array becomes arbitrarily large. This post will introduce a new **comparison-based** sorting algorithm - merge sort, which can run in $O(n\,lg\,n)$ in all cases (we will use $lg\,n$ to denote $log_2\,n$ for this post and all later posts). Asymptotically, this is a tremendous improvement over our previous sorting algorithms. The graph below compares the growth of $f(n)=n^2$ and $f(n)=n\,lg\,n$

![growthOfFunction]({{ site.url }}{{ site.baseurl }}/assets/images/merge_sort/growth.png)

# Divide-and-conquer

Many useful algorithms employ a common paradigm based on recursion. This paradigm, divide-and-conquer, breaks a problem into several subproblems that are similar to the original problem but smaller in size, recursively solves the subproblems, and finally combine the solutions to the subproblems to create a solution to the original problem. 

The divide-and-conquer paradigm involves three steps: 
- **Divide** the problem into a number of subproblems that are smaller of instances of the same problem.
- **Conquer** the subproblems by solving them recursively. If they are small enough, solve the subproblems as base cases.
- **Combine** the solutions to the subproblems into the solution for the original problem.

|![DivideAndConquer]({{ site.url }}{{ site.baseurl }}/assets/images/merge_sort/divide-and-conquer.png)|
|:---:|
|A typical example of divide-and-conquer [^2]|

# Merge Sort

The **merge sort** employs the divide-and-conquer paradigm. Specifically, it operates as follows
- **Divide**: Divide the array of $n$ elements into two subarrays of $\frac{n}{2}$ elements each. 
- **Conquer**: Sort the two subarrays recursively using merge sort.
- **Combine**: Merge the two sorted subarrays.

The algorithm reaches the base case when the subarray has the size of 1, in which there is no work needed to be done because every array of size 1 is already sorted. 

The key subroutine of the merge sort algorithm is the algorithm that merges two sorted arrays into a sorted one. We merge by calling an auxiliary function $merge(A,p,q,r)$, where $A$ is an array and $p,q,$ and $r$ are indices of the array such that $p \le q \lt r$. Assuming that the subarrays $A[p..q]$ and $A[q+1..r]$ are sorted, the algorithm merges them into a single subarray that replaces the current subarray $A[p..r]$

The merging process works as follows: 
- First, we have two pointers pointing to the smallest elements of subarrays, which are already sorted. 
- We compare two first elements, then place the smaller element into the output array and move the pointer to the next element of the smaller element. We repeat this step until one input array is empty, at which time we just take the remaining input array and put them into the output array. Comparing two elements takes constant time, and we do it at most $n$ times, where $n=r-p+1$. Therefore, the merge procedure takes $\Theta(n)$.

![growthOfFunction]({{ site.url }}{{ site.baseurl }}/assets/images/merge_sort/merge.png)[^3]

## Merge implementation

When implementing merge algorithm, we can add a special value $\infty$ to avoid having to check whether each subarray is empty. Whenever the pointer of one subarray moves to $\infty$, all the values of a remaining subarray will be sequentially placed into the output array until this subarray is empty (the pointer moves to $\infty$ value).

```python
def merge(A, p, q, r):
    L, R = [], []
    
    for i in range(p, q + 1):
        L.append(A[i])
    L.append(float('inf'))

    for j in range(q + 1, r + 1):
        R.append[A[j]]
    R.append(float('inf'))

    i = j = 0
    for k in range(p, r + 1):
        if L[i] <= R[j]:
            A[k] = L[i]
            i += 1
        else:
            A[k] = R[j]
            j += 1
```

We can now use **merge** procedure as a subroutine in the merge sort algorithm. The function **merge_ sort$(A, p, r)$** sorts the elements in the subarray $A[p..r]$. If $p \lt r$, the algorithm computes an index $q$ that divides $A[p..r]$ into two subarrays: $A[p..q]$, containing $\lceil \frac{n}{2} \rceil$, and $A[q+1..r]$, containing $\lfloor \frac{n}{2} \rfloor$ elements. Otherwise, the subarray has at most one element and is therefore already sorted.

```python
def merge_sort(A, p, r):
    if p < r:
        q = (p + r) // 2
        merge_sort(A, p, q)
        merge_sort(A, q + 1, r)
        merge(A, p, q, r)
```

References
---
[^1]: Cormen, Thomas H.; Leiserson, Charles E.; Rivest, Ronald L.; Stein, Clifford (2001) [1990]. Introduction to Algorithms (2nd ed.).
[^2]: Thomas Cormen, Devin Balkcom, Khan Academy computing curriculum team. Algorithms, Merge sort.  CC-BY-NC-SA License
[^3]: Srini Devadas, S. 6.006 Introduction to Algorithms: Lecture 3, Fall 2011.