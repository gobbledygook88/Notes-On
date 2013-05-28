Data Structures
===============

Here are a list of common data structures, and sample pseudocode.

## Arrays
Holds data with the same data type. Indexed by non-negative integers.

### Heaps
Linearised binary tree, stored in an array, where child elements can never have a higher value than its parent. There are two main types of heaps:

- Max-Heap: The maximum value is stored in the root
- Min-Heap: The minimum value is stored in the root

Member functions include:

- `Build(array)`: Build heap from input array
- `Left(i)`: Return left child index or value of node `i`
- `Right(i)`: Return right child index or value of element `i`
- `Parent(i)`: Return index or value of node `i`
- `Insert(value)`: Insert new element into heap at the correct position
- `Remove(i)`: Remove element at position `i` and rebuild heap

### Stacks
Last In First Out (LIFO) operations.

    Push(n):
        if stack.top == maxSize then 
            return full
        else
            stack.top = stack.top + 1
            stack.A[top] = n
        end if

    Pop():
        if stack.top == 0 then
            return empty
        else
            stack.top = stack.top - 1
            return stack.A[stack.top + 1]
        end if

### Queues
First In First Out (FIFO) operations. Not usually implemented with an array. Methods below require overflow testing.

    Enqueue(n):
        A[queue.tail] = n
        queue.tail = queue.tail + 1
        if queue.tail > maxSize then
            queue.tail = 1
        endif

    Dequeue():
        if queue.head == queue.tail then
            return empty
        else 
            ret = queue.head
            queue.head = queue.head + 1
            if queue.head > maxSize then
                queue.head = 1
            end if 
            return ret
        end if

## Linked Lists
Node recursive structure. Each node contains data, and a node pointer. The end of the list is marked by `NULL`. Member functions include:

- `Pop()`: Remove the last node from the list, and return the node
- `Push(data)`: Add node to head of list
- `Get(i)`: Return data at node `i`
- `Remove(i)`: Remove node `i`
- `Insert(i,data)`: Insert new node at position `i`
- `Find(data)`: Find first instance of node with specified data

### Stacks
Last In First Out (LIFO) operations.

    Push(data):
        node *n = new node (data)
        n -> next = stack.top
        stack.top = n

    Pop():
        if stack.top === NULL then
            return empty
        else 
            node *n = stack.top
            stack.top = stack.top -> next
            data d = n -> d
            delete n
            return d
        end if

### Queues
First In First Out (FIFO) operations.
    
    Enqueue(data):
        node *n = new node(data)
        if queue.tail == NULL then
            queue.tail = queue.head = n
        else
            queue.tail -> next = n
            queue.tail = n
        end if

    Dequeue():
        if queue.head == NULL then
            return empty
        else 
            node *n = head
            data d = head -> data
            if queue.head == queue.tail then
                queue.head = queue.tail = NULL
            else 
                queue.head = queue.head -> next
            end if
            delete n
            return d
        end if

### Deque (Double-Ended Queue)
Supports insertion and removal from both ends of the queue. Each node will have two pointers, one pointing to the next node, and the other pointing to the previous node.

    InsertAfter(node,newNode):
        newNode.prev = node
        newNode.next = node.next
        if node.next == NULL then
            deque.tail = newNode
        else
            node.next.prev = newNode
        end if
        node.next = newNode

    InsertBefore(node,newNode):
        newNode.prev = node.prev
        newNode.next = node
        if node.prev == NULL then
            deque.head = newNode
        else
            node.prev.next = newNode
        end if
        node.prev = newNode

    PushFront(newNode):
        if deque.head == NULL
            deque.head = newNode
            deque.tail = newNode
            newNode.prev = NULL
            newNode.next = NULL
        else
            InsertBefore(deque.head,newNode)

    PushBack(newNode):
        if deque.tail == NULL
            PushFront(newNode)
        else
            InsertAfter(deque.tail,newNode)

    PopFront():
        node n = deque.head
        if n == NULL
            return empty
        deque.head = deque.head.next
        data d = n.data
        delete n
        return d

    PopBack():
        node n = deque.tail
        if n == NULL
            return empty
        deque.tail = deque.tail.prev
        data d = n.data
        delete n
        return d

    Front():
        return deque.head

    Back():
        return deque.tail

### Circular Linked Lists
Circular linked lists are where the final element points to the first.

### Linked Lists (with Sentinels)
Sentinel nodes are supplementary ndoes that do not contain relevant data that are used to accelerate the performance of linked list data structures.

### Skip List
Improves efficiency of insert, remove and search operations. The skip list contains a hierarchy of levels, the lowest of which is a traditional linked list, and higher levels representing short-cuts to further levels down the list.

## Priority Queues
Stores elements in order of priority or value. It supports the following operations:

- `Insert(A,k)`: Inserts a new element with value `k` in the required position
- `Maximum(A)`: Returns the value of the maximum element in `A`
- `ExtractMax(A)`: Removes and returns maximum element
- `IncreaseKey(A,i,k)`: Increases key of specific element `i` to `k`

### Priority Queue (with Max-Heap)
Uses a max-heap to improve efficiency.

    Maximum(A):
        return A[1]

    ExtractMax(A):
        if A.size < 1 then
            return empty
        else
            max = A[1]
            A[1] = A[size]
            A.size = A.size - 1
            MaxHeapify(A,1)
            return max
        end if 

    IncreaseKey(A,i,k):
        if k < A[i] then 
            return "key is less than original value"
        else 
            A[i] = k
            while A[i] > A[Parent(i)] and i > 1 do
                Swap(A[i],A[Parent(i)])
                i = Parent(i)
            end while
        end if

    Insert(A,k):
        A.size = A.size + 1
        A[A.size] = -\infty
        IncreaseKey(A,A.size,k)

## Dictionaries
Commonly known as associative arrays or sets, formed of key/value pairs as elements. Basic methons are

- `Insert`: Insert element
- `Delete`: Delete element
- `Search`: Return value of element

### Direct Mapping
Diretly access the array. Works well for small numbers of keys. Does not handle collisions at all.

    Insert(T,k):
        T[k] = k

    Delete(T,k):
        T[k] = NULL

    Search(T,k):
        return T[k]

Better to use hash tables.

## Hashing
Abstraction of array to allow any value to be used as an index (key). Problems occur when two entries have same index (collision). With a table size a power of 2, we can use a masking operation to force value into the range of a power of two (`table[ hash(key) & 0xff ]`). Most recommended hash table size is a prime value, which is required by some collision resolution methods; poor hash functions need division by prime to resemble uniform distribution.

### Hash Table (with Seperate Chaining)
Use a linked list for each entry if there is a conflict. Could order the list of get average time for insertion/search at minor cost, rather than unordered list where unsuccessful search looks through entire column. 

    Insert(T,k):
        Calculate h(k) and add to end of list

    Delete(T,k):
        Calculate h(k) and delete from list

    Search(T,k):
        Calculate h(k) and return search from list

Advantages:

- No hard size limit for table
- Performance degrades gracefully with more collisions
- Easily handle duplicate tags
- Deletion is simple and permenant
- No memory limitations (scalable)

Disadvantages:

- Rebuilding the table if resized is harder
- Using links, so might need more memory
- Maybe slower as links need to be dereferenced
- Little coherence

### Hash Table (with Open Addressing)
Make use of actual array; conflict resolution makes use of empty cells. `m` is size of hash table, usually prime.

    Insert(T,k):
        i = 0
        repeat
            j = h(k,i)
            if T[j] == NIL or T[j] == DELETED then
                T[j] = k
                return j
            end if
            i = i + 1
        until i == m
        return "table full"

    Search(T,k):
        i = 0
        repeat
            j = h(k,i)
            if T[j] == k then
                return j
            end if
            i = i + 1
        until i == m or T[j] == NIL
        return "not found"

    Delete(T,k):
        i = 0
        repeat
            j = h(k,i)
            if T[j] == k then
                T[j] = DELETED
                return j
            end if
            i = i + 1
        until i == m or T[j] == NIL
        return "not found"

Advantages:

- Can be used without memory allocation
- Coherence (depending on data size)

Disadvantages:

- May cause problems with clustering
- Memory limitations

#### Linear Probing
Leads to primary clustering. When collision is detected, increment until empty bucket is found. Could change step size, relatively prime to table size, to reduce primary clustering (or exponentially if load factor increases).

Given hash function `h'(k)`, probe `h(k,i) = (h'(k)+i) mod m` for `i = 0 to m-1`.

#### Quadratic Probing
Leads to secondary clustering. Use of non-constant step size. Only works if load factor is less than 0.5 and size of table is prime.

Use `h(k,i) = (h'(k) + ai + bi^2) mod m`.

#### Double Hashing
Avoid primary clustering without restrictions. Uses two independant hash functions. The second hash function can never return zero (else there will be an infinite loop). The table size should be prime, or a power of two, and second hash function returns an odd number.

Use `h(k,i) = (h_1(k) + ih_2(k)) mod m`, with `h_1(k) = k mod m` and `h_2(k) = 1 + (k mod m')`, where `m'=m-1`.

#### Coalesced Hashing
Use first empty bucket as collision bucket. If, when inserting, a collision occurs, place new node in collision bucket and link colliding index and collision bucket. Update collision buckets.

## Trees
Emulates a tree, with a root node, internal nodes, and leaf nodes.

### Binary Tree
Supports typical dictionary instructions: insert, delete, search, as well well as minimum, maximum, predecessor, successor. Organised as a linked data structure of nodes, with pointers to left and right children, and to parent.

For any node `x` and any other element `y`,

- if `y` is to the left of `x`, then `y.key <= x.key`
- if `y` is to the right of `x`, then `y.key >= x.key`

Methods:

    InOrderTraverse(T,x):
        if x != NULL then
            InOrderTraverse(T,x.left)
            print x.key
            InOrderTraverse(T,x.right)
        end if

    Search(T,x,key):
        if x == NULL then
            return NULL
        else if x.key == key then 
            return x
        else if key < x.key then 
            return Search(T,x.left,key)
        else 
            return Search(T,x.right,key)
        end if

    Insert(T,x):
        if T.root == NULL then
            T.root = x
        else
            z = T.root
            while x != NULL do
                y = z
                if x.key < z.key then
                    z = z.left
                else 
                    z = z.right
                end if
            end while
            x.parent = y
            if z.key < y.key then 
                y.left = x
            else 
                y.right = x
            end if
        end if

    Minimum(T,x):
        if x.left == NULL then
            return x
        else 
            return Minimum(T,x.left)
        end if

    Successor(T,x):
        if x.right != NULL then
            return Minimum(T,x.right)
        end if
        y = x.parent
        while y != NULL and x == y.right do
            x = y
            y = y.parent
        end while
        return y

    Delete(T,x):
        if x has no children
            just remove x
        if z has one child
            replace x with child
        if z has two children
            replace x with successor, y
            rest of right subtree becomes right child of y
            left subtree of x befores left subtree of y

## Spatial Data Structures
Reduce amount of computation. Good for searching in space (close to O(log n) time). Leads to dramatic speed ups for many applications.

### Grids

- Construction
    - Find which cells encompass each primitive
    - Add a reference to the primitive in the relevant cell (linked list)
    - Linear in memory
- Traversal
    - Lookup based on coordinates
        - norm_coord = (coord - grid_min) / (grid_max - grid_min)
        - index = floor(norm_coord x gridsize)

Advantages:

- Simple spatial data structure - quick to build
- Copes well with dynamic data
- Ray traversal less efficient than other structures

Disadvantages:

- Memory intensive, especially in high dimensions
- Wasted space - too many references in few voxels

### Hierarchical Grids
Subdivide grid when too many references in a voxel. 

### Quad Tree
Split 2D-space more effectively. Create a tree with four children. Construction recursively subdivides spaces. Can be stored in an array.

### Octree
Eight children per node. Good build and traversal times. Can lead to wasted space (but not much).

### Bounding Volume Hierarchy
Partition space into a tree of volumes

- Primitives as leaves
- Volumes (good splits) higher in the tree
- Volumes can overlap
- Nodes should be spatially local further down

Surface Area Heuristic: Estimate the cost of creating a sub-tree vs cost of computing with the data if it were a leaf.

### KD-Tree
Partition space via axis aligned hyperplanes. Slower build times often, but very fast traversal. Primitives cannot overlap volumes.

### Binary Space Partition

- Binary tree of arbitrary aligned split planes, calculated with PCA
- Can lead to numerical instabilities in traversal
- Slower build times, and slower traversal due to non-aligned split planes

## Concurrent Data Structures

### Concurrent Stack (with Linked Lists)

### Concurrent Stack (with Single Lock)

### Concurrent Stack (Treiber's lock-free stack)

### Concurrent FIFO Queue (with one coarse-grained lock)

### Concurrent FIFO Queue (with two locks)

### Concurrent FIFO Queue (Michael and Scott's lock-free queue)