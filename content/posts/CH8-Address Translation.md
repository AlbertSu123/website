---
title: Address Translation
date: "2021-10-12T23:46:37.121Z"
template: "post"
draft: false
slug: "address-translation"
category: "Operating System Notes"
tags:
  - "Notes"
  - "Textbook"
  - "Operating System"

description: "Notes on the different types of real-time scheduling systems"
socialImage: "/media/image-2.jpg"
---

# Motivation

- Address Translation allows for process isolation, interprocess communication(via sharing a common memory region)
- Shared code segments(ie sharing a library between programs)
- Program initialization(starting program before all code is loaded from memory to disk)
- Efficient dynamic memory allocation (only allocate memory for what is needed)
- Cache management (change how programs are positioned in physical memory to improve cache efficiency)
- Program Debugging (address translation prevents the rewriting of the code section, making things easier to debug)
- Efficient I/O (allows data to be safely transferred between user mode apps and I/O devices)
- Memory Mapped Files (map files into address space so contents of files can be referenced with program instructions)
- Virtual memory (fool the application into thinking that there is more memory than is physically present on computer)
- Checkpointing + Restarting (Can restart program from the saved state)
- Persistent Data Structures (Region of memory that survives program + system crashes)
- Process Migration (can move executing program from on server to another, ie load balancing)
- Distributed Storage (Turn network of servers into shared memory using address translation)

## Possible Places to put address translation implementation

- Specialized hardware in system
- trusted compiler, linker, or byte-code interpreter
- application translates pointers
- hybrid, with address translations in software and hardware

## Simple Address Translation

- At its core, it is a black box that translates a virtual address to a physical address
- Possible implementations could be an array, a tree, a hash table, or pretty much anything

**Goals of an address translation black box:**

1. **Memory Protection -** Limit access of a process to certain regions of memory(ie code)
2. **Memory Sharing -** Allow multiple processes to share memory (ie program code, file)
3. **Flexible Memory placement -** Put process anywhere to pack physical memory + use caches better
4. **Sparse Addresses -** Programs have multiple dynamic memory regions such as the heap for data objects, a stack for each thread, and memory mapped files
5. **Runtime lookup efficiency -** Fetching addresses should be fast
6. **Compact Translation Table -** Black box should not take much memory
7. **Portability -** Works on all types of architectures

### Symbol Table and Dynamically Linked Library(DLL)

- **Problem:** Multiprogramming requires address translation for absolute addresses, such as goto the start of the procedure or loading a global variable.
- Old Solution: Early OS used relocating loaders, after picking an empty region of physical memory, the loader would modify instructions that used an absolute addresses. The modifications were tracked in the _symbol table_.
- Current Solution: Generate the symbol table, which tracks which values need to be modified when files are assembled together by the linker. Also, commerical operating systems use a dynamically linked library, which links a library into a program when the program first calls into the library.

## Flexible Address Translation

# Questions
