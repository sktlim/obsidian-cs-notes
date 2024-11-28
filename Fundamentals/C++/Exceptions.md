- Evaluating a `throw` expression will throw an exception


## Exception guarantees
1. `Nothrow`
	1. Function never throws exceptions. Expected of destructors and functions that may be called during stack unwinding
2. `Strong`
	1. If function throws, state of program is rollbacked to state before function call e.g. `vector::push_back`
3. `Basic`
	1. If function throws, program is in valid state. No resources leaked, all object invariants intact 
4. No guarantee
	1. If function throws, program may not be in valid state. Resource leak, memory corruption, other invariant destroying errors may have occurred