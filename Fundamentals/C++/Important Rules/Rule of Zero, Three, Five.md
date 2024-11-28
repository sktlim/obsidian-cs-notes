Rule of zero
- Classes that have custom <u>destructors</u>, <u>copy/move constructors</u> or <u>copy/move assignment operators</u> should deal exclusively with ownership. Other classes should <u>not</u> have custom destructors, copy/move constructors or copy/move assignment operators

Rule of 3: 
- If a class requires a <u>user-defined destructor</u>, a <u>user-defined copy constructor</u>, or a <u>user-defined copy assignment operator</u>, it almost certainly requires all 3

Rule of 5 
- Because the presence of a **<u>user-defined destructor</u>, <u>copy-constructor</u>, or <u>copy assignment operator</u> prevents implicit definition** of the <u>move constructor and move-assignment operator</u>, any class for which move semantics are preferable must declare all 5 


