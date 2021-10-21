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

# Questions
