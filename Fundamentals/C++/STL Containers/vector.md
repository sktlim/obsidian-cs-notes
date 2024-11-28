## Custom implementation

```cpp
template<typename T>
class vector{
public:	
	vector():_data(nullptr), _size(0), _capacity(0);
	vector(size_t size, T& value);
	vector(const vector& copy_target); // Copy constructor
	vector& operator=(const vector& copy_target); // Copy assignment
	
	vector(vector&& move_target) noexcept;
	vector& operator=(vector&& move_target) noexcept;


private:
	T* _data;
	size_t _size;
	size_t _capacity;
};
```
