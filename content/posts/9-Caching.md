---
title: Caching
date: "2021-10-22T23:46:37.121Z"
template: "post"
draft: false
slug: "caching"
category: "Operating System Notes"
tags:
  - "Notes"
  - "Textbook"
  - "Operating System"

description: "Notes on caching for operating systems"
socialImage: "/media/image-2.jpg"
---

## Examples of Caches

1. **TLBs:** Caches recent results of address translation from page tables.
2. **Virtually addressed caches:** Stores memory value associated with a virtual address
3. **Physically addressed caches:** This is a part of the virtually addressed cache. Each entry stores the memory value associated with a physical memory location.
4. DNS caches, Web content caches, web search, email clients, incremental compilation, just in time translation, virtual memory, file systems, conditional branch prediction all use caches.

## Design Challenges of Caches

- **Locating the cached copy:** Check if the value you are looking for is in the cache
- **Replacement Policy:** Which elements should be replaced when the cache is full?
- **Coherence:** What should we do when the cache is out of date?

## Naive Cache Implementation

- An (address, value) pair stored in a map. When looking for something, we check if it exists in the map and return the value if it does(aka a cache hit). If it doesn't this is a cache miss.
- The cost of retrieving data out of the cache must be less than fetching the data from memory. The likelihood of a cache hit must be high enough to be worth the effort.
- Two sources of predictability are temporal locality and spatial locality. Programs tend to reference the same instructions and data they recently referenced(Temporal Locality). Programs also tend to reference data that is near recently referenced data.
- `Latency(read request)=Prob(cache hit) × Latency(cache hit)+ Prob(cache miss) × Latency(cache miss)`
- **Write through cache:** All updates are sent immediately onward to memory
- **Write back cache:** Updates are stored in the cache and only sent to memory when the cache runs out of space.

## Memory Hierarchy + When to use Caches

- Fundamental tradeoff: the smaller memory is, the faster it can be. The slower memory is, the cheaper it can be.

### Working Sets

- A sufficiently large cache will have a high cache hit rate, since if you can fit all the memory and data, the miss rate will be 0.
- Most programs have an inflection point where a critical mass of program data can just barely fit inside the cache. This is known as a _working set_
- **Thrashing:** Occurs when a cache is too small to fit inside the working set.

### Zipf Model

- **Motivation:** New data is constantly being added to the web, it is likely that your cache is outdated when you request something from it. There is no working set for the web, a small amount of pages might be useful(gmail, facebook, youtube) but there is a huge amount of web pages that are only visited from time to time.
- Zipf can be described as
  `Frequency of visits to the kth most popular page ∝ 1 / kα`
  and can model the popularity of library books, the population of cities, salary distribution, number of facebook friends, etc.
- **Heavy tailed distribution:** A significant number of references will be the most popular items, but a substantial portion will be less popular ones.

## Cache Design

- **Fully associative:** Addresses can be stored anywhere in the cache, there is no ordering. When looking for an address, we check every entry in the cache: there is a cache hit if any of the entries match.
  Cons: This is really slow as caches becomes bigger.
- **Direct mapped:** Each address is hashed then the data stored at the location of the hash.
  Cons: If two different addresses hash to the same entry, the system will thrash.
- **Set associative:** The cache hashes the address to get a location, then checks the entry in each table in parallel. It returns the value if any entries match the address.

## Replacement Policies

**Motivation:** If we look up an address and find that it is a miss, how do we choose which memory block to replace?

### Random

- **How it works:** Choose a random block to replace.
- **Pros:** Randomly evicting a block is fast, thus it is good for first-level hardware caches. Non-pessimal, randomness will never be the worst possible choice on average.
- **Cons:** Unpredictable, it might counteract an application that tries to take advantage of the cache.

### First in First Out(FIFO)

- **How it works:** Evict the cache block or page that has been in memory the longest.
- **Pros:** Each page spends an equal amount of time in the cache.
- **Cons:** Imagine a program that cycles through an array, where the array is too large to fit into the cache. Then the cache is useless.

### Optimal Cache Replacement (MIN)

- **How it works:** Replace whichever block that is used farthest in the future.
- **Pros:** This is optimal. (aka the best possible algorithm)
- **Cons:** Impossible, MIN requires knowledge of the future.

### Least Recently Used (LRU)

- **How it works:** Evict the block that has not been used for the longest period of time. In software, this is done with a linked list. In hardware, it is approximated with the clock algorithm
- **Pros:** Takes advantage of temporal locality
- **Cons:** Weak when the task is repeated scans through an array, the optimal strategy there is FIFO

### Least Frequently Used(LFU)

- **How it works:** Discard the page used least often
- **Pros:** Good in web caching. Ie just because we recently looked at spas in Iceland doesn't mean we will frequently look at spas in Iceland. Frequency is a better approximation than time.
- **Cons:** If an item is referenced repeatedly for a short period of time, this will stay in the cache long after it should have been evicted.

### Belady's Anomaly

Intuitively, adding space to a cache should always improve or maintain the cache hit rate. However, _Belady's Anomaly_ says this isn't the case.

For some replacement policies like FIFO, adding more space will drastically change the contents of the cache, thus making the hit rate worse.

## Memory Mapped Files

**Demand Paging:** Applications can access more memory than is physically present on the machine by using memory pages as a cache for disk blocks.
**Memory Mapped File:** The program directly accesses and modifies the contents of the file, treating memory as a write-back cache for disk.

### Pros

- **Transparency:** The program can write directly into the file as if it was memory
- **Zero copy I/O:** The OS does not need to copy file data from kernel buffers into user memory and back, it can just change the program's page table entry to point to the physical page frame containing that part of the file.
- **Pipelining:** The program can operate on data in the file as soon as page tables are set up, no need to wait for the entire file to be read into memory.
- **Interprocess communication:** Two processes can share memory via a memory mapped file.
- **Large Files:** The only size limit of a memory mapped file is the size of the virtual address space.

### Implementation

OS provides a system call where the kernel initializes a set of page table entries for that region of virtual address space, setting each entry to invalid.

When the process gives an instruction that touches an invalid mapped address:

1. The TLB miss occurs, triggering a full page table lookup in hardware.
2. The hardware finds the page table entry and finds that it is invalid, causing a _hardware page fault_ into the OS kernel.
3. The OS kernel converts the address to a file offset.
4. The OS kernel allocates an empty page frame and reads the required file block into the empty page frame.
5. The disk interrupts the processor when the file block has been read and the scheduler resumes handling the page fault exception.
6. The kernel updates the page table entry to point to the page frame allocated for the block and sets the entry to valid.
7. The OS resumes execution of the process.
8. The TLB misses, trigger a full page table lookup.
9. The hardware returns the page frame to the processor, loads the TLB with the new translation, evicts the previous TLB entry, then uses the translation to construct a physical address.

To create the empty page frame that holds the incoming page from disk, the OS:

1. Selects a page to evict using the current replacement policy.
2. Finds page table entries that point to the evicted page, usually through a core map.
3. Set each page table entry to invalid, to prevent other processes from using the evicted page while it is being evicted.
4. Copy back any changes to the evicted page.

## Approximating LRU

Keeping a linked list to track when pages were last used in hardware is prohibitively expensive, thus, we will use the clock algorithm as an approximation instead.

### Clock algorithm

The OS scans through the core map of physical memory pages. 2. For each page frame, it records the value of the use bit and clears the use bit. 3. The OS does a shootdown for each page frame that has its use bit cleared.

A common usage of the collected values of the use bit is the **not recently used/kth chance algorithm** where the kernel picks a page that has not been set for the last k sweeps of the clock algorithm.

## Virtual Memory

By backing every memory segment with a file on disk, we create virtual memory.

### Self Paging

**Problem:** There is a malicious program that tries to grab as much memory as possible by using multiple threads to cycle through memory and add those pages.
**Solution:** Self paging assigns every process its fair share of page frames. As the system starts to page, it evicts the page from whichever process has the most pages allocated to it.
**Cons:** If there are two processes, one with a working set of 2/3 of memory and the other with 1/3 of memory, the ideal split is for the first process to get 2/3 of memory and the other to get 1/3 of memory. Unfortunately, self-paging will results in both process splitting the memory, causing the first process to thrash

### Swapping

**Problem:** Sometimes, there is simply too much data in all of the process' working sets to effectively run them at once. If we try to run things normally, we will end up thrashing with every process.
**Solution:** We can evict an entire process from memory (aka swapping)
**Cons:** The process that just got evicted from memory will be sad :(

# Future

1. **Low latency backing store:** Things that were once on hard disk are moving onto SSDs, things that are on SSDs will move onto even faster storage devices.
2. **Variable page sizes:** We will move toward smaller effective page sizes due to low latency solid state storage and cluster memory.
3. **Memory aware applications:** Applications can be tuned to how much memory is in the first level cache to increase performance.

# Questions

1. How does set associative caches work?
2. How does virtual memory work?
