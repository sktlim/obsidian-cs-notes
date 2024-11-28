- Self balancing BST
- For <u>lookup intensive operations</u>, AVL trees faster than [[Red-black trees]] because they are more strictly balanced
- $$balance factor = height_{left\space subtree} - height_{right\space subtree}\newline= \{-1, 0 ,1\}$$
- 
## Properties
- Height of two child subtrees of any node differ by **at most 1**. Else, tree rebalances
- Lookup, insertion, deletion: O(log n) in average and worst case

## Lookup
- Same as in BST, hence O(log n)

## Insertion
- Initially same as insertion into BST
- After insertion, check balance factor for all nodes along path of insertion to root
- If unbalanced, do rotation

## Deletion
- Similar to AVL insertion
## Rotation
- If $|balance factor| =\{-2, +2\}$ , perform rotation
- **LL-imbalance/ rotation**:
	- imbalanced subtree Z on the *left* and $balance factor(Z) \leq 0$   
- **LR-imbalance/ rotation**:
	- imbalanced subtree Z on the *left* and $balance factor(Z) > 0$ 
	- Two step rotation
- **RR-imbalance/ rotation:**
	- imbalanced subtree Z on the *right* and $balance factor(Z) \geq 0$   
- **RL-imbalance/ rotation:**
	- imbalanced subtree Z on the *right* and $balance factor(Z) < 0$ 
	- two step rotation

![[AVL Tree Rebalancing.png]]

## Set and bulk operations
TODO