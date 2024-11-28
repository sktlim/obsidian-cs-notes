- While UNIX consists of code to do things/ make system calls, Windows differ in that it is primarily event-driven (keyboard typing, mouse moving)
- Main program waits for something to happen, then calls a proc to handle it
- Microsoft defines a set of procedures: WinAPI, Win32 API, Win64 API for developers to access OS services. 
	- Win32 API is recommended as it is generally stable and compatible over most versions of Windows
	- Win32 API has thousands of API calls, with a substantial number being carried out in user space. Not all API calls are true system calls

![[Unix vs win32 system calls.png]]

- **Shared libraries** in UNIX are called **dynamically linked libraries (DLLs)** on Windows
	- Components that are loaded on demand