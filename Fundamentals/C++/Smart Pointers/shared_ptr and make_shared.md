- Always make a copy of an existing `std::shared_ptr` if you need more than one `std::shared_ptr` pointing to the same resource instead of constructing from resource 
- Prefer `make_shared` as its more performant

## Circular reference issue

```cpp
class B;  // Forward declaration

class A {
public:
	std::shared_ptr<B> ptrB;
	~A() { std::cout << "A destroyed\n"; }
};

class B {
public:
	std::shared_ptr<A> ptrA;
	~B() { std::cout << "B destroyed\n"; }
};

int main() {
	auto a = std::make_shared<A>();
	auto b = std::make_shared<B>();

	a->ptrB = b;
	b->ptrA = a;

	// End of main, but A and B will not be destroyed due to circular reference
	return 0;
}
```

- Can use `weak_ptr` to solve this by  changing `ptrA` to `weak_ptr` instead 
- A `std::weak_ptr` is an observer -- it can observe and access the same object as a std::shared_ptr (or other std::weak_ptrs) but it is not considered an owner
	- One downside of std::weak_ptr is that std::weak_ptr are not directly usable (they have no operator->). To use a std::weak_ptr, you must first convert it into a std::shared_ptr.