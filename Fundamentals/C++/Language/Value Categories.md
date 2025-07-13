![[C++ Value Categories.png]]
- **Expression:** Sequence of operators and operands

- **glvalue ("generalized" lvalue)**: Expression whose evaluation determines the identity of an object or function. Either **lvalue** or **xvalue**
- **rvalue**: Thing that appears on right hand side of assignment expression. Is an **xvalue** or a **prvalue**

- **lvalue:** Thing that appears on left hand side of assignment expression. Designates function or object
- **prvalue**: rvalue that is not an xvalue
- **xvalue (eXpiring value):** Refers to an object, usually near end of its lifetime

