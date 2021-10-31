---
title: Advanced Memory Management
date: "2021-10-25T23:46:37.121Z"
template: "post"
draft: false
slug: "memory-management"
category: "Operating System Notes"
tags:
  - "Notes"
  - "Textbook"
  - "Operating System"

description: "Notes on advanced memory management"
socialImage: "/media/image-2.jpg"
---

# Zero Copy I/O

**Problem:** Operating systems commonly stream data between user level programs and physical devices. However, it takes a ton of time to do so since we move data from hardware to kernel then userspace or vice versa.

**Potential Solutions that don't work:** We could move the application into the kernel directly so the data doesn't need to be copied from kernel to userspace. (This is not secure) We could also modify syscalls to allow applications to modify the data stored in the kernel buffer. (This isn't flexible, it doesn't allows for modifications)

**Solution 1: Use the process' page table** The application aligns the user level buffer to a page boundary which is then provided to the kernel. Thus, a page-to-age copy from user to kernel space can be simulated by changing page table pointers instead of physically copying memory.

# Virtual Machines

- A way for a host OS to run a guest OS as an application process.
- They are widely used on client machines to run non-native applications, are used in data centers to allow a machine to be shared by multiple machines

## Virtual Machine Page Tables

- There are two sets of page tables, guest and host
- The host OS provides a set of page tables to constrain the execution of the guest OS which is thinking it is running on a real OS but it is actually running on virtual memory.
- The host translates the guest virtual addresses into physical addresses
- The guest OS manages page tables for the guest processes, exactly the same as if it was on real hardware. This is because the guest OS thinks it is on real hardware.

When the host OS transfers control to the guest OS, everything works as expected.

When the guest OS transfer control to the guest process, this is a privileged instruction and the hardware processor will first trap back to the host. However, we can't use the guest OS page table, since it is using virtual addresses instead of physical addresses. Nor could we use the host OS page table since the guest OS should not have permission to modify that content. Thus, we will need to use **Shadow Page Tables**.

### Shadow Page Table

A composite page table that represents both the guest and host page tables.

- To keep the shadow page table up to date, the host needs to check if any shadow page tables need to be updated before it changes a page table entry.
- To keep the shadow page table up to date, the host also sets the memory of the guest page tables as read only. Thus, the guest OS traps the host every time it makes a change to a page table entry. When trapping, the host updates both the guest page table and the corresponding shadow page table.

### Transparent Memory Compression

- **Problem:** Multiplexing multiplexors is hard. For example, a data center usually has a single physical machine running many virtual machines at the same time(ie a computer hosting multiple web sites)
- When a guest OS is running two copies of the same program or process, the host kernel has no way to share pages between the two instances.
- **Solution:** Commercial Operating systems run a scavenger in the background that looks for common pages that can be shared among multiple virtual machines. Once identified, the host kernel provides an illusion that each guest has its own copy.

![Commerical scavanger for memory](/media/10-Memory-Management/commercialscavanger.JPG)

- **Case 1: Multiple copies of the same page** 
Multiple virtual machines might share a zeroed page, it would be wasteful to create a new zeroed page for each virtual machine that needs one. The host OS instead allocates a *read-only* zero page, which traps the host kernel when modified and allocates a new zeroed physical page for the guest OS.

They scavenger runs in the background looking for zeroed pages

- **Case 2: Compression of unused pages** 
Pages can differ by small amounts, this lets the host kernel add a layer in the memory hierarchy to save space. Instead of evicting a relatively unused page, we can save its delta with regards to the original page, and the full copy the delta is from stays read only. 

If the original page is modified, the delta can be quickly recompressed or evicted as necessary.

# Fault Tolerance
All systems break, but how can we manage memory to recover application data structures after a failure, not just user file data?

## Checkpoint and Restart
- **Naive Solution:** Periodically save the program's internal data to a file.
- **General Solution:** The OS uses the virtual memory system to provide application recovery as a service
**Implementation:** 
1. Suspend each thread executing in the process and save its state. (Suspending threads is to avoid race conditions)
2. Store a copy of the contents of the application memory on disk (this is the checkpoint)
3. After a failure, resume execution by restoring the contents of memory from the checkpoint and resume each of the threads.

Use address translation to minimize the amount of time the system is stalled during a checkpoint. We can mark the application pages as copy on write to do so.

- **Process Migration:** The ability to take a running program on one system, stop the execution, then resume it on a different machine.

## Recoverable Virtual Memory
Unfortunately, checkpoint + restart is an expensive operation that we should use sparingly. Is it possible to provide the application the illusion of persistent memory, so that the contents are restored to a point not long before failure?

**Implementation:** 
1. Take a checkpoint so that some consistent version of the application data is on disk
2. Record a log of every update that the application makes to memory.
3. Once things crash, read the checkpoint and apply the correct changes from the log.

**Pros:** This is how text editors save their backups.
**Cons:** Information that is not on the current version of the file can be leaked!

**Incremental Checkpoint:** Created by stopping the program and saving a copy of any pages that have been modified since the previous checkpoint.

How much work is lost during a crash is a function of how quickly we can write an incremental checkpoint to disk.

## Deterministic Debugging
- Debugging a concurrent program is hard, we don't control the scheduler, thus depending on how threads are scheduled program runs may vary.
- Debugging an OS is even harder, the OS uses widespread concurrency but also makes it difficult to tell input from output.
- **Implementation:** Use a virtual machine abstraction to allow for debugging. The host kernel mediates the initial state, the input data from I/O devices, and precise timing of interrupts.
- For multicore systems, we also need to track the order in which threads go through critical sections in addition to the initial state, the input data from I/O devices, and precise timing of interrupts

# Security
The operating system needs to protect itself and other applications from malicious or buggy implementations.

**Virtual Machine Honeypot:** Run the suspect application in a virtual machine constructed for the purpose of running suspect code. We can delete the virtual machine if the code is suspect.

# User Level Memory Management
Operating Systems now provide users the ability to:
1. Decide where to get missing pages. (local disk, non-volatile memory, remote memory, etc.)
2. Which pages can be accessed
3. Which pages should be evicted.

# Questions
