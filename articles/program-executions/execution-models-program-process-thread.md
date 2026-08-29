# Understanding Program Executions Fundamentals: Computer, Operating System, Program, Process, Thread

## Computers

A physical **computer** is a real machine with physical hardware that can execute instructions. A typical computer contains components such as:

- CPU (Central Processing Unit) executes instructions.
- Storage (hard discs, RAM, cache) holds the instructions and data.
- Input/output (I/O) devices, such as keyboard, mouse, printers, monitors, Network interfaces...
- Data Bus communicates data between I/O devices and the CPU

### CPUs and Cores

A **CPU** (Central Processing Unit) is the component responsible for executing instructions given by running programs (processes).

A **core** is an independent processing unit capable of executing instructions.

- Each core has an internal memory (cache) to store data (input data, output data) required to execute an instruction.
- Can access through the Data Bus to other shared resources of the Operating System (RAM, physical memory).

### Physical vs Virtual Machines

A **physical machine** is a real computer with physical hardware.

A **virtual machine** (VM) is a software-defined computer (with one or more cores) that runs on top of a physical machine. A virtual machine behaves like a computer from the perspective of the software running inside it. It has virtual CPUs, memory, storage, and other virtualized devices.

A virtualization layer, typically called a **hypervisor**, provides virtual hardware to the VM. Virtualization allows multiple virtual computers to run on the same physical hardware while maintaining **a degree of isolation** between them.

```text
┌─────────────────────────────────┐
│ ┌─────────────────────────────┐ │
│ │ ┌──────────┐  ┌──────────┐  │ │
│ │ │ VM 1     │  │ VM 2     │  │ │
│ │ │ Virtual  │  │ Virtual  │  │ │
│ │ │ hardware │  │ hardware │  │ │
│ │ └──────────┘  └──────────┘  │ │
│ │ Hypervisor                  │ │
│ └─────────────────────────────┘ │
│ Operating System                │
└─────────────────────────────────┘
Physical Machine
```

### Single core vs Multi core

A single core can execute only a just one piece of work at any given instant. The operating system can, however, rapidly switch between different activities, giving the appearance that many activities are executing simultaneously.

- Older CPUs typically contain one core.
- Modern CPUs contain one or more cores.
- Virtual machines can be created with one or more cores.

For example, a computer with four physical cores can potentially execute instructions on four cores **simultaneously**:

```text
CPU
├── Core 1
├── Core 2
├── Core 3
└── Core 4
```

## Operating Systems

An **Operating System** (OS) is the software layer that

- manages the computer's components (such as CPU, memory, storage, I/O devices)
- programs that run on it
- grants processes access to the resources such as memory and CPU time.

Examples of operating systems include Linux, Windows, Android, and macOS.

```
Process (running program)
↓
Operating System -> manages process and resources
↓
Hardware -> provides the physical resources
```

### Process Lifecycle

When a program is started, the operating system creates a process to represent its execution and assigns dedicated resources.

A process moves through different states during its lifetime. A simplified lifecycle is:

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
     ┌─────────┐      ┌───────────┐
     │ Waiting │      │ Terminated│
     └────┬────┘      └───────────┘
          │
       becomes ready
          │
          └──────────────►
```

## Program

A **program** is a set of instructions that describes how a computer should perform a task.

- Typically stored as code and data on persistent storage.
- By itself, it is not an executing activity; it becomes one when the operating system loads and starts it.

## Process

A **process** is a **running instance** of a program started by the Operating System on a computer.

- When the Operating System starts a program, it creates a process and provides it with the **resources** (address space: memory and data) needed for execution.
- Has its own **execution state** and lifecycle.
- Can be foreground (interaction with the user) or in the background (no interaction required).
 
There are typically hundreds of processes running on a modern computer.

- A process can execute independently from other processes running on the same system.

## Thread

A **thread** is an execution flow within a process. Threads are the basic units through which execution can be performed within a process.

- A process can contain one or multiple threads.
- Each thread represents a sequence of instructions that can be scheduled for execution by the operating system.
- A thread has its own **execution state** and **resources** (stack), while operating within the resources provided by its process.
