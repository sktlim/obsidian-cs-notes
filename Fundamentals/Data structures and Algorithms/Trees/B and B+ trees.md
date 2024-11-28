- Maintains sorted data and frequently used in Databases
## Properties
- Lookup, insert, delete in **O(log n)** average/worst case time
- Every node has at most *m* children (**B tree of order m**)
- Every node (excluding leaves and parent) has at least $\lceil m/2 \rceil$ children
- Root node always has 2 children unless leaf
- All leaves appear on same level
- non-leaf node with m children contains m-1 keys
- Creation process is bottom up
$$Max\space keys = m^h - 1$$ 
## Inserting and Deleting: 

When a new key is inserted:
- If the node where the key would go has room, it is simply added.
- If the node is full, it splits into two nodes, and the middle key(can be left or right biased) is moved up to the parent node. 
	- This process may continue up to the root. Deleting a key can cause nodes to merge, but the tree always maintains its balanced structure.


## Difference between B tree and B+ tree
- TLDR
	- All keys are present in B+ tree, unlike B tree
	- Leaf nodes will form a linked list

- **Advantages of B+ trees:**
	- Because B+ trees don't have data associated with interior nodes, more keys can fit on a page of memory. Therefore, it will require fewer cache misses in order to access data that is on a leaf node.
	- The leaf nodes of B+ trees are linked, so doing a full scan of all objects in a tree requires just one linear pass through all the leaf nodes. A B tree, on the other hand, would require a traversal of every level in the tree. This full-tree traversal will likely involve more cache misses than the linear traversal of B+ leaves.

- **Advantage of B trees:**
	- Because B trees contain data with each key, frequently accessed nodes can lie closer to the root, and therefore can be accessed more quickly.


![[B+ tree.png]]


![[Difference between B and B+ tree.png]]