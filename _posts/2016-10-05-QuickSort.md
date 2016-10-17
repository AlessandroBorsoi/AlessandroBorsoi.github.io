---
layout: post
title: QuickSort
description: "QuickSort"
headline: "Let's Fire up the Engines"
categories: algorithms
tags: 
  - algorithms
  - recursive sort
  - quick sort
  - sorting
  - recursion
comments: true
mathjax: null
featured: true
published: true
---
### Sorting Algorithms - Part 2
In this second part (here the [first](http://alessandroborsoi.com/algorithms/MergeSort)) I am going to look at 
another recursive sorting algorithm: the **Quick Sort**.

### QuickSort
The basic recursive structure of the algorithm is simpler than the `MergeSort`:

```c
void QuickSort(int Array[], int left, int right) {
    if (left < right) {
        int pivot = Partition(Array, left, right);
        QuickSort(Array, left, pivot - 1);
        QuickSort(Array, pivot + 1, right);
    }
}
```
The first thing to notice is the absence of the `Merge` function of the `MergeSort` algorithm. The reason is 
that all the comparison work is made by `Partition` at the beginning. Like the other algorithm, the recursive call is
made until there are at least 2 elements in the `Array` (`left < right`). A single element is, as said, already 
ordered and no further actions are needed. The portion of the `Array` to order is then passed to the core function of
the algorithm (`Partition`). It made 2 things:
 
 * returns an index of the array, the `pivot`, between the left and the right limits of tha `Array` inclusive;
 * makes sure that, at the end of the call, every values on the left side of the `pivot` are less than the value of 
 the `pivot` and every values on the right side is greater or equal than the value of the `pivot`.
 
It is easy to see that, at every step of the recursion, both the left and the right side is passed to the `Partition`
 until the whole structure is sorted. The `pivot` is excluded at every step because is by definition already in the 
 right place.  

A possible implementation of the `Partition` function can be:

```c
int Partition(int Array[], int left, int right) {
    Swap(left, (left + right) / 2, Array);
    int pivotValue = Array[left];
    int pivot = left;
    for (int i = left + 1; i <= right; i++) {
        if (Array[i] < pivotValue)
            Swap(++pivot, i, Array);
    }
    Swap(left, pivot, Array);
    return pivot;
}
```
A `Swap` function is used for clarity: it performs no more than the change of position of the 2 elements passed as 
parameter.

```c
void Swap(int index1, int index2, int Array[]) {
    int tmp = Array[index1];
    Array[index1] = Array[index2];
    Array[index2] = tmp;
}
```
The first step of the `Partition` is to swap the first value on the left with the value in the center of the portion 
of the `Array` taken care of. The new element on the `left` position is now the `pivot`; its value and its position are 
saved in 2 variables: `pivotValue` and `pivot` respectively. The `for` loop now checks all the elements from `left + 
1` to `right` and compare the value of the element indexed with the `pivotValue`. If the element found is less than 
the pivot (`if (Array[i] < pivotValue)`), it means that it is in the wrong part of the `Array` so: the pivot index is
 incremented by one (to make place for the just founded value) and the value swapped. This is iterate until the end 
 of the part of the `Array` considered. At the end of the loop, the value in `left` and the value in `pivot` are 
 swapped again. The value in `left` contains the value of the `pivot`; the value in `pivot` contains a value less 
 than that of the `pivot`. From `pivot + 1` all the values remain, if exist, are greater or equal than the value of 
 `pivot`. In the end of the function all the elements left to `pivot` are less than the ...

### Complexity
The **Quick Sort** has a _best case_ and a _worst case_ based on the position of the pivot. When the pivot is always 
in the center of the structure considered, the recursive calls form a balanced binary tree exactly like the **Merge 
Sort**. So the complexity is the same.

The worst case is happening when the pivot is always on the outermost of the structure. In this way, instead of 
having a balanced tree of _log<sub><sub>2</sub></sub>n_ levels, there will be _n_ (minus 1) levels for a space 
complexity of _O(n)_. For the time complexity let's look at how many confronting the `Partition` makes at every level:
At the first level it makes _n_ operation, at the second level _n - 1_, at third _n - 2_ and so on until 1 operation 
at the last level.