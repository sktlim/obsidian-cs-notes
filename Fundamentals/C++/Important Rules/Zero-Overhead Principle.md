> You don't pay for what you don't use. What you use is as efficient as what you can reasonably write by hand. 

## 2 features that don't obey this principle 
- runtime type identification (RTTI)
	- RTTI enables <u>dynamic type identification</u> at runtime through `dynamic_cast` and `typeid`
	- Enabling RTTI adds info to each polymorphic class in the form of a type id attached to each class instance, which consumes memory
	- when `dynamic_cast` is used, RTTI performs checks to determine if cast is valid, increasing runtime overhead
	
- Exceptions
	- System needs to support stack unwinding and exception handling even if exception is never thrown. 
	- Compiler generates additional code to handle stack unwinding including destructors for objects in scope when exception is thrown, increasing binary size
	- Exception handling mechanisms include tables or metadata that runtime needs to perform stack unwinding
