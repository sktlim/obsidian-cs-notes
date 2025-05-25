> Divides a program's memory into variable-sized logical segments, such as code, data, stack. Each segment treated independently with its own base address and size

![[1d address space.png|350]]![[Segmented memory.png|350]]
## Problems Addressed
- Paging doesn't solve the issue of different growth requirements of different parts of a program (e.g. stack and heap)
- Programs consist of logically distinct components (e.g. text, data, stack, heap) and but treating one whole program's memory as contiguous doesn't follow this structure

## Properties
- Each segment consists of linear sequence of addresses starting from 0 and going up to a maximum value
- Each segment can change during execution
- Each segment can grow/ shrink independently without affecting each other
- Segments are a **logical entity**

## Issues with Segmentation
- Prone to cause **external fragmentation** (space not filled up between segments)
- ![[External Fragmentation Example.png|550]]
- **Compaction** (collecting all free space into a big pile) is used to reduce external fragmentation

## Difference between Paging and Segmentation
![[Segmentation vs Paging.png|500]]