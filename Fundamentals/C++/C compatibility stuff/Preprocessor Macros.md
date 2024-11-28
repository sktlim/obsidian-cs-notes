## Overview
- Fragment of code given a name; Always expanded inline
- Preprocessor does not know about keywords, so it could be useful if one wishes to hide a keyword such as `const` from an older compiler
- Macros mean that the code you see is not the same as the code the compiler sees. This can introduce unexpected behavior, especially since macros have global scope.

## Usage (from Google C++ style guide)
- Avoid defining macros, especially in headers; prefer inline functions, enums, and `const` variables 
- Name macros with a project-specific prefix. 
- Do not use macros to define pieces of a C++ API.

- Don't define macros in a `.h` file.
- `#define` macros right before you use them, and `#undef` them right after.
- Do not just `#undef` an existing macro before replacing it with your own; instead, pick a name that's likely to be unique.
- Try not to use macros that expand to unbalanced C++ constructs, or at least document that behavior well.
- Prefer not using `##` to generate function/class/variable names.

## Trivia
- Macros aren't type safe