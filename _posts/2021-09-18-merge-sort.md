---
title: 'Merge sort - Quick sort'
categories: Sorting
toc: true
toc_label: ""
toc_sticky: true
---
# Introduction

The two sorting algorithms we have seen so far, bubble sort and insertion sort, have worst-case running times of $O(n^2)$. These two algorithms are inefficient as the size of the input array becomes arbitrarily large. This post will introduce a new **comparison-based** sorting algorithm - merge sort, which can run in $O(n\lg n)$ in all cases (we will use $\lg n$ to denote $log_2\,n$ for this post and all later posts). Asymptotically, this is a tremendous improvement over our previous sorting algorithms. The graph below compares the growth of $f(n)=n^2$ and $f(n)=n\lg n$

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
- **Divide**: Divide the array of $n$ elements into two subarrays of $n/2$ elements each. 
- **Conquer**: Sort the two subarrays recursively using merge sort.
- **Combine**: Merge the two sorted subarrays.

The algorithm reaches the base case when the subarray has the size of 1, in which there is no work needed to be done because every array of size 1 is already sorted. 

The key subroutine of the merge sort algorithm is the algorithm that merges two sorted arrays into a sorted one. We merge by calling an auxiliary function $merge(A,p,q,r)$, where $A$ is an array and $p,q,$ and $r$ are indices of the array such that $p \le q \lt r$. Assuming that the subarrays $A[p..q]$ and $A[q+1..r]$ are sorted, the algorithm merges them into a single subarray that replaces the current subarray $A[p..r]$

The merging process works as follows: 
- First, we have two pointers pointing to the smallest elements of subarrays, which are already sorted. 
- We compare two first elements, then place the smaller element into the output array and move the pointer to the next element of the smaller element. We repeat this step until one input array is empty, at which time we just take the remaining input array and put them into the output array. Comparing two elements takes constant time, and we do it at most $n$ times, where $n=r-p+1$. Therefore, the merge procedure takes $\Theta(n)$.

![mergeProcedure]({{ site.url }}{{ site.baseurl }}/assets/images/merge_sort/merge.png)[^3]

## Merge sort implementation

When implementing the merge procedure, we can add a special value $\infty$ to avoid having to check whether each subarray is empty. Whenever the pointer of one subarray moves to $\infty$, all the values of a remaining subarray will be sequentially placed into the output array until this subarray is empty (the pointer moves to $\infty$ value).

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

We can now use the **merge** procedure as a subroutine in the merge sort algorithm. The function **merge_ sort$(A, p, r)$** sorts the elements in the subarray $A[p..r]$. If $p \lt r$, the algorithm computes an index $q$ that divides $A[p..r]$ into two subarrays: $A[p..q]$, containing $\lceil \frac{n}{2} \rceil$, and $A[q+1..r]$, containing $\lfloor \frac{n}{2} \rfloor$ elements. Otherwise, the subarray has at most one element and is therefore already sorted.

```python
def merge_sort(A, p, r):
    if p < r:
        q = (p + r) // 2
        merge_sort(A, p, q)
        merge_sort(A, q + 1, r)
        merge(A, p, q, r)
```

Now, we have a complete merge sort program. To sort the array $A = [A[0], A[1], A[n-1]]$, we call **merge_sort$(A, 0, n-1)$**, where $n$ is the length of the array. The algorithm will recursively partition the initial array in two halves until the size of them is 1. Then in the merge procedure, the algorithm involves merging pairs of 1-item arrays to form sorted arrays of length 2, merging pairs of arrays of length 2 to form sorted arrays of length 4, and so forth, until two arrays of length $n/2$ are merged to form the sorted array of length $n$.

![mergeSort]({{ site.url }}{{ site.baseurl }}/assets/images/merge_sort/merge_sort.png)[^1]

## Complexity analysis

### Analysis of divide-and-conquer algorithm

When an algorithm is recursive in its nature, we can analyze the running time of it by using a **recurrence equation**, which describes the amount of work needed to solve the problem of size $n$ in terms of that needed to solve smaller problems. We can then use mathematics to solve the recurrence for the bounds on the running time of an algorithm.

The recurrence for divide-and-conquer algorithms can be formulated from evaluating three basic steps (divide, conquer, and combine). Let $T(n)$ be the running time on a problem of size $n$. If the problem is small enough, in other words $n \le c$ for some constant $c$, there will be a straightforward solution to the problem, so the running time on it is constant, or we can write as $\Theta(1)$. Suppose that we divide our problem into $x$ subproblems, each of which is $1/y$ the size of the original problem. It takes $T(n/y)$ to solve one problem of size $n/y$, so it takes $xT(n/y)$ to solve $x$ of them. Also, we let $D(n)$ be the time to divide the problem into subproblems and $C(n)$ be the time to combine the solutions to subproblems into the solution to the initial problem. Below is our recurrence for divide-and-conquer algorithms.

$$
T(n) = 
\begin{cases}
\Theta(1) & \text{if $n \le c$,} \\
xT(n/y) + D(n) + C(n) & \text{if $n \gt c$.}
\end{cases}
$$

### Analysis of merge sort

To simplify our recurrence analysis, we assume that if $n \gt 1$, then $n$ is always a power of 2, where $n$ is the original problem size. This assumption shall not affect the solution to our recurrence. Knowing that merge sort is a divide-and-conquer algorithm, we start solving the recurrence by analyzing the running time of each part of this paradigm. 

**Divide**: The divide step computes the mid point $q$ of the array, which takes constant time. Hence, $D(n) = \Theta(1)$.

**Conquer**: We recursively sort two subarrays of $n/2$ elements each, which contributes $2T(n/2)$ to the running time. 

**Combine**: We have already reasoned that merging $n$ elements takes $\Theta(n)$, so $C(n) = \Theta(n)$.

Because the $\Theta(1)$ running time for the divide step is lower-order term when compared to the $\Theta(n)$ running time of the combine step, we can ignore the $\Theta(1)$ part and say that the divide and combine steps together takes $\Theta(n)$ time. (Constant factors will be insignificant as we consider the asymptotic running time). Therefore, we can write our recurrence as follows. 

$$
T(n) = 
\begin{cases}
\Theta(1) & \text{if $n = 1$,} \\ 
2T(n/2) + \Theta(n) & \text{if $n \gt 1$.}
\end{cases}
$$

To keep things simple, let us rewrite the recurrence as 

$$
T(n) = 
\begin{cases}
c & \text{if $n = 1$,} \\ 
2T(n/2) + cn & \text{if $n \gt 1$.}
\end{cases}
$$

In the recurrence above, we let the same constant $c$ represent both the time needed to solve the problem of size 1 and the time per element of the divide and combine steps. This does not affect the solution of our recurrence because we use big-$\Theta$ notation, which gives an upper bound and a lower bound on the running time of an algorithm. 

Without the master theorem, we can solve this recurrence intuitively by using the recurrence tree.

![RecurrenceTree]({{ site.url }}{{ site.baseurl }}/assets/images/merge_sort/recur_tree1.png)[^1]

![RecurrenceTree]({{ site.url }}{{ site.baseurl }}/assets/images/merge_sort/recur_tree2.png)[^1]

From the images above, we can see that the level $i$ of the tree has $2^i$ nodes (the root is level $0$), each taking $c(n/2^i)$ 
time to run, so that the cost of level $i$ is $2^i.c(n/2^i)=cn$. Because we start with an array of $n$ elements, there will be $n$ subarrays of size 1. In other words, the last level of the tree has $n$ nodes. To calculate the total running time of the algorithm, we just add up the cost of each level, so the merge sort algorithm takes $l.cn$ time to run, where $l$ is the number levels of the tree. 

Now we just need to compute $l$ to get the solution. Noting that the bottom level has $n$ nodes, so $2^i = n \iff i = \lg n$. We start at level $0$, so the tree has $\lg n + 1$ levels in total. 

Thus, the total cost of the algorithm is $cn(\lg n + 1) = cn\lg n + cn$. When considering the asymptotic running time, we can discard the lower-order term $cn$, giving us the result of $\Theta(n)$.

# Quicksort


References
---
[^1]: Cormen, Thomas H.; Leiserson, Charles E.; Rivest, Ronald L.; Stein, Clifford (2001) [1990]. Introduction to Algorithms (2nd ed.).
[^2]: Thomas Cormen, Devin Balkcom, Khan Academy computing curriculum team. Algorithms, Merge sort.  CC-BY-NC-SA License
[^3]: Srini Devadas, S. 6.006 Introduction to Algorithms: Lecture 3, Fall 2011.