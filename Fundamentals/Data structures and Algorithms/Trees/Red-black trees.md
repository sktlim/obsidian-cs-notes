![[red-black tree.png]]

## Properties
- Lookup, insertion, deletion: O(log n)

1) Every node is either <span style="color:red">red</span> or black
2) All NIL nodes are considered black
3)  Red node does not have red child
4) Every path from given node to any of its descendant NIL goes through the same number of black nodes
5) If a node N has exactly one child, it is <span style="color:red">red</span>. 

## Insertion 
- When new node Z inserted, its initially colored red, which might violate rule 2 
- There are 4 scenarios:
	1) Z == root
		- color Z black
	2) Z.uncle == <span style="color:red">red</span>
		- recolor parent, uncle and grandparents 
	3) Z.uncle == **black** and Z forms a triangle
		- LR/RL rotation and recolor 
	4) Z.uncle == **black** and Z forms a line
		- LL/RR rotation and recolor 

## Deletion
- **If the node to be deleted is red**:
    - delete.
- **If the node to be deleted is black with a red child**:
    - delete black node, recolor the red child to black.
- **If the node to be deleted is black with no children or both children are black**, the following cases apply:
    - **Case 3.1: Sibling is red**:
        - Recolor the sibling black, the parent red, and rotate around the parent.
    - **Case 3.2: Sibling is black, and both sibling’s children are black**:
        - Recolor the sibling red and move the problem up to the parent.
    - **Case 3.3: Sibling is black, and at least one sibling’s child is red**:
        - If the red child is on the far side, perform a single rotation and recolor.
        - If the red child is on the near side, perform a double rotation and recolor.