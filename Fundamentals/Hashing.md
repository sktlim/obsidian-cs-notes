- [[#Properties|Properties]]
- [[#Collision-handling|Collision-handling]]
	- [[#Collision-handling#Open-addressing|Open-addressing]]
	- [[#Collision-handling#Closed-addressing|Closed-addressing]]
	- [[#Dynamic Perfect Hashing|Dynamic Perfect Hashing]]

Any function to arbitrarily map data of arbitrary size to fixed size values. Values are used to index a fixed-sized table called hash table

Good hash function should:
- be very fast to compute
- minimize collisions

### Properties
- Uniformity: Map expected inputs uniformly over output range
	- Perfect hash function: Maps distinct set _S_ to a set of integers _m_ integers with no collisions
	- Can be evaluated using $\chi^2$ test   

## Collision-handling
### Open-addressing
![[Open Addressing.png]]
- Hash collision is resolved by **probing** (searching through alternative locations until record is found, or an unused array slot is found)
- Well-known probe sequences
	- **Linear probing:** interval between probes fixed (usually at 1)
	- **Quadratic Probing**: interval between probes increases quadratically (indices described by quadratic function) $$i_{new}=hash(key)+c_1\cdot i+c_2 \cdot i^2$$  
	- **Double Hashing**: Interval fixed but is computed by another hash function
- Tradeoffs
	- Linear probing has best cache performance but most sensitive to clustering
	- quadratic probing middle ground
	- double caching has poor cache performance but virtually no clustering
 

### Closed-addressing
![[Closed Addressing.png]]
- Build linked list with key-value pair for each search array index
- Collided items are chained together using a linked list, which can be traversed to access the item with a unique search key
- If keys are ordered, could be efficient to use "self organizing concepts" like self-balancing BST (worst case O(log n)) 

### Perfect Hashing 
- Use case
	- More memory intensive
	- Useful for fast queries, insertions, deletes, on large sets of elements
- Key concept
	- **Perfect Hashing**: Design hash function that maps keys to unique positions without collisions
#### Static perfect hashing
- **1st level hash table**: Map key to bucket
- **2nd level hash table**: For each bucket, store all entries without collisions
		- If collisions occurs, rebuild bucket's hash table with new random hash function until no collision
- **Properties**
	- Fixed set of keys; once hash table is built, no new keys inserted or removed
	- Guaranteed constant look up
	- Overall space complexity: O(n)
#### Dynamic perfect hashing
- If collision happens during insertion, 2nd level hash table rebuilt with new random hash function
- **Properties**
	- Allows keys to be inserted or removed
	- Amortized constant operations; although rebuilding expensive, it happens infrequently
	- Lazy deletion: Keys marked as deleted instead of immediately removing them
	- Overall space complexity: O(n)

## Common hash functions
1) Division: $$h(k) = k \mod m$$ where k is key and m is size of mod table. Can result in poor performance if m not chosen carefully
2) Multiplication: $$h(k) = floor(m * (k * A \mod 1))$$ Known to have more uniformity than division method
3) 