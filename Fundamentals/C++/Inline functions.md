`inline`, `__inline`, `__forceinline` 
## Overview
- Inline functions suggest to the compiler to use substitute the code within the function definition in place of each call to that function
- This could theoretically improve performance by eliminating the overhead of function calls
- May lead to cache inefficiencies if improperly used

## Usage
- Best used for small functions  e.g. getter methods. Short functions are sensitive to overhead calls

## Trivia
- Compiler could ignore `inline` functions in certain scenarios
	- Recursive functions
	- Functions that are referred to through a pointer

- Note: Even `__forceinline` can't necessarily inline a function if certain conditions are present e.g. function is recursive and doesn't have **`#pragma inline_recursion(on)`** set