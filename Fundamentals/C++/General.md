## Trivia
- **Most vexing parse:** In certain situations, C++ grammar cannot distinguish between <u>creation of an object parameter</u> and <u>specification of the function type</u> 

```C++
// interpreted as function declaration instead of object definition
std::thread my_thread(background_task()); 

// can be avoided by doing below
std::thread my_thread((background_task())); // OK: Wrapping in parentheses
std::thread my_thread{background_task()};   // OK: Initialization
std::thread my_thread([]{do_something;});   // OK: lambda expression
```
## Concurrency
- Every C++ program has at least 1 thread started by C++ runtime: the thread running main()
- Once thread started, must explicitly decide to wait for it to finish using `join()` or leave it to run on its own by detaching it. If don't decide by time thread object is destroyed, program is terminated