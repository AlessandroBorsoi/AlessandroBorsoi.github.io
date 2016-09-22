---
layout: post
title: MergeSort
description: "MergeSort"
headline: "Let's Sort it up"
categories: algorithms
tags: 
  - algorithms
  - recursive sort
  - merge sort
  - sorting
  - recursion
comments: true
mathjax: null
featured: true
published: true
---
### Sorting Algorithms
Some of the first topics a computer science degree dig into in the early courses are the sorting algorithms. 
The **Merge** and the **Quick Sort** are those treated typically in the beginning as a well suited examples of recursion, 
together with another classic problem easy to solve with this technique: 
the [Tower of Hanoi](https://en.wikipedia.org/wiki/Tower_of_Hanoi "Tower of Hanoi").
Let's start to look at the common characteristics of this two recursive sorting algorithms.

### Recursive sort
The basic principle of this class of algorithms is 
[divide and conquer](https://en.wikipedia.org/wiki/Divide_and_conquer_algorithms "Divide and conquer"):
> A divide and conquer algorithm works by recursively breaking down a problem into two or more sub-problems of the same 
or related type, until these become simple enough to be solved directly. The solutions to the sub-problems are then 
combined to give a solution to the original problem.

A general pattern of this definition can be translated with the following eight lines of C:
```c
void RecursiveSort(int Array[], int left, int right) {
    if (left < right) {
        int middle = Partition(Array, left, right);
        RecursiveSort(Array, left, middle);
        RecursiveSort(Array, middle + 1, right);
        Merge(Array, left, middle, right);
    }
}
``` 
The code above is valid for both the `MergeSort` and the `QuickSort` with the main difference in the _distribution of
work_. In the former, `Partition` returns the middle of the array with no further operations. The sorting part is 
performed only by `Merge`. In the latter there is no `Merge` because all the work, pivoting and sorting, is done by the 
`Partition` function.

### MergeSort
Let's see in detail the merge sort algorithm.
```c
void MergeSort(int Array[], int left, int right) {
    if (left < right) {
        int middle = (left + right) / 2;
        MergeSort(Array, left, middle);
        MergeSort(Array, middle + 1, right);
        Merge(Array, left, middle, right);
    }
}
```
As already said, the `Merge` function do all the comparison work, but before to call it, the recursion take place on
every half of the array until a two elements remain `(left < right)`. In the case of `left = right` there will be a 
single element left that is, by definition, already ordered. So no operation is needed further. Then, before returning
to the caller, `Merge` take the portion of the array limited by the parameters and does its job: ordering the elements.
The `Merge` function in the caller recursive step will have two half parts already ordered and will be able to easy 
sorting together both; and so on until the full array.

A possible implementation for `Merge` can be:
```c
void Merge(int Array[], int left, int middle, int right) {
    int i = left;
    int j = middle + 1;
    int k = left;
    int temp[];
    while (i <= middle && j <= right) {
        if (Array[i] < Array[j])
            temp[k++] = Array[i++];
        else 
            temp[k++] = Array[j++];
    }
    while (i <= middle)
        temp[k++] = Array[i++];
    while (j <= right)
        temp[k++] = Array[j++];
    for (k = left; k <= right; k++)
        Array[k] = temp[k];
}
```
* The index `i` walks the left side of the part of the array considered;
* the index `j` walks the right side of the part of the array considered;
* the index `k` are used to walk the temporary array `temp[]` and to store the ordered values before to copy them in the 
original one.

The first left side element is compared with the first right side element (at `middle + 1` position): the smallest is 
put in `temp[]` and the corresponding indexes incremented until the end of one of the two half. This means that all the 
values in the other side are greater than the ones already in the `temp` array and can be inserted without additional 
controls. The final step consists to recopy the values from `temp` to `Array`.
```
|23|14|32|36|9|3|40|25|11|

|23|14|32|36|9|<->|3|40|25|11|

|23|14|32|<->|36|9|      |3|40|<->|25|11|

|23|14|<->|32|    |36|<->|9|     |3|<->|40|   |25|<->|11|

|23|<->|14|    |32|  |36|    |9|     |3|     |40|     |25|   |11|

|23|    |14|32|    |9|36|     |3|40|     |11|25|

|23|14|32|    |9|36|     |3|40|     [11|25|
```

### Complexity
If we would study the complexity of this algorithm, as per any recursive algorithm at all, beside the time complexity
 is important to look at the space complexity. The latter is dependent by the maximum number of stack frame 
simultaneous occurring. At any step the number of elements affected by the `Merge` halve, so the result of this will 
be a balanced binary tree. It is possible to count the number of levels as *log*<sub>*<sub>2</sub>*</sub>*n* so the 
complexity will be *O*(*log*<sub>*<sub>2</sub>*</sub>n).
The time complexity is affected by the number of comparison in the `Merge` function. At first level it is call once 
and operate n (minus 1) checks. At the second level it is called twice on *n*/2 checks each and so on. So, for every 
*log*<sub>*<sub>2</sub>*</sub>n level, there are n call for a complexity of *O*(*n*⋅*log*<sub>*<sub>2</sub>*</sub>n).
