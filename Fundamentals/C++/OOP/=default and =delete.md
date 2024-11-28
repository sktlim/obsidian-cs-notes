## =default
### Explanation
- Tells compiler to use default implementation for a function
- Mainly used for special functions like constructors, destructors, and copy/move operations

### Usage
1. Custom constructor along default
```cpp
class MyClass { 
public: 
	MyClass(int value) : x(value) {} // Custom constructor 
	MyClass() = default; // Default constructor explicitly requested 
private: 
	int x; 
};
```

2. Performance optimization
3. Rule of 5 compliance

## =delete
> "I forbid this"
### Explanation
- Explicitly prevents functions from being used 

### Usage
1. Disallow unwanted copying/ assignment 
2. Prevent implicit type conversions
4. Restrict certain overloads
	1. Explicitly deny certain operations/ conversions

```cpp
class MyClass { 
public: 
	void func(int x) {} // Accepts int 
	void func(double) = delete; // Deletes double version; fulfills usage 2 and 3 
};
```
