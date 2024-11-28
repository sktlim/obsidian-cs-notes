- Numeric
	- `short`: 2 bytes
	- `int`: 4 bytes
	- `long`: 4/8 bytes
	- `long long`: 8 bytes
	- `float`: 4 bytes
	- `double`: 8 bytes
	- `long double`: 8/12/16 bytes
- Fixed-width integers
	- `int8_t`, `uint8_t` 1 byte; Treated like signed/unsigned char on many systems
	- `int16_t`, `uint16_t` 2 bytes
	- `int32_t`, `uint32_t` 4 bytes
	- `int64_t`, `uint64_t` 16 bytes
	- Fast variants
		- `int_fast8_t`, ...
	- Least variants
		- `int_least8_t`, ...
## Integers
- integers are signed by default
- 7 bits used to hold value, last bit holds sign
- General recommendation is to avoid `unsigned ints`

## Fixed-width integers
- Not guaranteed to be defined on all architectures
- *May* be slower on certain architectures
- C++ solution to this is `fast` and `least` variants
	- But fast might be more memory inefficient as actual size might be larger


## Best practice
- **Prefer `int` when the size of the integer doesn’t matter** (e.g. the number will always fit within the range of a 2-byte signed integer) and the variable is short-lived (e.g. destroyed at the end of the function). For example, if you’re asking the user to enter their age, or counting from 1 to 10, it doesn’t matter whether int is 16 or 32 bits (the numbers will fit either way). This will cover the vast majority of the cases you’re likely to run across.
- Prefer `std::int#_t` when storing a quantity that needs a guaranteed range.
- Prefer `std::uint#_t` when doing bit manipulation or where well-defined wrap-around behavior is required.
- Favor `double` over `float` unless no space, as lack of precision will lead to inaccuracy

Avoid the following when possible:
- `short` and `long` integers -- use a fixed-width type instead.
- Unsigned types for holding quantities.
- The 8-bit fixed-width integer types as they are often treated like chars instead of integer values
- The fast and least fixed-width types.
- Any compiler-specific fixed-width integers -- for example, Visual Studio defines `__int8`, `__int16`, etc…
- `long double` because its implementation dependent. `float` and `double` usually follows IEEE 754 

## Notes
- Scientific notations are `double`
- 