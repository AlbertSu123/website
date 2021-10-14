---
title: Virtual Memory and Address Translation
date: "2021-10-12T23:46:37.121Z"
template: "post"
draft: false
slug: "real-time-scheduling"
category: "Operating System Notes"
tags:
  - "Notes"
  - "Operating System"
  
description: "Notes on the different types of real-time scheduling systems"
socialImage: "/media/image-2.jpg"
---

# General Address Translation

## Motivation 
We want to store addresses in a different place than the computer expects. We can then use address translation to move between our what the computer thinks is storing stuff and the actual stuff.

## Useful things we can do with Address Translation
1. **Process Isolation:** Protect the operating system kernel and other applications against buggy or malicious code. This is done by limiting memory references by applications so that some applications cannot access that memory. Also, address translation can be used by applications to construct sandboxes for third party extensions.
2. **Interprocess Communication:** 

## Segmentation
  - Translation happens on every instruction fetch, load, or store. 
  - Each virtual address space has holes, leading to efficient segmentation for sparse address spaces
  - When is it OK to address outside valid range? This is how the stack grows
  - What if not all segments fit in memory? One solution is swapping, where we save the segment of the current process to disk then switch to the next process. A better way is to only keep parts of memory we need.

## Problems with Segmentation
1. Must fit variable sized chunks into physical memory, there could be multiple gaps but no segment is small enough to fit in any of the gaps.
2. Segmentation may move processes multiple times to fit everything.
3. Limited options for swapping to disk

## Solutions to Fragmentation
Paging - Allocate physical memory in fixed size chunks where every chunk of physical memory is equivalent. The pages should be smaller than segments

**Implementation:** Use a page table per process, which resides in physical memory and contains a physical page and permission for each virtual page(valid bits, read, write, etc.)
The offset from virtual address is copied to physical address, the virtual page number is all the remaining bits.

**Usage:** Page sharing is used in the kernel region of every process which has the same page table entries. Also used when different processes run the same binaries, user level system libraries, and shared memory segments between different processes. (Can actually share objects between processes as long as the page is mapped into the same place in the address space)

## Page Table

**Page Table Context Switch:** What need to be switched on a context switch? Page table pointer and limit.  
Pros: Simple memory allocation, easy to share
Cons: Poor when address space is sparse, table is really big

**Implementation:** Page tables are maps from Virtual Addresses to Physical Addresses. A simple implementation is a very large lookup table, but can also be implemented via Trees or Hash tables

**What is in a Page Table Entry(PTE):**  
  - Pointer to the next level page table or actual page
  - Permission bits: valid bits, read-only, write-only, read-write
  - Invalid PTE can imply region of address space is actually invalid or page/directory is just somewhere else other than memory

**Using the Page Table Entry(PTE):**
1. Used in demand paging: keeps only active pages in memory, place others on disk and mark PTEs as invalid
2. Used in Copy on Write: Unix fork gives copy of parent address to child. Address spaces disconnected after child created. Implementation - make copy of parent's page tables, mark entries in both sets of page tables as read-only, page fault on write creates two copies
3. Used in Zero Fill on Demand: New data pages stay zeroed, mark PTEs as invalid, page fault gets zeroed page.

## Multi-Level Translation
**Pros:**
  - Only need to allocate 

# Paging - Caching and TLBs, Demand Paging
Problem: A single level page table can only have so many addresses, how do we get more space? 

Solution: Use a multilevel page table, where the first page table points to the second page table which points to the third page table

Pros: Only need to allocate as many page table entries as we need for applications.(Sparse address spaces are easy)
Easy memory allocations, Easy Sharing at segment or page level(needs additional reference counting)

Cons: one pointer per page, page tables need to be contiguous, two or more if more than 2 levels lookups per reference(very expensive!)

Problem: Given a virtual address, how to map to physical address?
Solution: Use a hash table aka Inverted Page Table whose size is independent of the virtual address space. The size is directly related to the amount of physical memory.

Cons: The complexity of looking things up in hardware in a hash table makes things hard. There will also be cache issues since each thing in the hash table is far from everything else. Total size of page table = number of pages used by program in physical memory

## Address Translation Comparison
TODO: Add slide 10 here

## How is Translation Accomplished? - The Memory Management Unit (MMU)
The MMU must translate virtual addresses to physical addresses for every instruction fetch, every load, and every store.

Implementation:
1. In the first level of the page table, Read PTE from memory, check that its valid, merge address
2. In the second level page table, read and check the first level, then read and check and update the PTE
3. Repeat for the n levels in the page table

Problems: This is really slow, since a single load/store will require n reads, n being the number of levels in the page table.

Solution: Caching - take advantage of the principle of locality to present the illusion of memory as the cheapest technology. 

**Temporal Locality:** Keep recently accessed data items closer to processor.
**Spatial Locality:** Move contiguous blocks to the upper levels.

### How to make Address Translation Fast?
Cache the results of recent translation(the results of the MMU). Cache the PTE extra info, physical frame number, using the virtual page number as the key

**Translation Look Aside Buffer (TLAB):** 
The name of the cache we use for translations. The TLAB records recent Virtual Page Number to Physical Frame number translation. 
  - If the Virtual Page Number is present, return the Physical Address.
  - If not, the use the MMU to find and save the result of the physical address.
     - Instruction accesses spend a lot of time on the same page since their accesses are sequential
     - Stack accesses have definite locality of reference,
  - We can have a multilevel TLAB with different size/speeds

### Sources of Cache Misses
**Compulsory:** Cold start or context switch - the first access to a block. Not much we can do here.
**Capacity:** Cache cannot contain all blocks accessed by the program. Solution - increase cache size.
**Conflict/Collision:** Multiple memory locations mapped to the same cache location. Solution - increase cache size or increase associativity.
**Coherence(Invalidation):** Other processes eg I/O updates main memory, while the copy of main memory in the cache is not updated since it was some other process that modified main memory. THIS IS VERY COMMON IN MULTI-PROCESS PROGRAMS! 
**False Sharing:** Common cause of coherence misses.

## How is a Block found in a cache?
**Block:** Minimum quantum of caching. Data Select field is used to select data within blocks. Many caching applications don't have data select fields.
**Index:** Used to Lookup Candidate in a Cache.
**Tag:** Used to identify actual copy.

**Direct Mapped Cache:** 
Cons: Conflict Misses. Since data is stored 1:1 it is likely there is already something in that slot and we will need to evict that old piece of data

**N-way set associative:** N entries per cache index.
**Fully associative cache:** Every block can hold any line, address does not include a cache index. Compare cache tags of all cache entries in parallel.

### Replacing blocks on a cache miss
For Direct Mapping, there is only one possibility.
For Set associative or Fully associative, do it randomly or evict the least recently used block

### What happens on a cache write?
**Write through:** The information is written to both the block in the cache and to the block in the lower level memory. 
Pros: read misses cannot result in writes
Cons: The processor is held up on writes unless writes are buffered.
**Write back:** The info is written only to the block in the cache. The modified cache block is written to main memory only when it is replaced.
Pros: repeat writes not sent to DRAM processor not held up on writes
Cons: More complex, read miss may require writeback of dirty data

## Physically Indexed vs Virtually Indexed Caches
  - Physically Indexed Caches (More common)
      - Address handed to cache after translation, the page table holds physical addresses
      - Pros: Every piece of data has a single place in the cache and the cache stays unchanged 

## What TLB Organization Makes Sense?
Requirements: The TLB needs to be really fast since it is in the critical path of memory access. Also, there needs to be very few conflicts, otherwise miss times will be extremely high because we need to traverse the page table.

TLB lookup is serial with cache lookup, thus. the speed of the TLB can impact the speed of access to the cache

# Discussion Notes - TLB
## Paging and Address Translation - Conceptual Questions
If the physical memory size (in bytes) is doubled, how does the number of bits in each entry of the page table change?
It doesn't?
Answer: It increases by one bit since now there are twice as many physical pages so the physical page number needs to expand by 1 bit.

If the physical memory size (in bytes) is doubled, how does the number of entries in the page table change?
Doubled
Answer: No change, the number of entries in the page table is determined by the size of the virtual address and the size of a page, it isn't affected by the size of physical memory.

If the virtual memory size (in bytes) is doubled, how does the number of bits in each entry of the page table change?
It doesn't
Answer: No change, the size of each entry of the page table has nothing to do with the virtual memory 
If the virtual memory size (in bytes) is doubled, how does the number of entries in the page map change?
It is multiplied by a factor of n, n being the number of levels?

If the page size (in bytes) is doubled, how does the number of bits in each entry of the page table change?
Doubled

If the page size (in bytes) is doubled, how does the number of entries in the page table change?
It is multiplied by a factor of n, n being the number of levels?

## Page Allocation
Suppose that you have a system with 8-bit virtual memory addresses, 8 pages of virtual memory, and 4 pages of physical memory.

How large is each page? Assume memory is byte addressed.
8 + 8 + 4

What will the page table look like if the program runs the following function? Page out the least recently used page of memory if a page needs to be allocated when physical memory is full. Assume that the stack will never exceed one page of memory?

What happens when the system runs out of physical memory? What if the program tries to access
an address that isn’t in physical memory? Describe what happens in the user program, the operating system, and the hardware in these situations.

## Address Translation
Consider a machine with a physical memory of 8 GB, a page size of 8 KB, and a page table entry size of 4 bytes. How many levels of page tables would be required to map a 46-bit virtual address space if every page table fits into a single page?

List the fields of a Page Table Entry (PTE) in your scheme.

Without a cache or TLB, how many memory operations are required to read or write a single 32-bit word?

With a TLB, how many memory operations can this be reduced to? Best-case scenario? Worst-case scenario?
# Questions to ask
1. What is `0x12345678` `123456` is what? `78` is the offset in the page table?
2. Caching in industry? What is it used for?
3. How do we know that the main memory is different from the cached memory when detecting coherence misses? <--Apparently very hard to do :( take CS152
4. Why isn't LRU implemented using a heap