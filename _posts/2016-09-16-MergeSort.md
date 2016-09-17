---
layout: post
title: MergeSort
description: "MergeSort"
headline: "Let's Fire up the Engines"
categories: algorithms
tags: 
  - algorithms
comments: true
mathjax: null
featured: true
published: false
---
# Sorting Algorithms - Part 1
The purpose of these series of articles is digging into the sorting algorithms throught 
the implementation of some of them. Here I will discuss the general idea of the recursive 
sort and the merge sort in particular.

## Recursive sort
The basics of this class of algorithms is the principle of 
[divide and conquer](https://en.wikipedia.org/wiki/Divide_and_conquer_algorithms 
"Divide and conquer"):
> A divide and conquer algorithm works by recursively breaking down a problem into two 
or more sub-problems of the same or related type, until these become simple enough to be 
solved directly. The solutions to the sub-problems are then combined to give a solution 
to the original problem.

A general pattern of this definition can be the following:
```
void RecursiveSort(int Array[], int left, int right) {
    if (left < right) {
        int middle = Partition(Array, left, right);
        RecursiveSort(Array, left, middle);
        RecursiveSort(Array, middle + 1, right);
        Merge(Array, left, middle, right);
    }
}
``` 
The code above is valid both for the `MergeSort` and the `QuickSort` with the difference 
in the "distribution of work". In the former, the `Partition` function return the middle 
of the array with no further operations. The sorting part is performed only in the 
`Merge` function. In the latter, the `Merge` call is useless because all the confronting 
work is done in the `Partition`.    

## MergeSort
Let's see in detail the merge sort algorithm.
```
void MergeSort(int Array[], int left, int right) {
    if (left < right) {
        int middle = (left + right) / 2;
        MergeSort(Array, left, middle);
        MergeSort(Array, middle + 1, right);
        Merge(Array, left, middle, right);
    }
}
```
As already said, the `Merge` function do all the comparison work. The array is splitted
in half on every recursive call until two elements remain `(left < right)`.
In the case of `left = right` we have a single element left that is, by definition, 
alredy ordered. So no operation is needed.

A possible implementation for `Merge`:
```
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
Three indexes `i`, `j` and `k` are used to walk the part of the array defined 
by the parameters:
* `i` from `left` to `middle`;
* `j` from `middle + 1` to `right`;
* `k` from `left` to `right` of the support array `temp`;

The first while loop iterate in parallel through the left and right part of the array
and store the ordered values in the support array `temp`. Then, when one part or 
another reach the end, the second or the third loop begin to complete the copy of the
unfinished part of the array. The final for loop back copies the ordered values
in the target array.

## Performance
Now let's see how many calls are needed to complete a sort. At any call, the number of 
elements affected by the `Merge` halve. There is a balanced binary tree.
The space complexity is dependend by the maximum number of stack frame simultaneus
occurring. In this case is  _O(*log*<sub>*<sub>2</sub>*</sub>n)_.
The time complexity is affected by the number of comparison in the `Merge` function.
At first level it is call once and operate n (minus 1) checks. At the second level
it is called twice on *n*/2 checks each and so on. So, for every 
*log*<sub>*<sub>2</sub>*</sub>n level, there are n call for a complexity of
_O(*n*⋅ *log*<sub>*<sub>2</sub>*</sub>n)_.
