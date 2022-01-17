---
title: Input and Output
date: "2021-10-25T23:46:37.121Z"
template: "post"
draft: false
slug: "input-output"
category: "Operating System Notes"
tags:
  - "Notes"
  - "Textbook"
  - "Operating System"

description: "Notes on I/O"
socialImage: "/media/image-2.jpg"
---
# Motivation
Without I/O, computers are useless. However, there are thousands of devices, each one slightly different. How do we standardize the interfaces? Devices can be unreliable(media failures/transmission errors/ packet loss), how do we make them predictable, fast, and reliable?

## Bus
- Common set of wires for communication among hardware devices + associated protocols to transfer data
- Allows for read, write
- Connects n devices over a single set of wires, connections, protocols O(n^2) relationships with 1 set of wires
- Cons: Only one transaction at a time

## How does the Processor Talk to the Device?
- The CPU contains a set of registers that can be read and written
- **Port Mapped I/O:** in/out instructions
- **Memory Mapped I/O:** load/store instructions

### Optimal Parameters for I/O
- **Data granularity:** Some devices provide single bytes at a time(keyboards), while other provide whole blocks (disks, networks)
- **Access Pattern:** Some devices must be accessed sequentially(tape), others can be accessed randomly (disk, cd). Some devices require continual monitoring while other generate interrupts when they need service

## Transferring Data To/From Controller
- **Programmed I/O:** Each byte transferred via processor in/out or load/store
  - Pros: Simple hardware and software
  - Cons: Consumes processor cycles proportional to data size
- **Direct Memory Access:** Give controller access to memory bus and asks it to transfer data blocks to/from memory directly
- **Blocking Interface:** When system reads data, sleep the process until the data is ready. When the system writes data, sleep the process until device is ready for data.
- **Non-blocking Interface:** Return quickly from read/write with count of bytes transferred. It may return nothing and write may write nothing.
- **Async Interface:** When reading data, take pointer to user buffer, return immediately since the kernel will fill the user buffer and notify user. When writing data, get the pointer to user buffer, return immediately. The kernel will read the data and notify the user.