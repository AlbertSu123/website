---
title: File Systems
date: "2021-11-20T23:46:37.121Z"
template: "post"
draft: false
slug: "file-systems"
category: "Operating System Notes"
tags:
  - "Notes"
  - "Textbook"
  - "Operating System"

description: "Notes on File Systems"
socialImage: "/media/image-2.jpg"
---

# What does a storage system need?

1. **Reliability:** User data should be safely stored even if a machine's power is turned off or the OS crashes.
2. **Large Capacity and Low Cost** It takes 350 MB for an hour of music, 1 GB for 300 photos, and 4GB to store an hour long video. Many individuals own 1 TB or more of storage for personal files.
3. **High performance:** Programs must have quick access to data to satiate users(ie computer starting up, youtube serving video, amazon processing orders)
4. **Named data:** Data must be organized for future retrieval, the easiest way is to name the file.
5. **Controlled Sharing:** Users want to share stored data, but only with the right people.

## How does a file system help?

Non-volatile storage is much slower than DRAM, also, access must be done in coarse grained units, 512 bytes or more at a time.

- Goal: High performance
  - Physical Characteristic: Large cost to initiate I/O access
  - Organize data placement with files, directories, the free space bitmap(free map), and placement heuristics so that storage is accessed in large sequential units
  - Use Caching to avoid accessing persistent storage
- Goal: Named data
  - Physical Characteristic: Storage has large capacity, survive crashes, and is shared across programs
  - Support files and directories with meaningful names
- Goal: Controlled Sharing
  - Physical Characteristic: Device stores many users' data
  - Include access control metadata with files
- Goal: Reliable Storage
  - Physical Characteristic: Crashes can occur during updates. Storage devices can fail. Flash memory cells can wear out
  - Use transactions to make a set of updates atomic
  - Use redundancy to detect and correct failures
  - Move data to different storage locations to evenly wear out the disk

## Why is it important to know how file systems work even if I'm not building a file system?

- **Performance:** Even though file systems allow existing bytes in a file to be overwritten, inserting new bytes may require rewriting the entire file. Thus, autosaving may take as much as a second.
- **Corrupt Files** When overwriting the existing file with updated data, an untimely crash can leave the file in an inconsistent state, containing a combination of old and new versions.
- **Lost files** If instead of overwriting the document file, the applications writes to a new file, then deletes the original file, then moves the new file to the original file location, an untimely crash can leave the system with no copies of the document at all.

# The File System Abstraction

An OS abstraction that provides persistent named data.

- **File:** named collection of data in a file system. It is an arbitrary size, consisting of metadata and data. From the POV of the file system, a file's data is just an array of untyped bytes.
- **Directory:** Provides names for files. Contains a list of human readable names and a mapping from each name to a specific file or directory
- **Hard link:** Mapping between a name and the underlying file
- **Soft link:** Mapping from a file name to another file name. Unfortunately, this can cause dangling links: ie you link /a to point to /b which points to hi.txt then you unlink /b, /a will dangle)
- **Volume:** A collection of physical storage resources that form a logical storage device. This is usually an abstraction that corresponds to a logical disk.
- **Mounting:** Create a mapping from some path in the existing file system to the root directory of the mounted volume's file path. Used when plugging in USB drive to your computer

# The File System API

## Creating and Deleting Files

- `Create()` creates a new file that has initial metadata but no other data and it creates a name for the file in a directory
- `Link()` creates a hard link(new path name for existing file). After calling link(), there should be multiple path names that refer to the same underlying file. You cannot call link() on a directory
- `Unlink()` removes a file from its directory. If there are multiple links to a file, unlink() removes the specified name. If there is only one link to a file, unlink() also deletes the underlying file and frees the resources.
- `mkdir()` and `rmdir()` creates and deletes directories.

## Open and close

- `open()` a process calls open() to get a file descriptor it can use to refer to the open file.
- `close()` releases the open file record in the OS

## File access

- `read()` starts from the process's current file position and advances it by the number of bytes successfully read or written. Reads bytes
- `write()`starts from the process's current file position and advances it by the number of bytes successfully read or written. Writes bytes
- `seek()` changes a process' current position for a specified open file
- `mmap()` establish a mapping between a region of the process's virtual memory and some region of the file so that memory loads and stores to that virtual memory region will use the kernel's file cache or triggers a Page fault exception which causes the kernel to fetch the desire page from the file system to memory
- `munmap()` remove mapping created by mmap()
- `fsync()` Ensures all pending updates for a file are written to persistent storage before the call returns. Used to ensure that updates are durable and will not be lost in case of crash. If fsync() is called twice, the first is written to persistent storage before the second.

# Software Layers

## API and Performance

### System calls and libraries

Application libraries wrap syscalls mentioned above in the file system API to add additional functionality such as buffering.

### Block Cache

Since storage devices are much slower than DRAM, the OS has a block cache that caches recently read blocks and buffers recently written blocks.

### Prefetching

When a process reads the first two blocks of a file, the OS may prefetch the first 10 blocks. When the predictions are accurate, it can reduce latency from future reads(by returning the contents in the cache), reduce device overhead(by replacing a large number of small requests with one large one), and improve parallelism(by allowing hardware to process multiple requests at once in parallel).

Some disadvantages of prefetching include cache pressure(each prefetched block might displace another block in the block cache which might be more useful), I/O contention(prefetching costs IO, other requests might need to wait behind prefetch requests), and wasted effort(the prefetched blocks are not actually used).

## Device Drivers

- Used to translate the high level abstractions implemented by the OS and IO devices.
- Layering helps simplify OS by providing a common way to access various classes of devices.

### Memory Mapped IO

- IO devices typically have a controller with a set of registers that can be written and read to transmit commands and data to and from the device.
- Memory Mapped IO maps each device's control registers to a range of physical addresses on the memory bus. Reads and writes by the CPU to this physical address range go to the IO device's controllers instead of main memory

### DMA

- Most IO devices transfer data in bulk. Rather than requiring the CPU to read or write each word of a large transfer, IO devices can use direct memory accesses(DMA).
- DMA allows the IO device to copy a block of data between its own internal memory and the system's main memory.
- The OS uses memory mapped IO to set up a DMA transfer, then the device copies data to or from the target address without additional processor involvement
- After setting up a DMA transfer, the OS must not use the target physical pages for any other purpose until the DMA transfer is done.

# Lifecycle of a Disk Request

- When a process issues a system call like read() to read data from disk into the process’s memory, the operating system moves the calling thread to a wait queue.
- Then, the operating system uses memory-mapped I/O both to tell the disk to read the requested data and to set upDMA so that the disk can place that data in the kernel’s memory. - The disk then reads the data and DMAs it into main memory; once that is done, the disk triggers an interrupt.
- The operating system’s interrupt handler then copies the data from the kernel’s buffer into the process’s address space.
- Finally, the operating system moves the thread the ready list.
- When the thread next runs, it will returns from the system call with the data now present in the specified buffer.

# Files and Directories

Motivation: How do we go from a file name and offset to a block number?
Naive Approach: The file system is a dictionary that maps keys(file name and offset) to values(block number)

## Challenges with building a file system

1. **Performance** File systems need good spatial locality, where blocks that are accessed together are stored sequentially. The naive approach would just map block numbers in random places.
2. **Flexibility** File systems allow apps to share data, and thus must let people access small files, large files, short-lived files, and anything in between.
3. **Persistence** File systems must maintain and update both user data and internal data structures through OS crashes and power failures
4. **Reliability** File systems must store data for long periods of time

## File System Implementation Overview

Most file systems have directories, index structures, free space maps, and locality heuristics.

- **Directories** A way to map human readable file names to file numbers
- **Index structures** A way to translate the file number to locate the blocks of the file. This is usually some form of tree.
- **Free space maps** Tracks which storage blocks are free and which are in use as files grow and shrink. Also, blocks returned must have good spatial locality. Usually implemented as bitmaps
- **Locality Heuristics** Policies that group data to optimize performance. Some file systems group by directory, others periodically defragment their storage by rewriting existing files so that each file is stored in sequential blocks.

## Directories

- **Purpose** Translate human readable names to internal file numbers in a mapping
- **Implementation** Stored as a file
- The root directory's file number is agreed upon ahead of time, for the Unix Fast File System it is 2.
- To read `/home/albert/hi.txt` we search the root directory by reading the file associated with file number 2. In file2 , we search for the name `home` and find that /home is stored in file number 880. In file 880, we search for the name `albert` and find /home/albert is stored in 526. In file 526, we search for `hi.txt`, we find /home/albert/hi.txt is in file number 469
- The directory API is different from the standard open/read/close/write, since the file name to file number mapping cannot be corrupted
- The syscalls that modify directories are `mkdir`, `link`, `unlink`, and `rmdir`. Mkdir and rmdir create and delete the directory, `link("hi.txt", "new.txt")` creates a hard link to an existing file, `unlink("hi.txt")` removes that hard link.
- Processes can read the contents of directory files using the standard file read syscall

### Hard vs Soft Links

- **Hard Links** Multiple file directory entries that map different path names to the same file number. File systems must ensure that a file is only deleted when the last hard link is removed.
- File systems use reference counts that count the number of hard links to each file, starting with 1 at creation. Each call to `link()` increases the reference count by 1 and each call to `unlink()` decreases the reference count by 1
- **Soft Links** are directory entries that map one name to another name
- If the file system supports hard links, storing file metadata in directory entries would be problematic since whenever the file size changes, all the file's directory entries need to be updated

## Files

Purpose: Translate a file number to the blocks that belong to the file
Goals:

1. Put data blocks sequentially to maximize spatial locality
2. Provide efficient random access to any file block
3. Limit overhead to be efficient for smaller files
4. Be scalable to support large files
5. Provide a place to store metadata, ie reference count, owner, access control list, size

| File System        | Index Structure              | Free Space Management     | Locality Heuristics          | Index Structure Granularity |
| ------------------ | ---------------------------- | ------------------------- | ---------------------------- | --------------------------- |
| FAT(USB drives)    | Linked List                  | FAT array                 | Defragmentation              | Block                       |
| FFS(Unix)          | Tree(fixed, assymmetric)     | Bitmap(fixed)             | Block groups, reserve space  | Block                       |
| NTFS(Windows)      | tree(dynamic)                | Bitmap in file            | Best fit, defragmentation    | Extent                      |
| ZFS(Copy on write) | tree(copy on write, dynamic) | Space map(log-structured) | Write anywhere, block groups | Block                       |

### FAT Filesystem

<!-- ![File allocation table](/media/File-Systems/FAT.JPG) -->

- **Techniques** Uses a extremely simple index structure(linked list). FAT stands for file allocation table, its array of 32 bit entries in a reserved area of the volume. Each file corresponds to a FAT entry in the array, which is either the last block or contains a pointer to the next block.
- **Directories** map file names to file numbers. The file's number is the index of the file's first entry in the FAT. We can then parse the linked list to find the rest of the file's blocks.
- **Free space tracking** If data block i is free, then FAT[i] = 0
- **Locality Heuristics** Some FAT implementations use a next fit algorithm that sequentially scans the file allocation table starting from the last entry and returns the next free entry. This fragments the file, so there is often a defragmentation tool that reads files from their existing locations and rewrites them for better spatial locality.
- **Limitations**: Poor locality(badly fragmented files), Poor random access(need to sequentially traverse the FAT entries), limited metadata and no access control, no support for hard links, limitations on volume and file size(max file size if 4GB), and a lack of support for reliability techniques(does not support transactional updates).

### Unix Fast File System(FFS)

<!-- ![Multilevel index](/media/File-Systems/multilevel-index.JPG) -->

- **Techniques** FFS uses a carefully structured tree that locates any block of a file that is efficient for both large and small files. Each file is a tree with fixed size blocks as its leaves. Each file's tree is rooted in an _inode_ that contains the file metadata.
  - Typically the file's inode contains 15 pointers: the first 12 pointers are direct pointers that point directly to the first 12 data blocks of a file
  - The 13th pointer in the _inode_ is an indirect pointer which points to an array of direct pointers.
  - The 14th pointer in the \*inode is a double indirect pointer which points to an internal node which points to an array of indirect pointers, each pointing to an array of direct pointers.
  - The 15th pointer in the inode is a triple indirect pointer that contains an array of double indirect pointers.
  - All of the file system's inodes are located in an inode array. The file's file number, aka _inumber_ is an index into the inode array.
  - This asymmetric setup allows for efficient support for both large and small files.
- **Free Space Management** FFS allocates a bitmap with one bit per storage block, the ith bit in the bitmap indicates whether the ith block is free or in use.
- **Locality Heuristics** FFS uses block group placement and reserved space
  - _block group placement_ divides the disk into block groups so the seek time between any blocks in a block group will be small  
    <!-- ![Block Group Placement](/media/File-Systems/block-group-placement.JPG) -->
  - Each block group holds a portion of the metadata structures
  - FFS puts a directory and its files in the same block group
  - Within a block group, FFS writes to the first free block in the file's block group. This helps locality in the long term since fragmentation is reduced
- When the disk's space is almost full, the block group heuristic will perform poorly since new writes will be scattered randomly around the disk. Thus, FFS reserves a portion of the disk's space and presents a slightly reduced disk size to applications

### Windows New Technology File System(NTFS)

Instead of using fixed trees like FFS, NTFS uses extents and flexible trees
<!-- ![NTFS index record](/media/File-Systems/nfts-index-record.JPG) -->

- **Extent** variable sized regions of files that are stored in a contiguous region on the storage device
- **Flexible tree and master file table** Each file in NTFS is represented by a variable depth tree. The extent pointers for a file with a small number of extents can be stored in a shallow tree, deeper trees are only needed if the file becomes badly fragmented.
- **Master File Table(MFT)** stores an array of 1KB MFT records, each of which stores a sequence of variable size attribute records.
- **Resident attribute** Stores its contents directly in the MFT record
- **Non-resident attribute** Stores extent pointers in its MFT record and stores its contents in those extents.
- **MFT record** Includes standard information(creation time, last modified, owner ID), file name, and attribute list
- **Four stages of file growth**
  1. Small files can have their contents included in the MFT record as a resident data attribute.
  2. Normal file data has a small number of extents tracked by a single non-resident data attribute.
  3. Large files combined with a fragmented file system can make a file have so many extents that the extent pointers will not fit in a single MFT record. These files can have multiple non-resident data attributes in multiple MFT records, with the attribute list in the first MFT record indicating which MFT record tracks which range of extents
  4. If the file is huge, the file attribute list can be made non-resident, allowing arbitrarily large numbers of MFT records
- **Free Space Map**
  NTFS stores all metadata in a dozen files with low file numbers. For example, file number 5 is the root, file number 6 is the free space bitmap, file number 8 contains a list of the volume's bad blocks.
- **Locality Heuristics** NFTS uses best fit, where the system tries to place a newly allocated file in the smallest free region large enough to hold it. NTFS also has a defragmentation utility

### Copy on Write (COW)

When updating an existing file, COW file systems do not overwrite existing data or metadata, instead, they write new versions to new locations.

- **Motivations**
  - Small writes are expensive. Large sequential writes is much better than small random writes, and the gap will continue to grow since bandwidth grows faster than seek time/rotational latency
  - Small writes are expensive on raid since we need to read old data, read old parity, write new data, and write new parity. The number of times we read/write to parity bits grows linearly with the number of reads/write but not the size of the reads/writes
  - Large DRAM caches can handle essentially all file reads, thus the cost of writes dominate performance so optimizing write performance is the number one priority.
  - Flash storage works better with copy on write: there is no need to clear the erasure block and it writes data to a different location instead of overwriting the current data wearing out the flash drive evenly
  - Since old data isn't overwritten, we can support versioning
    <!-- ![Copy on Write](/media/File-Systems/copy-on-write.JPG) -->
- **Implementation:** We store inodes in a file rather than in the inode array. All the file system's contents are stored in a tree rooted in the root inode, when we update a block, we write the block and all of the blocks on the path from it to the root to new locations.

### ZFS file system

- **uberblock** the root of the ZFS storage system, ZFS has an array of 256 uberblocks and rotates successive versions
- **dnode** similar to inode, a file represented by a variable depth tree whose root is a dnode and leaves are data blocks
- **Free space map** per block group space maps. ZFS maintains a space map for each block group, has a tree of extents, and log structured updates.
- **Locality Heuristics** On writes, ZFS ensures sequential writes and batched updates.

## File and Directory Access Walkthrough

<!-- ![Walkthrough](/media/File-Systems/walkthrough.JPG) -->
Goal: Read the file /foo/bar/baz
Steps:

1. Read the root directory `/` to determine `/foo`'s inumber
2. Open and read file 2's inode. Since FFS stores pieces of the inode array at fixed locations on disk, given a file's inumber it is easy to read the file's inode
3. From the root directory's inode, extract the direct and indirect block pointers to determine which block stores the contents of the root directory.(block 48912)
4. Read that block of data to get the list of name to inumber mappings in the root directory to find `/foo` has inumber 231
5. Since we know `/foo`'s inumber, we read inode 231 to find where `/foo`'s data blocks are stored-block 1094 in this example.
6. Read block 1094 to get the list of name to inumber mappings in the `/foo` directory to find that the directory file `/foo/bar` has inumber 731.
7. Follow similar steps to read `/foo/bar`'s inode and data block 30991 to find `/foo/bar/baz`'s inumber is 402
8. Read `/foo/bar/inode` to get the data blocks 89310, 14919, 23301
9. Usually, data blocks are cached so we don't need to repeat this process.

# Questions

1. What does mmap() do?
