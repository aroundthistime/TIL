### Data Structure

### 1. Tree
- **`Tree` is a hierarchical data structure** in which a collection of elements known as **nodes are connected to each other via edges such that there exists exactly one path between any two nodes**.
- A node can be connected one, more or 0 number of children. But every node except for the `Root node` must have one parent.
- No cycles are allowed in `Tree` structure.
- Types of `Tree` structure:
    - **Binary Search Tree (BST)**
        - The **left subtree of a certain node should be smaller** than the node.
        - The **right subtree of a cetain node should be bigger** than the node.
        - Capable of **searching an item in `O(log n)`** (height of the tree)
        - Used for searching when items are entered/left often (eg. could be used to implment `set`)

    - **Self Balancing Tree**
        - A type of `Balanced Tree` which **automatically updates the structure on `insert` or `delete` operation to minimize the height** of the tree (since the height of the tree is a critical factor for the search performance).

    - **AVL Tree**
        - **`AVL Tree` is a self-balancing BST** in which the **gap between the height of left subtree and the right subtree cannot be bigger than 1** (If the nodes inside `BST` is too much focused on one side, the cost for searching an item would increase.).
        - `AVL Tree` shows balanced structure, but because of that requires heavy cost for `insert` and `delete` operations.
        - `AVG Tree` still shows `O(log n)` performance for search operation, but prevents worst cases of one-sided `BST`.

    - **Red-Black Tree**
        - **`Self Balancing BST` in which nodes take 1 extra bit containing Red or Black**.
        - The color of each node will be used to decide the structure of the tree on insert/delete operation.
            - The root of the tree is always black.
            - There are no two adjacent red nodes (A red node cannot have a red parent or red child).
            - Every path from a node (including root) to any of its descendants’ NULL nodes has the same number of black nodes.
            - All leaf (NULL) nodes are black nodes.
        - **Compared to `AVL Tree`, `Red-Black Tree` shows better performance with `insert` or `delete`** operation but the **search performance is not as effective** (since it puts less effort to balanace the tree)

    - **Segment Tree**
        - Tree for storing intervals or segments.
        - Could be used to do calculation or query based on range.
        - Takes `O(nlogn)` memory size and `O(nlogn)` time for building.
        - Takes `O(logn + k)` for query operation.
        - `Fenwick Tree`: Similar to `Segment Tree` but with more efficient memory/code size.
    
    - **Heap**
        - Tree for finding the node with highest or lowest priority.
        - In `Max heap`, the key of **parent node should be equal or greater than its child nodes**. (doesn't matter whether the child node exists between left or right side) - will be opposite for `Min Heap`
        - **`Heap` is not sorted**. It could be partially sorted but **the focus is one getting one root node**.
        - `O(1)` for getting the priority node.
        - `Heap` is a Data Structure implmenting an abstract data type called `priority queue`.
        - Could be used for **`heap sorting alghorithm` or `graph algorithms` (eg. `Djkstra`)** (and also load-balancing)
        - Common implementation will be `binary heap`, which is a `complete binary tree`. (`insert` or `delete` will take `O(logn)`)

- Possible applications:
    - Sorting items
    - Search Trees (eg. `binary search tree`)
    - AST (Abstract Syntax Tree) in programming languages
    - DOM Tree
    - Class Hierachy
    - File Systems (eg. Directory Structure)


### 2. Hash Table
- **`Hash Table` is a data structure which maps `keys` to `values`**.
- `Hash Table` uses **`hash function` which takes `key` and computes `index` of the `bucket` where the `value` is stored**.
- `Hash Table` shows **`O(1)` performance for searching and storing item**.
- The performance of `Hash Table` is determined by `hash function`, especially with regards to `collision` (when the result of `hash function` for different `keys` give out the same result)
- Possible applications:
    - Password Storage
    - Search Algorithms
    - File comparison
    - DB indexing