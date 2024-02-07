# Algorithm

### 1. Sorting Algorithm
- **Bubble Sort**
![Bubble Sort](image.png)
    - Simplest sorting algorithm which **compares 2 adjacent elements to get the biggest(smallest) element on each iteration**.
    - The iteration will go for (number of elements - 1).
    - **`O(N^^2)` performance**

- **Heap Sort**
    - Sorting algorithm using **`Heap` data structure to find min(max) value one by one**.
    - could be useful when sorting the entire elements is not necessary, but **only for extracting a few biggest** (smallest) values.
    - **`O(nlogn)` performance** (assuming that it's using binary heap)