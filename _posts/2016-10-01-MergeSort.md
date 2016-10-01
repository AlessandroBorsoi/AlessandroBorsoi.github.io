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
The **Merge** and the **Quick Sort** are those treated typically in the beginning as a well suited examples of recursion 
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
The code above is valid for both the `MergeSort` and the `QuickSort` 
with the main difference in the _distribution of work_. In the former, `Partition` returns the middle of the array 
with no further operations. The sorting part is performed only by `Merge`. In the latter there is no `Merge` because
 all the work, pivoting and sorting, is done by the `Partition` function.
Of course in this example the assumption is that we want to order an array of integers.

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
single element left that is, by definition, already ordered. So no operation is needed further. Then, in the other 
cases with more than one element, `Merge` takes the portion of the array limited by the parameters and does its job. 
Indeed, at every step, the `Merge` function will handle the two half of the array (left to middle and middle +1 to 
right) that are already ordered by the previous steps. So the task will be to order all the elements left to right.

A possible implementation for `Merge` can be:

```c
void Merge(int Array[], int left, int middle, int right) {
    int i = left;
    int j = middle + 1;
    int k = left;
    int temp[right + 1];
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
* the index `k` are used to walk the temporary array `temp[]` and to store the ordered values before to copy them 
again in the original one.

In the first `while` loop, the first left side element is compared with the first right side element (`left` with 
`middle + 1`): the smallest value is put in `temp[]` and the corresponding index is incremented together with the 
index `k` until the end of one of the two half. This means that all the values remain (in the left or the right side 
of the `Array`) are greater than the ones already in the `temp` and can be inserted without additional controls by 
the second or the third `while`. The `for` loop recopy the now ordered values from `temp` to `Array`.

If, at the end of `Merge`, we print out the elements ordered with something like this:

```c
for (int i = left; i < right + 1; ++i) {
    printf("|%d|", Array[i]);
}
printf("\n");
```
with an input like this: `int Array[] = {23, 14, 32, 36, 9, 3, 40, 25, 11};` the result will be:

```
|14||23|
|14||23||32|
|9||36|
|9||14||23||32||36|
|3||40|
|11||25|
|3||11||25||40|
|3||9||11||14||23||25||32||36||40|
```
In this manner it is easy to follow the sequence of split and ordering made by the algorithm. 

### Complexity
If we would study the complexity of this algorithm, as per any recursive algorithm at all, beside the time complexity
 is important to look also at the space complexity. The latter is dependent by the maximum number of stack frame 
simultaneous occurring. At any step the number of elements affected by the `Merge` halve, so the result of this will 
be a balanced binary tree. It is possible to count the number of levels as *log*<sub>*<sub>2</sub>*</sub>*n* so the 
complexity will be *O*(*log*<sub>*<sub>2</sub>*</sub>n).

<img class="image-post" src="{{ site.url }}/images/mergesort/merge-sort-recursion-tree.png" alt="MergeSort Recursion 
Tree">

The time complexity is affected by the number of comparison in the `Merge` function. At first level it is called once 
and operate n (minus 1) checks in the worst case. At the second level it is called twice on *n*/2 checks each and so 
on. So, for every *log*<sub>*<sub>2</sub>*</sub>n level, there are n calls for a complexity of *O*
(*n*⋅*log*<sub>*<sub>2</sub>*</sub>n).
