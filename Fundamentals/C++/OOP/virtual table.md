> Lookup table of functions used to resolve function calls in a dynamic manner

Alternative names: vtable, virtual function table, virtual method table, dispatch table

## Notes
- <u>Every class</u> that uses virtual functions have a corresponding virtual table
- Table is <u>static array</u> that compiler sets up at compile time
- Contains one entry for each virtual function that can be called by objects of the class
	- Each <u>entry just a function pointer</u> that points to the <u>most-derived function accessible by class</u>

- Compiler also adds <u>hidden pointer</u> `*__vptr` that is a <u>member of the base class</u> 
	- Set automatically when class object is created so that `*__vptr` <u>points to the vtbl</u> for that class
	- 