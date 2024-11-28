## Sample Implementation
```cpp
#include <iostream>

template <typename T>
class UniquePtr {
private:
    T* ptr; // Raw pointer to manage

public:
    // Constructor: takes ownership of a raw pointer
    explicit UniquePtr(T* p = nullptr) : ptr(p) {}

    // Destructor: deletes the owned pointer
    ~UniquePtr() {
        delete ptr;
    }

    // Disable copy constructor and copy assignment operator
    UniquePtr(const UniquePtr&) = delete;
    UniquePtr& operator=(const UniquePtr&) = delete;

    // Move constructor: transfers ownership from another UniquePtr
    UniquePtr(UniquePtr&& other) noexcept : ptr(other.ptr) {
        other.ptr = nullptr; // Set the other pointer to nullptr to avoid double deletion
    }

    // Move assignment operator: transfers ownership from another UniquePtr
    UniquePtr& operator=(UniquePtr&& other) noexcept {
        if (this != &other) {
            delete ptr;       // Clean up current resource
            ptr = other.ptr;  // Transfer ownership
            other.ptr = nullptr; // Set the other pointer to nullptr
        }
        return *this;
    }

    // Dereference operator
    T& operator*() const { return *ptr; }

    // Arrow operator
    T* operator->() const { return ptr; }

    // Get the raw pointer (for access without transferring ownership)
    T* get() const { return ptr; }

    // Release ownership of the pointer and return the raw pointer
    T* release() {
        T* oldPtr = ptr;
        ptr = nullptr;
        return oldPtr;
    }

    // Reset the UniquePtr to manage a new pointer
    void reset(T* p = nullptr) {
        delete ptr;
        ptr = p;
    }
};

// Test the UniquePtr
int main() {
    UniquePtr<int> ptr1(new int(10));
    std::cout << "Value: " << *ptr1 << std::endl;

    UniquePtr<int> ptr2 = std::move(ptr1); // Move ownership
    std::cout << "Moved Value: " << *ptr2 << std::endl;

    ptr2.reset(new int(20)); // Reset with a new value
    std::cout << "New Value after reset: " << *ptr2 << std::endl;

    return 0;
}

```