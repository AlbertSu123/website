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
# Questions to ask
1. What is `0x12345678` `123456` is what? `78` is the offset in the page table?