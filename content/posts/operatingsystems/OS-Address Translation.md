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

![Paged Segmentation](/media/CH8-AddressTranslation/PagedSegmentation.JPG)

### Multi Level Paged Segmentation

- Use a _Global Descriptor Table_, aka a segment table.
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

## Motivation

Page tables, Segmentations, etc. all require multiple extra memory references before reaching the physical memory location. This greatly increases the overhead per instruction.

Thus, we can use a cache to improve performance. A cache stores certain data to increase the speed of future requests, usually by taking advantage of temporal locality or spatial locality.

## Translation Lookaside Buffer (TLB)

- **Problem:** Instructions are usually next to each other both in virtual memory and physical memory, it would be pointless to load instructions one at a time from physical memory since they are on the same page in physical memory.
- **TLB:** A small hardware table that contains results of a recent address translation. Each entry maps a virtual page to a physical page.

```
TLB entry = {
          virtual page number,
          physical page frame number,
          access permissions
            }
```

- On a lookup, the TLB hardware checks all of the entries against the virtual page. If there is a match, the TLB returns the physical address. (TLB hit) If none of the entries match, the hardware does the full address translation and is added to the TLB, usually replacing the least recently used entry.

![Paged Segmentation](/media/CH8-AddressTranslation/TLBhardwarepagetable.JPG)

- The cost for a TLB lookup is  
  `Cost (address translation) = Cost (TLB lookup) + Cost (full translation) × (1 - P(hit))`

## Superpages

- **Superpage:** Set of contiguous pages in physical memory that map to the same contiguous region in virtual memory. For example, two 4 Kb pages that are next to each other form an 8 Kb superpage.
- **Pros:** Reduces the number of TLB entries needed since one superpage can map many pages.
- **Cons:** Complicates the OS by requiring the system to allocate chunks of memory in different sizes.
- **Implementation:** Each entry in the TLB has a flag signifying whether the entry is a page or superpage. When matching against a superpage, the TLB ignores the offset within a superpage and only considers the most significant bits.
- **Uses:** When the computer display is being withdrawn, a normal TLB isn't big enough to hold all the entries. Large Matrices in scientific code can be sped up via superpages.

## TLB Consistency Issues

1. **Process context switch:** The virtual addresses of the old process are no longer valid. Otherwise, the new process would be reading the old process' data structures, causing it to crash or even worse, reading sensitive info such as passwords stored in memory.

We need to get rid of the TLB's copies of the old process' page translations and permission. We can flush the TLB(discard all contents), or flush the tagged TLB, where entries contain the process ID that produced each translation.

```
tagged TLB entry = {
  process ID,
  virtual page number,
  physical page frame number,
  access permissions
}
```

2. **Permission Reduction:** When the OS modifies an entry in a page table, the special purpose hardware keeps the cached data consistent. Software is needed so a single modification can affect multiple TLB entries and the OS ensures the TLB does not contain an incorrect mapping.

When adding permissions to virtual address space, the TLB can be left alone.
When reducing permissions to a page, the kernel checks if the TLB has a copy of the old page. If it does, the kernel must update permissions.

3. **TLB Shootdown:** Multiprocessors might have a cached copy of a translation in its TLB. Thus, whenever a page table entry is modified, the corresponding entry in every processor's TLB must be discarded. This requires interrupting the processor. (very expensive!)

## Virtually Addressed Caches

- Stores a copy of the contents of physical memory, indexed by the virtual address.
- On modern processors, these are split up located near the processor core, one for instruction lookups and one for data.
- Process context switch, permission reduction, and shootdown are all issues with virtually addressed caches.
- **Memory address alias:** Different processes have different virtual addresses that point to the same memory location. Each process has its own TLB entry for that memory, and the virtual cache stores a copy of the memory for each process. However, when a process modifies its own copy, how does the system know to update the other copy?

The solution is to store the physical address with the virtual address in the virtual cache.

## Physically Addressed Caches

- Consulted as a second level cache after the virtually addressed cache and the TLB but before main memory.
- Once the physical address of the memory address is located, we look at the Physically Address cache to see if there is a match. Thus we can avoid going to main memory

# Software Protection

- Naive software only protection: A machine code interpreter implemented in software simulates each instruction and only runs it if the addresses in the page table are permitted. (this is extremely slow!)

Goals of software protection:

1. **Simplify Hardware:** Is it possible to have eliminate hardware address translation to limit hardware complexity and runtime overhead from computers.
2. **Application level protection:** A browser wants to protect itself and the OS from malicious code in applications it runs.
3. **Protection inside the kernel:** Running third party code inside the kernel is dangerous, we need to protect the code in software.
4. **Portable Security:** Protection as a part of the runtime system allows us to download and run different applications without feat that the application will corrupt the OS.

- Thus, we want a _sandbox_ to execute untrusted code so that it can work without harming the rest of the system.

## Single Language OS

- Restrict all applications to be written in a single, carefully designed programming language(like solidity -> EVM?)
- For example, Javascript in modern web browsers is sandboxed. If a JavaScript program attempts to call a procedure that does not exist or reference arbitrary memory locations, the interpreter causes a runtime exception and stop the program.
- **Cons:** Using a single language based software protection relies on trusting the interpreter. Since interpreted code is slow, many interpreted systems put their functionality into system libraries that are compiled into machine code and run directly on the processor. A JavaScript program could cause a library routine to overwrite the end of a buffer for a buffer overflow attack.

Thus, many systems use both software and hardware protection.

## Language Independent Software Fault Isolation

- **Motivation:** Single Language OS requires trusting every compiler for every possible language. Is there a way we can isolate application code in a language independent fashion without using hardware?
- **Implementation:** Use a base + bound method. First, insert machine instructions into executable and check that each address is in base + bound. Second, use control + data flow analysis to remove checks that are not strictly necessary for the sandbox to be correct.

### Checking that each address is in base + bound

- For store + load instructions, just check to make sure that the addresses are within the correct region of data.
- For indirect branch instructions, make sure the program cannot branch outside of the sandbox except for predefined entry + exit points.

### Control + Data flow analysis

This allows us to eliminate extra inserted instructions if they are not needed. Possible optimizations include:

1. **Loop invariants:** If a loop strides through memory, we can use math to ensure all memory accesses in loop will be in the protected region.
2. **Return Values:** If static analysis can prove a procedure does not modify the return program counter on the stack, the return can be made safely.
3. **Cross procedure checks:** If code analysis can prove a parameter is always checked before it is passed as an argument to a subroutine, it does not need to be checked inside the subroutine.

### Sandboxes via intermediate code

Instead of creating a sandbox for x86 or ARM code, we can check a form of intermediate code and run that into the sandbox(kind of like a virtual machine)

For example, the JVM can be used to safely contain java code.

# Future

1. **Very large memory systems:** Ever increasing memories will require larger TLBs to hide the depth of the lookup trees.
2. **Multiprocessors:** TLB consistency will be more and more expensive as more processors will be added.
3. **User level sandboxes:** Each application has its own sandbox which is enforced in hardware.

# Questions
