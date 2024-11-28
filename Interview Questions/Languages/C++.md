## OOP
#flashcards/programming-languages/CPP/OOP
What is the virtual destructor used for?

#flashcards/programming-languages/CPP/OOP
What happens if a virtual destructor is not defined in the base class? [Citadel 2024]

  
#flashcards/programming-languages/CPP/OOP
What is the sizeof() an empty class? [Citadel 2024]
?
1 byte for definition of class name
<!--SR:!2024-10-12,4,270-->

  
#flashcards/programming-languages/CPP/OOP
Explain polymorphism in C++. [Citadel 2024]
?

#flashcards/programming-languages/CPP/Basics
Differences between references and pointers
?
TL;DR: Reference is an alias of object/ variable, used to avoid copying. Modifying reference changes original object. A pointer stores the address of a variable, but it can also be initialized using `nullptr`. Pointers can also be used to avoid copying large objects.
1) Pointer can be re-bound, while references must be bound at initialization
2) Pointers have their own identity while references can be thought of as being bounded to the variable they are initialized from
3) Its possible to create a pointer to a pointer but not pointer to reference
4) Its possible to create vector of pointers but not of references due to 1 (vectors require *CopyAssignable* components)
5) References only offer 1 level of indirection because reference to references collapse [[Reference collapsing]] 
6) Pointer can be assigned `nullptr`, but reference must be bound to existing object. Reference bound to `nullptr` is `undefined` 
7) Pointers are *ContinguousIterators* and operator `++` is defined for them
8) Pointers need to be dereferenced but references can be used directly 
9) const references and rvalue references can be bound to temporaries while pointers generally cannot

#flashcards/programming-languages/CPP/Basics
Difference between memory allocation on stack vs heap
?
Variables are typically allocated on the stack while C++ objects that generally use `new` or `malloc` are allocated on the heap. Variables on the stack are deallocated when the variable is out of scope while objects on the heap require explicit deallocation preferably through RAII

#flashcards/programming-languages/CPP/Basics 
What are lvalues and rvalues?
?
lvalue is value on the left of an assignment operation, or the expression that refers to an object with defined location in memory
rvalue is value on the right of an assignment operation, or a temporary value that does not necessarily have a distinctive 

#flashcards/programming-languages/CPP/Basics 
What is std::move? 
std::move is used to cast an object to an rvalue reference and signals to the compiler that the object can be moved from

#flashcards/programming-languages/CPP/Basics 
What is `std::forward`?
?
`std::forward` is used for perfect forwarding
## Idioms

#flashcards/programming-languages/CPP/idioms 
What is RAII? 
?
Resource Acquisition is Initialization
- Binds <u>lifecycle of a resource</u> that must be acquired before use to <u>lifetime of object</u>
- Every resource requiring cleanup should be given to an object's constructor

#flashcards/programming-languages/CPP/idioms 
What is zero-overhead principle? :: You don't pay for what you don't use. What you use is as efficient as what you can reasonably write by hand.
<!--SR:!2024-10-17,1,232-->

#flashcards/programming-languages/CPP/idioms 
Rule of zero
?
- Classes that have custom <u>destructors</u>, <u>copy/move constructors</u> or <u>copy/move assignment operators</u> should deal exclusively with ownership

#flashcards/programming-languages/CPP/idioms 
Rule of 3: 
?
- If a class requires a <u>user-defined destructor</u>, a <u>user-defined constructor</u>, or a <u>user-defined copy assignment operator</u>, it almost certainly requires all 3

#flashcards/programming-languages/CPP/idioms 
Rule of 5 
?
- Because the presence of a **<u>user-defined destructor</u>, <u>copy-constructor</u>, or <u>copy assignment operator</u> **prevents implicit definition** of the <u>move constructor and move-assignment operator</u>, any class for which move semantics are preferable must declare all 5 


## Smart Pointers

#flashcards/programming-languages/CPP/SmartPointers
What kind of smart pointers exist and what are the differences between them?

#flashcards/programming-languages/CPP/SmartPointers
How does `unique_ptr` work to ensure that only one copy of it exists at any point of time?
?
`unique_ptr` is implemented by explicitly deleting the copy constructors to ensure that `unique_ptr` cannot be created from another. `unique_ptr`'s constructor takes ownership of a raw pointer to the object and because of the deleted copy constructor, only one `unique_ptr` to that object can be created

#flashcards/programming-languages/CPP/SmartPointers 
Can you copy `unique_ptr` or pass it from one object to another?
?
Cannot copy due to copy constructor being explicitly deleted. But move semantics possible whereby ownership is transferred

#flashcards/programming-languages/CPP/SmartPointers
How does `shared_ptr` work to keep track of other `shared_ptrs`?
?
Reference counting. `shared_ptr` has normally points to a control block, which itself has 2 pointers: 1 pointer to shared object and 1 pointer to a struct containing 2 reference counts: strong references(references with ownership) and weak references (references with no ownership). 

