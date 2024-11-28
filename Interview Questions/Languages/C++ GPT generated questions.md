### 1. **Language Basics**

#### Beginner
#flashcards/programming-languages/CPP/gpt-questions
- What are the primary differences between C++ and C
?
	- C++ builds on C by adding support for OOP concepts, function and operator overloading, STL. While C is procedural, C++ adds on OOP (and more recently FP concepts) to make programming more versatile

#flashcards/programming-languages/CPP/gpt-questions
- Explain the `const` keyword and its various uses in C++.
?
	- The `const` keyword defines a object, variable, or function parameter to be a constant which means its value cannot be altered after initialization.

#flashcards/programming-languages/CPP/gpt-questions
- How do you define and use namespaces in C++
?
	- Namespaces prevent name conflicts by grouping logically related classes, functions, and variables under a unique scope
	- namespaces are defined and used as such:
```cpp
namespace Zoo{
	class Animal{
		...
	};
	class Manager{
		...
	};
};

using Zoo::Animal; // using declaration; brings Animal into scope
Animal a;

using namespace Zoo; // using directive; brings Zoo into scope

```

#flashcards/programming-languages/CPP/gpt-questions
- What are `inline` functions, and when would you use them
?
	- Inline functions suggest to the compiler to use substitute the code within the function definition in place of each call to that function
	- This could theoretically improve performance by eliminating the overhead of function calls
	- Compiler could ignore `inline` functions in certain scenarios
		- Recursive functions
		- Functions that are referred to through a pointer
	- `inline`, `__inline`, `__forceinline` 

#flashcards/programming-languages/CPP/gpt-questions
- What is the difference between `const int*`, `int* const`, and `const int* const`?
?
**`const int*`** is a pointer to a `const int`, meaning you can't modify the `int` value. **`int* const`** is a `const` pointer to an `int`, meaning you can't change the pointer itself. **`const int* const`** is a constant pointer to a constant integer: you can't modify either the pointer or the integer.
#### Intermediate
#flashcards/programming-languages/CPP/gpt-questions
- Explain the difference between `struct` and `class`.
?
	- `struct` and `class` differ on access levels. Members and base classes in struct are public by default while they are private by default in class.
	
#flashcards/programming-languages/CPP/gpt-questions
- Describe the `mutable` keyword and where it might be useful
?
	- Its a keyword that can only be applied to a non-const, non-static, non-referenced variable of a class. It could be useful to signal to maintainers that the variable can be changed 
	- Allows a class member variable to be modified even when accessed through a `const` instance of a class 

#flashcards/programming-languages/CPP/gpt-questions
- How is function overloading different from function overriding
?
	- Function overloading is an example of compile time polymorphism while function overriding is runtime polymorphism. Function overloading occurs when for example I have two definitions of a method taking in different variables, while function overriding is when i declare a method to be `virtual` and override it in for example a derived class 

#flashcards/programming-languages/CPP/gpt-questions
- What are the implications of using preprocessor directives like `#define` in C++
?
	- Using preprocessor directives like `#define` essentially makes whatever is defined to be a macro, which is not recommended in C++ as this could lead to unexpected behavior 

#### Advanced

#flashcards/programming-languages/CPP/gpt-questions
- How does C++ handle memory management differently than C
?
	- In C, to dynamically allocate memory, one would typically use `malloc` and `free` to allocate raw memory
	- In C++, one would use `new` and `delete` to invoke constructors/ destructors which reduces memory leaks and errors

#flashcards/programming-languages/CPP/gpt-questions
- What are the advantages and disadvantages of `typedef` versus `using`
?
	- `typedef` and `using` can both create type aliases, but `using` is more flexible, especially with templates
	- `using` should generally be preferred due to its readability and compatibility with templates, making it more versatile than `typedef`.

#flashcards/programming-languages/CPP/gpt-questions
- Describe the “One Definition Rule” (ODR) in C++.
?
	- A variable/ function/ class can only be defined once. Subsequent re-definitions would result in linker errors or undefined behavior 

#flashcards/programming-languages/CPP/gpt-questions 
- Explain name mangling in C++. How does it impact linking with other languages
?
	- Name mangling encodes function names with argument types in object files to support overloading and scope. 
	- For instance, a function `void foo(int)` and `void foo(double)` might be encoded as `_Z3fooi` and `_Z3food`. 
	- Name mangling complicates linking with other languages like C, but `extern "C"` can disable mangling to ensure compatibility with C functions:

---

### 2. **Object-Oriented Programming (OOP)**

#### Beginner
#flashcards/programming-languages/CPP/gpt-questions
- What are the four pillars of OOP? Give examples for each in C++.
?
	- Inheritance
	- Polymorphism
	- Encapsulation
	- Data Abstraction

- How do constructors and destructors work in C++?
	- 
- What is inheritance? Give an example.
?
```cpp
class Animal{
public:
	int _height;
	int _weight;
	Animal(int height, int weight):_height(height), _weight(weight)
	{}
};

class Dog : public Animal{
	
}
```
- Explain the purpose of the `this` pointer.

#### Intermediate

- How does multiple inheritance work in C++? What are the potential issues?
- What is virtual inheritance, and why is it useful?
- Explain the "Rule of Three" in C++.
- Describe what a "pure virtual function" is and how it’s used in abstract classes.

#### Advanced

- What is polymorphism, and how is it implemented in C++?
- Explain how vtables and vptrs are used to implement polymorphism.
- Describe the "Rule of Five" and how it applies to modern C++.
- How does C++ handle object slicing, and how can it be avoided?

---

### 3. **Memory Management**

#### Beginner

- What is the difference between `new` and `malloc`?
- Explain the purpose of the `delete` and `delete[]` operators.
- How does automatic memory allocation work in C++?
- What is a memory leak, and how can it be prevented?

#### Intermediate

- What are smart pointers, and what types are available in C++11?
- How does `std::shared_ptr` differ from `std::unique_ptr`?
- Explain RAII (Resource Acquisition Is Initialization) and its importance in C++.
- Describe how `std::weak_ptr` helps prevent cyclic references in smart pointers.

#### Advanced

- How does `std::allocator` work in the Standard Library?
- What are placement new and placement delete operators, and when would you use them?
- Explain the concept of memory alignment and how C++ ensures it.
- Describe the use and impact of custom allocators in performance-critical applications.

---

### 4. **Templates and Generic Programming**

#### Beginner

- What is a template in C++? Give an example of a function template.
- Explain the purpose of class templates.
- What is a non-type template parameter?
- How do you specialize a template function?

#### Intermediate

- Explain template instantiation and how it affects the compilation.
- What is SFINAE (Substitution Failure Is Not An Error), and where is it useful?
- How do you perform partial specialization of class templates?
- Describe variadic templates and give an example.

#### Advanced

- What are concept-based type requirements in C++20?
- How do `std::enable_if` and SFINAE help with template metaprogramming?
- Explain constexpr if (C++17) and how it differs from regular if statements in templates.
- Describe the impact of templates on compile-time performance and executable size.

---

### 5. **Standard Template Library (STL)**

#### Beginner

- What are the main components of the STL?
- How does `std::vector` differ from `std::array`?
- Explain the purpose of `std::map` and `std::unordered_map`.
- What is a lambda expression? Provide an example.

#### Intermediate

- How does `std::sort` work, and what algorithm does it use by default?
- Describe the differences between `std::list` and `std::deque`.
- How does a hash function work in `std::unordered_map`?
- What is an iterator, and how does it work in C++?

#### Advanced

- What are allocator-aware containers, and why are they useful?
- How do you implement custom comparators in STL containers?
- Explain the significance of `std::move_iterator` and `std::reverse_iterator`.
- Describe how STL handles iterator invalidation for different container types.

---

### 6. **Concurrency**

#### Beginner

- What is a thread, and how do you create one in C++?
- Explain the role of `std::mutex`.
- What are atomic operations, and why are they important in C++?

#### Intermediate

- Describe `std::lock_guard` and `std::unique_lock` and how they differ.
- What is a race condition, and how can you avoid it?
- How does `std::future` and `std::promise` enable asynchronous programming?

#### Advanced

- What are thread pools, and how would you implement one in C++?
- Explain `std::async` and when to use it over manual thread creation.
- How do C++ memory models and `std::memory_order` affect performance and thread safety?

---

### 7. **C++11/14/17/20 Features**

#### Beginner

- What are lambda expressions, and when would you use them?
- Explain the purpose of `nullptr` in C++11.
- How does the `auto` keyword work in C++?

#### Intermediate

- Describe the use and advantages of `std::move` and `std::forward`.
- What are range-based for loops, and how do they differ from traditional loops?
- Explain the significance of `std::optional` and `std::variant`.

#### Advanced

- How do coroutines work in C++20, and where might they be useful?
- Describe the significance of `std::concepts` in C++20.
- How does the spaceship operator (`<=>`) simplify comparisons in C++20?

---

### 8. **Advanced Topics**

#### Code Optimization

- How would you approach optimizing a slow-performing C++ program?
- What are cache lines, and how can they impact C++ performance?
- Explain the “inline” keyword in terms of optimization and its potential downsides.

#### Debugging and Testing

- How do you use `gdb` or `lldb` to debug C++ applications?
- Explain how you would handle an application crash in C++.
- What tools and techniques do you use for memory leak detection?

#### Design Patterns in C++

- What is the Singleton pattern, and how would you implement it in C++?
- Describe the Observer pattern and where it could be useful in C++.
- Explain the Factory pattern and how you would use it to manage object creation.

#### Systems Programming

- How do you handle low-level file I/O in C++?
- Explain how you would write a custom memory allocator in C++.
- What are signals, and how can you handle them in a C++ application?