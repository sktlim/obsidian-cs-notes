# Curiously Recurring Template Pattern
> Pattern in which class X derives from class template Y, taking a template parameter Z, where Y is instantiated with Z = X

## Explanation
- Gives the X template the ability the ability to be a base class for its specializations
- Basically, I can use base class to call derived class methods without using `virtual`

## Usage
- Simulated dynamic binding
	- By inheriting from a base class with derived as the template type, the base class can make calls to methods defined in the derived class
## Example

```cpp
template<class Z>
class Y {};

class X : public Y<X>{};
```

```cpp
template <class Derived>
class Base {
public:
	void callDerived() {
		static_cast<Derived*>(this)->overrideableFunction();
	}
};

class Derived : public Base<Derived> {
public:
	void overrideableFunction() {
		// Custom implementation
	}
};
```

## Benefits
- compile-time polymorphism
- memory efficiency
	- no v-table storage
	- no per-object v-pointer