> Policy and Mechanism should be separated in any complex system

## Example
![[Policy and Mechanism Separation Example.png]]
- MMU logic encapsulated in MMU handler
- Same for page fault handler
- Policy determined by external pager, which runs as user process

- Implementation leaves the question of where page replacement algorithm is placed open
	- Cleanest to have it in external pager, but external pager does not have $R$ and $M$ bits access
		- $R$ and $M$ bits either have to be communicated to external pager, or page replacement algorithm can be handled by fault handler, which then tells external pager the necessary info to write to disk

## Pros and Cons
### Pros
- Leaves design modular and allows greater flexibility

### Cons
- Crosses user-kernel boundary multiple times
- overhead of sending messages around