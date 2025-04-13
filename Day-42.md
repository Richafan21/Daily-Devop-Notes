# Algorithms

## Big O

![image](https://github.com/user-attachments/assets/91b4b532-1f93-4bec-9ffe-b019d244d69a)



## Sorting Algorithms

### Bubble Sort
- Compares adjacent elements and swaps them if they are in the wrong order.
- Time Complexity: O(n²)

```python
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):
        for j in range(n - i - 1):
            if arr[j] > arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]
    return arr
```

![image](https://github.com/user-attachments/assets/29867f93-4dbb-4c4e-acdd-02ebf0c0b828)



### Insertion Sort
- Builds the sorted array one element at a time.
- Time Complexity: O(n²)

```python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        key = arr[i]
        j = i - 1
        while j >= 0 and arr[j] > key:
            arr[j+1] = arr[j]
            j -= 1
        arr[j+1] = key
    return arr
```

![image](https://github.com/user-attachments/assets/a093301a-ce74-4f25-a9d5-d1009c2a3e6e)



## Searching Algorithms

### Linear Search
- Goes through each element.
- Time Complexity: O(n)

```python
def linear_search(arr, target):
    for i in range(len(arr)):
        if arr[i] == target:
            return i
    return -1
```

### Binary Search
- Requires sorted list.
- Time Complexity: O(log n)

```python
def binary_search(arr, target):
    low, high = 0, len(arr) - 1
    while low <= high:
        mid = (low + high) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            low = mid + 1
        else:
            high = mid - 1
    return -1
```


## Recursion vs Iteration

### Recursion
- Function calls itself.

```python
def factorial_recursive(n):
    if n == 0:
        return 1
    return n * factorial_recursive(n-1)
```


### Iteration
- Uses loops instead of function calls.

```python
def factorial_iterative(n):
    result = 1
    for i in range(2, n+1):
        result *= i
    return result
```


