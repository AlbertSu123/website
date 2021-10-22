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

# Simple Address Translation

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

## Symbol Table and Dynamically Linked Library(DLL)

- **Problem:** Multiprogramming requires address translation for absolute addresses, such as goto the start of the procedure or loading a global variable.
- Old Solution: Early OS used relocating loaders, after picking an empty region of physical memory, the loader would modify instructions that used an absolute addresses. The modifications were tracked in the _symbol table_.
- Current Solution: Generate the symbol table, which tracks which values need to be modified when files are assembled together by the linker. Also, commercial operating systems use a dynamically linked library, which links a library into a program when the program first calls into the library.

# Flexible Address Translation

- The simplest model of memory management is base + bound. Each process has a base and bound to put everything between.

## Segments

- **Segmentation:** Instead of keeping only one pair of base and bound, we can instead keep an array of base and bounds.
- Segments are contiguous in memory, however, the different segments are not contiguous to each other, this would defeat the point. There will likely be gaps in the segments.
- A **segmentation fault** is a failure to a part of memory outside the legal segments.
- Segments are powerful, they are used in x86 and allows two processes to share code as their two segment tables point to the same base and bounds for their segments. They can also be used to share library routines.
- **Fork:** Makes a copy of the parent's segment table then copy the data there.
- **Zero on reference:** The operating system allocates memory for the heap but only zeroes out the first few kb of the segment. It then sets a bound register to limit the program to the zeroed part of memory. If the program wants more memory, the OS will zero out existing memory before resuming execution.
- **Cons:** There is a large overhead for managing the large number of variable size memory segments. Also, external fragmentation can occur where the small chunks between the segments have enough free space for a new application but the free space is not contiguous.
- **Questions left:** How much memory should we allocate per segment?

## Paged Segments

- **Paged Memory:** Memory that is allocated in fixed sized chunks called page frames.
- **Page Table:** Similar to segment table,its entries contain pointers to page frames. No need for a bound since all page sizes are the same.
- To share memory between processes, we set the page table entry for each process sharing a page to point to the same physical page frame using a **core map** - a data structure that tracks info about each physical page frame such as which page table entries point to it.
- **Data breakpoint:** request to stop the execution of a program when it references or modifies a memory location. It is implemented by making that location read only, so that every change can check if the instruction causing the write on read-only memory exception is the breakpoint.
- **Cons:** While physical memory management is easier, virtual address space becomes more challenging since compilers expect the stack to be contiguous(for virtual addresses) and of arbitrary size.
- **Questions:** How big should a page frame be? A larger page frame wastes space if not all memory inside frame is used. A small page frame means there are tons of page frames, meaning that the page table is very large.

# Multi Level Translation

- Instead of using an array for lookup, we can use a tree instead for the following reasons.
- **Efficient Memory Allocation:** Use a bitmap to store free space
- **Efficient Disk Transfers:** Hardware disks are partitioned into fixed size regions known as _sectors_, we can make page size a multiple of the _sector_ to simplify transfers, loading, reading, and writing.
- **Efficient Lookup:** Use a _translation lookaside buffer_ to make lookups fast
- **Efficient Reverse Lookup:** Easy to go from physical page frame to set of virtual addresses. This is crucial for the infinite virtual memory illusion.
- **Page protection and sharing granularity:** Each page table entry has its own access permissions

## Paged Segmentation

- Only two levels in the tree.
- Each segment table entry points to a page table, which points to the memory backing that segment.

![Paged Segmentation](/media/PagedSegmentation.JPG)

### Multi Level Paged Segmentation
- Use a *Global Descriptor Table*, aka a segment table. 
- For 32 bit systems, First 10 bits-page directory, Second 10 bits-second level page table, final 12 bits-offset within a page
- For 64 bit systems, only the first 48 bits are used

# Portability

## Motivation
Main memory density is increasing by almost a bit per year, for a multi-level page table to map all of memory, an extra level of the page table is needed every decade to keep up with main memory size.

For things like copy-on-write, zero-on-reference, fill-on-reference, we need a different view for hardware and virtual address space.

We also need:
- **List of memory objects:** We need to known which memory region represents program code, library code, shared data, copy-on-write, or memory-mapped file.
- **Virtual to physical translation:** In exceptions and system call parameter copying, the kernel needs to translate virtual to physical addresses to check if invalid page is truly invalid. (it could be just not yet loaded or doing some copy-on-write magic)
- **Physical to virtual translation:** The core map. Which processes map to a specific physical memory location so that when the kernel updates a page status, it also updates every page table entry that refers to that page.
- OSX uses a hash table rather than a tree for software translation data. This hash table is known as an inverted hash table 
```
inverted page table entry = {        
  process or memory object ID,        
  virtual page number,        
  physical page frame number,        
  access permissions    
}
```

# Efficient Address Translation
# Questions
