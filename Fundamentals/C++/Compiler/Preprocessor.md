- Prior to compilation, each `.cpp` file goes through **preprocessing**
- Preprocessor does things like
	- strips out comments
	- ensure each `.cpp` file ends in new line
	- process `#include` directives (important!!)

- When preprocessor done, the output is a **translation unit**, and this is what is then compiled 

## Preprocessor directives
- instructions starting with `#`, ending with `\n`
- `#include`
- `#define`, `#ifdef`, `#ifndef`, `#endif`, `#if0`, 
- `#pragma once`
	- Avoid header file getting included multiple times
	- **One known case of failure:** 
		- If a header file is copied so it exists in multiple places in the system, and somehow both copies get included
		- Header guards will successfully de-dupe, but `pragma once` won't
	- **Not defined by C++ standard, so compiler support not guaranteed**