[](https://cs184.eecs.berkeley.edu/sp23/docs/cp-intro)
# [The C++ Programming Language](https://chenweixiang.github.io/docs/The_C++_Programming_Language_4th_Edition_Bjarne_Stroustrup.pdf)

Note: These notes are merely a collection of things that are new, unintuitive, or cool from my point of view. If you have a different background in programming languages than me, you might not find these notes very useful. For reference, here is a list of programming languages I am proficient in:
Advanced - Knows most of the language: Solidity, Python. 
Intermediate - Can work on a FAANG sized project: Java, Javascript, Go
Rudimentary - Can solve leetcode questions in: C++, C

- Types of Programming: C++ supports four types of programming - procedural(ie passing structs, types, functions around), data abstraction(using interfaces to hide implementation details), object oriented(using class hierarchies), and generic programming(designing general algorithms). C++ is not just a object oriented language, people who think that are cringe.

- C++ tries to avoid features that would occur memory or runtime overhead even when not used

- C++ was created to distribute the services of a UNIX kernel across multiprocessors and local area networks(aka multicores and clusters)
  
- you can use `auto` instead of a type when defining a variable if te type is a primitive. ie `auto i = 0;`

- `new` is equivalent to allocating memory from the heap. ie `double* vector = new double[5];`

