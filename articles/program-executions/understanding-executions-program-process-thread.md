# Understanding Program Executions Fundamentals: Computer, Operating System, Program, Process, Thread

![Static Badge](https://img.shields.io/badge/date-2026-blue)

![Static Badge](https://img.shields.io/badge/computer-purple)
![Static Badge](https://img.shields.io/badge/operating--system-purple)
![Static Badge](https://img.shields.io/badge/program-purple)
![Static Badge](https://img.shields.io/badge/process-purple)
![Static Badge](https://img.shields.io/badge/thread-purple)

## Purpose

> How does a computer actually **run** the code you write?

## Layer 1: Hardware Foundation

### Physical Computers

A physical **computer** is a real machine with physical hardware that can execute instructions. A typical computer contains components such as:

- **CPU** (Central Processing Unit) executes instructions.
- **Storage** (hard discs, RAM, cache) holds the instructions and data.
- **Input/output (I/O) devices**, such as keyboard, mouse, printers, monitors, Network interfaces...
- **Data Bus** communicates data between I/O devices and the CPU

### CPUs and Cores

A **CPU** (Central Processing Unit) is the component responsible for executing instructions given by running programs (processes).

A **core** is an independent processing unit capable of executing instructions.

- Each core has an internal memory (cache) to store data (input data, output data) required to execute an instruction.
- Can access through the Data Bus to other shared resources of the Operating System (RAM, physical memory).

### Single core vs Multi core

A single core can execute only a just one piece of work at any given instant. The operating system can, however, rapidly switch between different activities, giving the appearance that many activities are executing simultaneously.

- Older CPUs typically contain one core.
- Modern CPUs contain one or more cores.
- Virtual machines can be created with one or more cores.

For example, a computer with four physical cores can potentially execute instructions on four cores **simultaneously**:

```
CPU
├── Core 1
├── Core 2
├── Core 3
└── Core 4
```

### Physical vs Virtual Machines

A **physical machine** is a real computer with physical hardware.

A **virtual machine** (VM) is a software-defined computer (with one or more cores) that runs on top of a physical machine. A virtual machine behaves like a computer from the perspective of the software running inside it. It has virtual CPUs, memory, storage, and other virtualized devices.

A virtualization layer, typically called a **hypervisor**, provides virtual hardware to the VM. Virtualization allows multiple virtual computers to run on the same physical hardware while maintaining **a degree of isolation** between them.

```
┌────────────────────────────────┐
│ ┌────────────────────────────┐ │
│ │ ┌──────────┐  ┌──────────┐ │ │
│ │ │ VM 1     │  │ VM 2     │ │ │
│ │ │ Virtual  │  │ Virtual  │ │ │
│ │ │ hardware │  │ hardware │ │ │
│ │ └──────────┘  └──────────┘ │ │
│ │ Hypervisor                 │ │
│ └────────────────────────────┘ │
│ Operating System               │
└────────────────────────────────┘
Physical Machine
```

## Layer 2: Operating Systems

An **Operating System** (OS) is the software layer that

- manages the computer's components (such as CPU, memory, storage, I/O devices)
- programs that run on it
- grants processes access to the resources such as memory and CPU time.

Examples of operating systems include Linux, Windows, Android, and macOS.

The OS acts as a **referee** between all running processes, ensuring fair access to CPU time, memory, and I/O resources, while ensuring authorized access.

## Layer 3: Program → Process → Thread

### Program

A **program** is a set of instructions (code) and associated data that describes how a computer should perform a task.

- Typically stored as code and data on persistent storage (disk).
- By itself, it is not an executing activity; it becomes one when the Operating System loads and starts it.
- The same program can be started multiple times, creating multiple separate processes.

### Process

A **process** is a **running instance** of a program started by the Operating System on a computer.

Characteristics of processes:

- Typically, hundreds of processes run on a modern computer
- A process can execute independently from other processes running on the same system
- Can be foreground (interaction with the user) or background (no user interaction required)
- Processes are **isolated**: one process cannot directly access another process's memory

#### Process Lifecycle

When the Operating System starts a program, it;

- Creates a process to represent its execution
- Provides it with the **resources** (address space: memory and data) needed for execution.
- Assigns it an identity (**process id**) and lifecycle with its own **execution state**.
- Manages its interaction with other processes and the hardware

A process moves through different states during its lifetime.

```
                 ┌───────────┐
                 │           │
                 ▼           │
             ┌────────┐      │
             │  Ready │◄─────┤
             └───┬────┘      │
                 │           │
              scheduled      │
                 │           │
                 ▼           │
             ┌─────────┐     │
             │ Running │─────┘
             └────┬────┘
                  │
          ┌───────┴────────┐
          │                │
       waits            finishes
          │                │
          ▼                ▼
     ┌─────────┐      ┌────────────┐
     │ Waiting │      │ Terminated │
     └────┬────┘      └────────────┘
          │
       becomes ready
          │
          └──────────────►
```

> ⚠️ **Hidden Complexity**:
> 
> This diagram is simplified. The OS actually maintains scheduling queues, uses scheduling algorithms (round-robin, priority-based, etc.), and handles context switching (saving and restoring process state). These details become critical when discussing concurrency and synchronization.

### Thread

A **thread** is an execution flow within a process. Threads are the basic units through which execution can be performed within a process.

Characteristics of threads:

- A process can contain one or multiple threads at any given moment of time.
  - The **main thread** is the initial thread created when a process starts.
  - Typically, a thread spawned to do an specific work is called **Worker** thread.
  - A **Thread pool** is collection of reusable worker threads.
- Each thread represents a sequence of instructions that can be scheduled for execution by the OS
- A thread has its own:
  - **Execution state** (ready, running, waiting, etc.)
  - **Stack** (local variables, function call information)
  - **Program counter** (tracks current instruction)
- Threads within the same process **share**:
  - The process's memory (address space)
  - Open file handles
  - Other process resources

> ⚠️ **Hidden Complexity**:
> 
> Threads share memory, which is powerful but dangerous. Unlike processes (which are isolated), threads can accidentally overwrite each other's data. The synchronization primitives (locks, semaphores, monitors) exist precisely because of this shared memory model. Memory visibility, atomic operations, and cache coherency all become relevant here.

#### Threads Lifecycle Inside a Process

Consider this scenario:
> A process starts with a main thread. After 1 CPU cycle, the main thread creates 3 worker threads, each worker thread reads a different file
> - Worker T1 takes 1 cycle to read and then terminates
> - Worker T2 takes 2 cycles to read and then terminates
> - Worker T3 takes 3 cycles to read and then terminates.
> - The main thread waits for all workers to complete. Once all files are read, the main thread takes 1 cycle to save data to disk and then terminates.

In a single-core CPU

| CPU Cycles      | 1                  | 2                  | 3                  | 4                  | 5                  | 6                  | 7                  | 8                  | 9          |
|-----------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|------------|
| **Main Thread** | **Running** (init) | Waiting            | Waiting            | Waiting            | Waiting            | Waiting            | Waiting            | **Running** (save) | Terminated |
| **Worker T1**   | —                  | **Running** (read) | Terminated         | —                  | —                  | —                  | —                  | —                  | —          |
| **Worker T2**   | —                  | —                  | **Running** (read) | **Running** (read) | Terminated         | —                  | —                  | —                  | —          |
| **Worker T3**   | —                  | —                  | —                  | —                  | **Running** (read) | **Running** (read) | **Running** (read) | Terminated         | —          |

In a double-core CPU

| CPU Cycles      | 1                  | 2                   | 3                  | 4                  | 5                  | 6                  | 7          | 
|-----------------|--------------------|---------------------|--------------------|--------------------|--------------------|--------------------|------------|
| **Main Thread** | **Running** (init) | Waiting             | Waiting            | Waiting            | Waiting            | **Running** (save) | Terminated |
| **Worker T1**   | —                  | **Running** (read)  | Terminated         | —                  | —                  | —                  | —          |
| **Worker T2**   | —                  | **Running** (read)  | **Running** (read) | Terminated         | —                  | —                  | —          |
| **Worker T3**   | —                  | —                   | **Running** (read) | **Running** (read) | **Running** (read) | Terminated         | —          |

## Execution Stack: Putting It All Together

Now we can see the complete picture:

```
Your Code (Program)
↓ [OS loads and executes]
Process (isolated execution environment)
↓ [Execution happens through]
Thread(s) (scheduled execution units)
↓ [OS scheduler distributes across]
CPU Cores (via the OS)
↓
Hardware (executes instructions)
```

Key insights:

- **Programs** are static; **Processes** are dynamic instances
- **Processes** provide isolation; **Threads** provide concurrency within that isolation
- The **OS scheduler** manages which thread runs on which core at any moment
- **Cores** can execute multiple threads, but the OS decides the scheduling
