---
title: Reliable Storage
date: "2021-11-20T23:46:37.121Z"
template: "post"
draft: false
slug: "reliable-storage"
category: "Operating System Notes"
tags:
  - "Notes"
  - "Textbook"
  - "Operating System"

description: "Notes on Reliable Storage"
socialImage: "/media/image-2.jpg"
---

# Why do we need reliable storage?

- Physical devices may fail, they may wear out, they may become damaged. Large organizations see annual disk failure rates of 2-4%, which means that over 10 years 30% of the data in the disk would be gone
- Two threats to reliability
  - **Operation Interruption:** A crash or power failure in the middle of a series of related updates leaves the data in an inconsistent state. Ie crashing when moving a file so we now have two copies
    - **Solution** Create transactions for atomic updates.
  - **Loss of stored data:** Failure of non-volatile storage media can cause stored data to become corrupted or disappear
    - **Solution** Create redundancy for failures, ie RAID or using multiple layers of checksums

## Reliability, Availability,

- **Reliable:** Can the system perform the intended function? A system can be available but not working properly. Can increase reliability by testing system.
- **Available:** Can the system respond to a request? Also known as the percentage of time when the system is up. Can achieve higher availability with deploying apps across multiple geographically distant servers using load balancing to reroute requests to healthy servers
- **Durability:** Is the data still in the system? A system is durable even if you can't access the data(ie bury a usb underground). Can achieve higher durability by making backups, storing in different geographical area, performing checksums.
- A data block replicated across 100 machines is highly reliable and highly available for reads, but not highly available for writes.

## Transactions + Atomic Updates

- **Problem:** When the system crashes in the middle of several updates, the data in the system might not make sense or leaves it in a vulnerable. For example, bob is transferring 100 dollars to alice and the bank database crashes, it could have crashed when the bank pulled 100 dollars out of bob's account but before it sent the 100 dollars to alice's account. Now, bob just lost 100 dollars
- **Naive Solutions:** The unix fast file system would control the order of updates such that if a crash occurred, it could scan the disk during recovery and identify and repair inconsistent data structures. Now if this sounds kind of sketchy, it is and there were three main problems with it.
  - **Extremely slow recovery:** When the machine reboots, it has to scan all of its disks for inconsistent metadata structures.
  - **Slow Updates** To ensure the unix fast file system updates were analyzable, they had to be ordered correctly, making it hard to parallelize the stream of requests to storage devices
  - **Complex reasoning** We need to consider all possible failure scenarios to make sure that it is always possible to recover the system to a consistent state.
- **Application Level Approaches** The POSIX api first writes updates to a temporary file, then renames the temporary file to atomically replace the previously stored file.

### The Transaction Abstraction
- **Problem** You are updating a website and you want to update files stored in `/prod` with files stored in `/testing`. However, you don't want users to see the intermediate step where some docs in `/prod` are old and some are new. 
- **Solution** The transaction first does all of its changes, then commits the transaction. If at any time the transaction fails before the commit, we roll back everything done in the transaction. The benefit here is that transactions are ACID.(Yes this sounds like a dumb acronym they teach you in 6th grade to memorize the dinosaurs but this is actually an extremely well thought out acronym. ACID = transaction)
    - **Atomicity** All or nothing, either a transaction commits or the system is not changed.
    - **Consistency** The transaction moves from one legal state to another. All invariants must hold at the start of the transaction and during the commit.
    - **Isolation** The updates within a transaction cannot interleave with other transactions. Even if multiple transactions concurrently execute, the results will be that T is executed entirely before T` or vice versa
    - **Durability** Once a transaction is committed, the only way to change state is with another transaction. Must survive crashes!
    - Transactions are just critical sections with durability.

## Transaction Implementations
Problem: We want to group related writes in a single, atomic transaction but for disks and other hardware, the largest atomic operation is a single write(sector or page). Thus, we need a way to combine multiple writes into a single atomic operation.

**Idempotence:** The property where an operation can be applied multiple times without changing the result beyond the first application.

Idea: A transactional system can store all of the transaction's intentions, and the updates will only be made after all of the transaction's intentions are written down. If we store all updates as idempotent operations, we can also start off where we finished during a crash

### Redo Logging
- An implementation of transactions using four phases
  1. **Prepare** Append all planned updates to the log.
  2. **Commit** Append a commit to the log, indicating the transaction has been committed
  3. **Write-back** Write values in log to disk
  4. **Garbage Collect** Remove transaction records from log

- If the system crashes in the middle of the transaction, we scan sequentially through the log and depending on the type of record we do different things. If the record is an
  1. **Record Type = Update** add this record to a list of updates for the specified transaction
  2. **Record Type = Commit** Write back all of the transaction's logged updates to disk
  3. **Record Type = Roll-back** Discard list of updates for specified transaction
  4. **Record Type = End** Discard all update records for transactions without commit records.

- Implementation Details
  1. Multiple transactions might be executing at once, meaning that we need an identifier to show which transaction the log record belongs to
  2. The write-back phase can be asynchronous, meaning we can minimize latency in the commit phase and write-back multiple things at once. However, this makes crashes take longer and the log needs to take more space in the disk.
  3. Since log records are idempotent, we can reapply an update from a redo log multiple times.
  4. Restarting in recovery is ok, we just continue from where we last left off.
  5. Once write-back completes, the space in the log can be reclaimed
  6. All transaction updates must be on the log before the commit, all commits must be on disk before the write backs, all write backs must be on disk before the transaction log records can be garbage collected. 

- Performance: redo logging can have performance better than update in place, especially for small writes. Four factors allow efficient redo logging:
  - Log updates are sequential, so writing to spinning disks can be very fsat.
  - Write back is asynchronous, so transactions using redo logs can have good response time and throughput
  - The only barriers are when updates are logged and before the commit is logged, and after the commit is logged and before write-backs begin
  - Group commits combine a set of transaction commits into one log write to amortize the cost of initiating the write

## Isolation and Concurrency
To fulfill ACID properties, we need to ensure all of its properties. Redo logging only ensures durability, we also need isolation if there are multiple transactions executing at once. 

Naive Solution: Two phase locking divides a transaction into two phases, the expanding phase where locks can only be acquired and the contraction phase where locks can be released but not acquired.

- Two phase locking results in serializability, meaning the execution is equivalent to processing the transactions one at a time.
- If we encounter deadlocks, we can roll back a transaction and restart at some later time. 

## Transactions in File Systems
If the file system crashes when updating data, it could be left in an inconsistent state. Thus, all file system updates should be done in transactions.

- **Logging:** Include all updates to disk in transactions
- **Journaling:** Apply metadata updates via transactions. Apply file updates normally. Ths is because file updates are idempotent
- **Copy-on-write:** Does not overwrite data in place, but rather writes data then replaces pointer.
  - Copy on write systems also usually have batch updates, where you batch multiple updates into one atomic group
  - This also has an intent log, a redo log that tracks updates between large batch updates

# Error Detection and Correction
Problem: All data storage devices will fail
Solution: Use layered error detection and correction: checksums for devices, error correcting codes, RAID architectures, end to end correctness checks.

## Types of Storage Device Failures
### Sector and Page Failures
Problem: Data in the sector of a disk is lost. Flash page failures are the same thing but on SSDs.
Cause: The magnetic coating on spinning disks get scratched off, machine oil coats a sector of the spinning disk, the disk head gets too far from the surface. In flash storage, a page can get worn out from using it too many times.
Mitigation: 
- Error correcting codes: Encode data with redundant metadata that tells you if a sector becomes damaged
- Remapping: remap failed sectors to good ones
Pitfalls:
- Assuming that non-recoverable read error rates are negligible. A bit error rate of one sector per 10**14 bits means in a 2TB read there is a 10% chance of error
- Assuming non-recoverable read error rates are constant. Specific workloads can wear out some parts more than others
- Assuming failures are independent. Errors are usually correlated by time and space
- Assuming uniform error rates. Some models are just worse than others

### Device Failures
Problem: Device stops being able to read or write to any disk sector.
Cause: 
- For disks: Disk heads become damaged, power surge damages electronics. 
- For flash devices: Wear out and device runs out of space for remapping
Mitigation: Notify the device reading the disk that there is an error
Pitfalls:
- Trusting advertised failure rates. Big tech lies
- Failures are usually correlated
- A mean time to failure is not the same as its useful life
- Failure rates are not constant, there are devices with disk infant mortality, and devices that die after wearing out
- Ignoring warning signs
- Devices don't behave identically, even if manufactured identically.

## RAID Multi disk redundancy for error correction
- Idea: Spread data redundantly across multiple disks in order to tolerate individual disk failures.
- Mirroring: The system write each block of data to two disks and can read data from either disk. If one fails, read from the other disk
- Rotating Parity: store several blocks of data on several disks and protect those blocks with one redundant block stored on yet another disk.
  - Each disk has 1 / # data blocks of its space dedicated to parity and is responsible for storing (# data blocks - 1) / # data blocks.
- Striping data: a strip of several sequential blocks is placed on one disk before shifting to another disk for the next strip. Thus, requests larger than a block but smaller than a strip require just one disk to seek then read/write.
- Recovery: If a disk suffers a sector failure, it reports it when there is an attempt to read the sector then remaps the damaged sector to a spare and reconstructs the lost sector from the other disks and rewrites it to the original disk.
- Ways RAID can fail
  - Both disks fail
  - One disk fails and the other disk's sector fails
  - Both disk's sectors fail
### Improving RAID Reliability
- There are three main things we can do to improve reliability: increase redundancy, reduce non recoverable read error rates, reduce mean time to repair
- To increase redundancy, duplicate it to 3 disks instead of 2.
- To reduce non-recoverable read error with scrubbing, read the entire contents of disk, find sectors with unrecoverable read errors, and reconstruct lost data from remaining RAID disks
- To reduce non recoverable read error rates with more reliable disks, use higher quality disks
- To reduce mean time to repair, use hot spares: disks that are idle but plugged into a server so that if one disk fails, the hot spare can be automatically activated
- Reduce mean time to repair with declustering: split the reconstruction of a failed disk accross multiple disks.
- Pitfalls: assuming uncorrelated failures, ignoring risk from latent errors, not implementing scrubbing, not having a backup, 