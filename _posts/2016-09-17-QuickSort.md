---
layout: post
title: QuickSort
description: "QuickSort"
headline: "Let's Fire up the Engines"
categories: algorithms
tags: 
  - algorithms
comments: true
mathjax: null
featured: true
published: true
---
# Sorting Algorithms - Part 2
In this second part I am going to look at anothor recursive sorting algorithm: 
`QuickSort`.

## QuickSort
The basic recursive structure of the algoritm is simpler than the merge sort:
```
void QuickSort(int Array[], int left, int right) {
    if (left < right) {
        int pivot = Partition(Array, left, right);
        QuickSort(Array, left, pivot - 1);
        QuickSort(Array, pivot + 1, right);
    }
}
```
The `Merge` function is not needed because all the comparison is made by the 
`Partition` call which is the core of the algorithm. The recursion work the same
as the merge sort: calling the recursive sort until the reach of left = right.
Only, this time, the `pivot` value returned by the `Partition` is not necessary 
the center of the array but, in the worst case, can be one edge or another of the
interval of the recursion.

Let's see an implementation of `Partition`:
```
int Partition(int Array[], int left, int right) {
    Swap(left, (left + right) / 2, Array);
    int pivot = Array[left];
    int pivotpos = left;
    for (int i = left + 1; i <= right; i++) {
        if (Array[i] < pivot)
            Swap(++pivotpos, i, Array);
    }
    Swap(left, pivotpos, Array);
    return pivotpos;
}
```
The logic behind this block of code is to return a position of the array such that
all the elements in the left are smaller and all the elements in the right are
greater or equal of the value marked by this position.

The first `Swap(i, j, A)` (a generic function that switch `A[i]` with `A[j]`)
take the central element of the array and bring it in position 0. This value
is saved as `pivot` and its position as `pivotpos`. After the initial setting 
begins a for loop from the second element (`left + 1`) to the end.
Now, if the current value is smaller than the pivot, its position is in the
wrong part of the array and must be swapped with the element right after the
pivot `++pivotpos`. This element is for sure greater or equal of the pivot 
as already examined. The final `Swap` "restore" the pivot in the position find
by the loop, and then return it.

After the first call the array has a certain number of values less than the pivot,
the pivot itself, and a certain number of values greater or equal the pivot.
At this point the recursion call `QuickSort` both the less part and the 
greater/equal part until the reach of left = right.

## Performance
The best performance of the quick sort are equal to the performance of the merge
sort. When the pivot returned is always in the middle of the array, the call tree
is a balanced binary tree like the merge one. So the space complexity is
_O(*log*<sub>*<sub>2</sub>*</sub>n)_ and the time complexity is
_O(*n*⋅ *log*<sub>*<sub>2</sub>*</sub>n)_.

`QuickSort` can have much worst performance in the case of an unbalanced pivot.
Let assume that the `Partition` return always a pivot at the extreme of the array.
In this case only one side of the branch will be ordered forcing the `Partition`
to handle `n - 1` element per call. In this situation the space complexity will
be _O(*n*)_; consequantly the time complexity will be 
_O(*n*<sup>*2*</sup>)_.