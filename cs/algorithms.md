# Algorithms to higlight
> ref: https://medium.com/techie-delight/top-algorithms-data-structures-concepts-every-computer-science-student-should-know-e0549c67b4ac

## Binary Search Algorithm
### Binary Search Algorithm – Iterative and Recursive Implementation
references :
> https://www.techiedelight.com/binary-search/
> http://www.dspmuranchi.ac.in/pdf/Blog/Data%20Structure%20and%20Algorithms%20Binary%20Search.pdf
> 

### Overview
Basic search of an array(or list) will have a O(n) in a worst case. (* n = size of input)
If the array (or list) is sorted alreayd, Binary Search will show an advantage.

### Principle
Binary search is a fast search algorithm with run-time complexity of Ο(log n). This search algorithm works on the principle of divide and conquer.
low = mid + 1
mid = low + (high - low) / 2

### Pseudocode
```
Procedure BinarySearch
  A : Sorted Array
  n : size of array
  x : value to be searched
  
  Set minBoundary = 1
  Set maxBoundary = n
  
  while x not found
    if maxBoundary < lowBoundary
      Exit: x not existed
    
    set midPoint = minBoundary * (maxBoundary - minBoundary) / 2
    if A[midPoint] < x
      Set minBoundary = midPoint + 1
    
    if A[midPoint] > x
      Set maxBoundary = midPoint - 1
    
    if A[midPoint] = x
      Exit: x found at midPoint
  end while
end Procedure
```

## Breadth First Search (BFS) Algorithm
## Depth First Search (DFS) Algorithm
## Inorder, Preorder, Postorder Tree Traversals
## Insertion Sort, Selection Sort, Merge Sort, Quicksort, Counting Sort, Heap Sort
## Kruskal’s Algorithm
## Floyd Warshall Algorithm
## Dijkstra’s Algorithm
## Bellman Ford Algorithm
## Kadane’s Algorithm
## Lee Algorithm
## Flood Fill Algorithm
## Floyd’s Cycle Detection Algorithm
## Topological Sorting in a DAG
## Union Find Algorithm
