> Forward declaration of a class to allow usage without exposing implementation details 

## Useful example IRL
- Using PImpl to keep `<windows.h>` out of header files by forward declaring `impl` in the header file and then subsequently only including `<windows.h>` in the `.cpp` implementation file

PImpl 
- Removes implementation details of a class from its object representation by placing them in a separate class, accessed through an opaque pointer
- Uses forward-declared pointer 

```cpp
// --------------------
// interface (widget.h)
struct widget
{
    // public members
private:
    struct impl; // forward declaration of the implementation class
    // One implementation example: see below for other design options and trade-offs
    std::experimental::propagate_const< // const-forwarding pointer wrapper
        std::unique_ptr
            impl>> pImpl;               // to the forward-declared implementation class
};
 
// ---------------------------
// implementation (widget.cpp)
struct widget::impl
{
    // implementation details
};
```

## Explanation 
Because private data members of a class participate in its object representation, affecting size and layout, and because private member functions of a class participate in [overload resolution](https://en.cppreference.com/w/cpp/language/overload_resolution "cpp/language/overload resolution") (which takes place before member access checking), any change to those implementation details requires recompilation of all users of the class.

pImpl removes this compilation dependency; changes to the implementation do not cause recompilation. Consequently, if a library uses pImpl in its ABI, newer versions of the library may change the implementation while remaining ABI-compatible with older versions.