# 🔷 Weenix Operating Systems Kernel Development

## 🔶 Overview
This project is a **semester-long implementation of the Weenix operating system**, focusing on **kernel development, process management, virtual memory, and file system handling**. The Weenix kernel was incrementally built across three major assignments, each adding **essential OS functionalities**. The goal was to develop a **Unix-like kernel** from scratch while exploring key **systems programming concepts**.

---

## 🔷 Key Features and Learning Objectives:
✔ **Low-Level Systems Programming**: Designed and implemented kernel components in **C**, dealing with **hardware interactions, memory management, and concurrency**.  
✔ **Multithreading & Concurrency**: Implemented **kernel threading, synchronization primitives**, and **inter-process communication (IPC)**.  
✔ **Process & Memory Management**: Created a **thread scheduler**, virtual memory (VM) subsystems, **demand paging**, and **Copy-On-Write (COW) for process forking**.  
✔ **File Systems & Device Drivers**: Implemented a **Virtual File System (VFS)**, including **file descriptors, inode handling, and buffer caching**.  
✔ **Debugging & Self-Checks**: Used **GDB, assertions, and manual self-checking mechanisms** to validate system correctness.  

---

## 🔷 Project Structure

### 🔶 1️⃣ Kernel 1: Process Management & Threading
#### **Objectives:**
This phase focused on implementing **basic process control, thread scheduling, and synchronization mechanisms**. The kernel in this phase was responsible for:  
- **Creating and managing kernel threads (`kthread_t`)**.  
- **Implementing cooperative context switching (`context_switch`)**.  
- **Designing a simple Round-Robin Scheduler**.  
- **Implementing `fork()`, `exec()`, `exit()` system calls**.  
- **Thread synchronization using Mutexes (`kmutex_t`) & Condition Variables (`kcondvar_t`)**.  

#### **Key Features:**
- **Threading & Context Switching**: Implemented the **context switch mechanism** allowing user-space threads to execute cooperatively.  
- **Process Control & Signals**: Built **process lifecycle management** with **parent-child relationships** and **inter-process communication**.  
- **Basic Kernel Shell (`kshell`)**: Created a basic **debugging shell** with **interactive commands** to test thread scheduling.  

#### **Tests Performed for Kernel 1:**

✔ **Process Lifecycle Tests**  
   - Verified correct behavior of **process creation (`fork`), execution (`exec`), and termination (`exit`)**.  
   - Ensured correct **parent-child relationships** were maintained.
     
✔ **Thread Scheduling Tests**  
   - Checked that the **scheduler correctly switches between threads** using `context_switch`.  
   - Simulated multi-threaded workloads to verify the **round-robin algorithm**.
     
✔ **Mutex & Synchronization Tests**  
   - Tested **mutex locking & unlocking** to ensure proper synchronization.  
   - Introduced artificial race conditions to verify **mutex effectiveness**.
     
✔ **Edge Case Testing**  
   - Checked behavior when **a process attempts to fork itself recursively**.  
   - Verified **behavior when all threads terminate, leaving the system idle**.  

---

### 🔶 2️⃣ Kernel 2: Virtual File System (VFS) & File Handling
#### **Objectives:**
The second kernel phase introduced **file system management**, allowing user-space programs to perform **file I/O operations**. This included:  
- **Virtual File System (VFS) design & abstraction layer**.  
- **Handling file descriptors, inodes, and superblocks**.  
- **Implementing essential file system system calls (`open`, `read`, `write`, `close`, `dup`, `lseek`)**.  
- **Implementing directory structure handling (`mkdir`, `rmdir`, `unlink`)**.  
- **Synchronizing concurrent access using read-write locks**.  

#### **Key Features:**
- **Filesystem Abstraction Layer**: Created a **VFS interface** that allowed multiple file systems (e.g., **RAMFS, S5FS**) to be mounted.  
- **Path Resolution & Name Lookup**: Implemented **hierarchical file paths**, ensuring **correct inode traversal**.  
- **Buffer Caching & Performance Optimization**: Optimized **I/O operations** using **block caching techniques**.  
- **File Locking Mechanisms**: Added **synchronization primitives** for **concurrent access control**.  

#### **Tests Performed for Kernel 2:**

✔ **Basic File Operations Tests**  
   - Created test programs to verify **`open()`, `read()`, `write()`, and `close()`** behavior.  
   - Ensured **correct file descriptor allocation and release**.
     
✔ **Path Resolution & Directory Tests**  
   - Tested edge cases such as **absolute vs. relative paths** and **non-existent file handling**.  
   - Ensured `mkdir()` and `rmdir()` correctly updated the **inode table and directory structures**.
     
✔ **Concurrency and Locking Tests**  
   - Simulated multiple threads reading/writing to the same file to verify **lock enforcement**.  
   - Tested `dup()` and `lseek()` calls under **multi-threaded access scenarios**.
     
✔ **Corruption Handling Tests**  
   - Deliberately corrupted inode metadata and verified **system resilience to file corruption**.  
   - Checked that **orphaned file descriptors were correctly freed on process termination**.  

---

### 🔶 3️⃣ Kernel 3: Virtual Memory & Paging
#### **Objectives:**
The third and final kernel phase introduced **virtual memory management**, handling **demand paging, Copy-On-Write (COW), and page swapping**. This phase included:  
- **Implementing page allocation and deallocation (`kmalloc`, `kfree`)**.  
- **Creating page tables for process memory isolation**.  
- **Handling page faults and implementing demand paging**.  
- **Developing Copy-On-Write (COW) to optimize `fork()` system calls**.  
- **Designing an efficient page replacement algorithm**.  

#### **Key Features:**
- **Demand Paging & Lazy Allocation**: Pages were **loaded into memory only when accessed**, reducing startup costs.  
- **Copy-On-Write Optimization**: Forked processes **shared memory pages**, reducing **unnecessary duplication**.  
- **Page Fault Handling**: Implemented a **fault handler** to **load missing pages** from swap storage.  
- **Memory Protection & Security**: Enforced **access control** via **page permissions (read/write/execute)**.  

#### **Tests Performed for Kernel 3:**

✔ **Page Fault Handling Tests**  
   - Verified correct **handling of invalid memory accesses** using `gdb`.  
   - Created test programs to **trigger page faults** and check correct **trap handling**.
     
✔ **Copy-On-Write Optimization Tests**  
   - Used `fork()` to create **child processes** and verified **memory sharing efficiency**.  
   - Measured **memory allocation savings** when running large workloads with `COW`.
      
✔ **Virtual Memory Protection Tests**  
   - Attempted **buffer overflows** and **illegal memory accesses** to check enforcement of **access control mechanisms**.
     
✔ **Swapping & Page Replacement Tests**  
   - Forced memory exhaustion scenarios to verify **page replacement strategy**.  
   - Tested swapping behavior under **heavy memory usage scenarios**.  

---

## 🔷 Debugging, Self-Checks & GDB Usage

### 🔶 1. Self-Checks for Kernel Stability
- **Assertions (`KASSERT`)**: Used throughout kernel subsystems to **verify assumptions** and catch errors early.  
- **Manual Consistency Checks**: Ensured that **thread scheduling, process control, and file system states** were valid.  
- **State Validation**:  
  - Verified **run queue consistency** in the scheduler.  
  - Ensured **file descriptor integrity** in VFS operations.  
  - Checked **page table correctness** after `fork()`.  

### 🔶 2. GDB Debugging & Verification
- **Breakpoints & Step Debugging**:  
  - Set breakpoints inside critical functions (e.g., `do_fork()`, `do_exec()`) to analyze behavior.  
- **Backtrace Analysis (`bt`)**:  
  - Used to **track kernel panics and crashes** by identifying stack traces.  
- **Examining Kernel Structures (`print`)**:  
  - Inspected **process control blocks (PCBs), thread states, and memory mappings**.  
- **Memory Leak Checks**:  
  - Used `valgrind` and manual `kmalloc`/`kfree` tracking to detect **dangling pointers and leaks**.  

---

## 🔷 Development Environment
- **Language**: C (Low-Level Systems Programming)  
- **Platform**: Ubuntu 16.04 with **QEMU 2.5**  
- **Tools Used**:  
  - `GDB` for **debugging the kernel**  
  - `Valgrind` for **memory leak detection**  
  - `QEMU` for **testing the OS in a virtualized environment**  
  - `Makefiles` for **compiling and linking kernel components**  

---
📌 **Note:**  
The code for this project **cannot be made publicly available** due to academic restrictions. However, it can be shared **privately upon request** for review or discussion.
