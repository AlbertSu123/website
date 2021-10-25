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
- **Solution:** Commerical Operating systems run a scavenger in the background that looks for common pages that can be shared among multiple virtual machines. Once identified, the host kernel provides an illusion that each guest has its own copy. 

# Questions