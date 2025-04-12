# Data Structures

## String
- Immutable sequence of characters.
```python
s = "hello"
print(s[::-1])  # Reverse: "olleh"
```

## List / Array
- **List** (Python): dynamic, holds mixed types.
- **Array** (e.g. C/Java): fixed size, same type.
```python
arr = [1, 2, 3]
arr.append(4)  # [1, 2, 3, 4]
```

## Trees
- Hierarchical data structure. Each node has children.
- Binary Tree: each node has up to 2 children.
```python
class Node:
    def __init__(self, val):
        self.left = None
        self.right = None
        self.val = val
```
![image](https://github.com/user-attachments/assets/7b577ff6-bb8e-492e-a666-86d3c684c754)




## Queue (FIFO)
- First-In-First-Out structure.
```python
from collections import deque
q = deque()
q.append(1)
q.popleft()
```

![image](https://github.com/user-attachments/assets/c58b4a0d-407b-40a1-a880-8088108ab659)


## Stack (LIFO)
- Last-In-First-Out structure.
```python
stack = []
stack.append(10)
stack.pop()
```

![image](https://github.com/user-attachments/assets/10a61fce-f3f5-4c64-b73b-c9dbb697703f)


##  Heap
- Binary tree with parent smaller (min-heap) or greater (max-heap) than children.
```python
import heapq
min_heap = []
heapq.heappush(min_heap, 3)
```

![image](https://github.com/user-attachments/assets/ccccd16f-cfa5-48e9-9577-373b11ff73c4)



## Hash Table / Hash Map
- Key-value store. Fast lookup.
```python
d = {"a": 1, "b": 2}
print(d["a"])  # 1
```

![image](https://github.com/user-attachments/assets/1ae394df-8cb3-4d7e-9217-fc9dfd524fe8)


