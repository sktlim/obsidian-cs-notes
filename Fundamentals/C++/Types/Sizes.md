- 1 bit represents 0 or 1
- 8 bits = 1 byte
- Smallest object = 1 byte

## Size of common C++ types
- Note: C++ standard does <u>not</u> define exact size for fundamental types
- `bool`: 1 byte
- `char`: 1 byte
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
- pointers
	- `nullptr`: 4/8 bytes

