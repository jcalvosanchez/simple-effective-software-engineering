# Program Execution Models: Sequential, Concurrent, Parallel, and Distributed

![Static Badge](https://img.shields.io/badge/date-2026-blue)

![Static Badge](https://img.shields.io/badge/program--executions-purple)
![Static Badge](https://img.shields.io/badge/sequential--programming-purple)
![Static Badge](https://img.shields.io/badge/concurrent--programming-purple)
![Static Badge](https://img.shields.io/badge/parallel--programming-purple)
![Static Badge](https://img.shields.io/badge/distributed--programming-purple)

![Static Badge](https://img.shields.io/badge/tasks-purple)
![Static Badge](https://img.shields.io/badge/execution--models-purple)

## Purpose

> Now that we understand **what** processes and threads are, how do we **use** them to execute code? What execution models exist, and when should we use each one?

## Sequential Execution

In sequential execution, tasks are performed one after another, never simultaneously. Task B starts after Task A has finished, and Task C starts after Task B has finished.

```
Task A ──────> Task B ──────> Task C
```

This is the simplest execution model: at any point in time, there is only one active flow of execution.

### Pros

1. **Simplicity**: Easy to reason about. Code executes top-to-bottom.
2. **Predictability**: Execution order is guaranteed.
3. **Debugging**: No race conditions, no non-deterministic bugs. Stack traces are clear.

### Cons

1. **Poor resource utilization**: While one task waits (e.g., for I/O), the CPU is idle.
2. **Low responsiveness**: Long-running tasks block others. A UI might freeze.
3. **Performance**: On multi-core systems, single-threaded execution wastes cores.
4. **Throughput**: Can only process one task at a time.

## Concurrent Execution

In **concurrent execution**, multiple tasks can make progress during overlapping periods of time, not necessarily at exactly the same moment.

```
Task A ──┐   ┌──────┐       ┌──────┐
         └───┘      └───────┘      └──>

Task B ──────┐   ┌──────┐
             └───┘      └──────────────>
```

Concurrency is therefore concerned with managing multiple activities that are in progress at the same time, even when they are not executing simultaneously.

- **Context switching**: The OS saves one task's state and loads another's
- Tasks may share memory and resources
- A task waiting for I/O doesn't block others—the OS switches to another task
- Order of completion is **non-deterministic**

### Pros

1. **Better resource utilization**: While Task 1 waits for I/O, Task 2 and 3 can run.
2. **Responsiveness**: Long-running tasks don't block others. A UI remains responsive.
3. **Throughput**: Can process many I/O-bound tasks efficiently.
4. **Language support**: Threads, goroutines, async/await make concurrency accessible.

### Cons

1. **Complexity**: Multiple tasks running "at the same time" complicates reasoning.
2. **Race conditions**: If tasks share memory, they can interfere with each other.
3. **Synchronization overhead**: Locks, mutexes, and synchronization primitives add complexity and performance cost.
4. **Debugging difficulty**: Non-deterministic bugs (Heisenbugs) are hard to reproduce and fix.
5. **Context switching cost**: Saving/restoring task state consumes CPU time.

## Parallel Execution

In **parallel execution**, multiple tasks execute simultaneously.

- Requires **multi-core** hardware (CPU with 2+ cores)
- True simultaneous execution, not context switching
- Tasks may or may not share memory (depends on implementation)
- Speedup is proportional to the number of cores (with caveats)

```
Computer 1 Core 1 ─── Task A ─────────────────>

Computer 1 Core 2 ─── Task B ──────────>

Computer 2 Core 3 ─── Task C ──────────────>
```

### Pros

1. **Performance**: True speedup from simultaneous execution on multiple cores.
2. **Throughput**: Can process multiple CPU-intensive tasks at the same time.
3. **Scalability**: Adding more cores increases performance (up to a point).
4. **No context switching overhead**: Unlike concurrency, no OS switching between tasks.

### Cons

1. **Requires multi-core hardware**: Doesn't help on single-core systems.
2. **Shared memory complexity**: If tasks share memory, synchronization is critical.
3. **False sharing**: Multiple tasks on different cores accessing nearby memory locations can degrade performance.
4. **Not ideal for I/O-bound tasks**: Cores sit idle waiting for I/O; concurrent model is better.
5. **Difficult to debug**: Race conditions and ordering issues are as complex as concurrency.

## Distributed Execution

Distributed execution means tasks execute on **different machines**, coordinating over a **network**. Each machine may run sequentially, concurrently, or in parallel locally, but globally the system is distributed.

- Tasks run on **separate machines**, not shared cores or memory
- Communication happens over a **network** (HTTP, TCP, message queues, etc.)
- Each machine runs its own execution model (sequential, concurrent, or parallel)
- **Network latency** and **network failures** are fundamental constraints
- **No shared memory**: Tasks communicate via message passing
- **Partial failures**: One machine can fail while others continue

### Pros

1. **Scalability**: Add more machines to handle more work (horizontal scaling).
2. **Fault tolerance**: If one machine fails, others continue (depending on design).
3. **Geographic distribution**: Process data closer to where it's needed.
4. **Independent evolution**: Machines can be updated independently.
5. **Resource isolation**: No one machine can consume resources from another.

### Cons

1. **Network latency**: Communication is slow (milliseconds vs microseconds).
2. **Network failures**: Messages can be lost, delayed, or duplicated.
3. **Partial failure ambiguity**: You can't tell if a machine crashed or if the network is slow.
4. **Ordering complexity**: Without shared memory, establishing order requires consensus (expensive).
5. **Consistency challenges**: Copies of data can diverge; strong consistency is costly.
6. **Debugging difficulty**: Bugs depend on timing, network conditions, and machine state.
7. **Operational complexity**: More machines, more things that can fail.


## Comparison Table

| Aspect | Sequential | Concurrent | Parallel | Distributed |
|--------|-----------|-----------|----------|------------|
| **Tasks executing** | 1 | Many (interleaved) | Many (simultaneous) | Many (on different machines) |
| **Cores/machines needed** | 1 core | 1+ cores | 2+ cores | 2+ machines |
| **I/O efficiency** | Poor (CPU idle) | Excellent (switch to other tasks) | Good (tasks on different cores) | Good (tasks on different machines) |
| **CPU throughput** | Low | Depends (I/O-bound: high, CPU-bound: low) | High (CPU-bound) | Medium (network overhead) |
| **Shared resources** | N/A | Yes (memory) | Yes (memory) | No (message passing) |
| **Synchronization needed** | No | Yes (locks, mutexes) | Yes (locks, mutexes) | Yes (consensus, quorum) |
| **Deterministic** | Yes | No (non-deterministic) | No (non-deterministic) | No (non-deterministic) |
| **Debugging difficulty** | Easy | Hard | Hard | Very hard |
| **Simplicity** | Very simple | Complex | Complex | Very complex |
| **Scalability** | Single core limit | Limited to one machine | Limited to cores on one machine | Unlimited (add machines) |
| **Fault tolerance** | None (single point of failure) | Limited (machine failure = total loss) | Limited (machine failure = total loss) | High (depends on design) |
| **Network involved** | No | No | No | Yes (latency, failures) |

## Decision Guide: When to Use Each Model

### Use **Sequential** if:
- ✓ Simplicity is more important than performance
- ✓ The workload is single-threaded and low-volume
- ✓ You're building a learning project or proof-of-concept
- ✓ Debugging ease is critical

### Use **Concurrent** if:
- ✓ Many I/O-bound operations (network, disk, database calls)
- ✓ Need to handle many simultaneous requests (web server, API)
- ✓ Single machine is sufficient
- ✓ Responsiveness is important (UI, interactive systems)

### Use **Parallel** if:
- ✓ CPU-intensive operations (heavy computation)
- ✓ Multi-core CPU is available
- ✓ Tasks don't depend on network or external I/O
- ✓ Maximum throughput is the goal

### Use **Distributed** if:
- ✓ Need to scale beyond a single machine
- ✓ Geographic distribution is required (serve users globally, data locality)
- ✓ Fault tolerance is critical (one machine failure shouldn't break the system)
- ✓ Data volume or processing load exceeds one machine's capacity
- ⚠️ Accept increased complexity and operational overhead

### Hybrid Approaches

In practice, systems often combine models:
- **Concurrent + Parallel**: A web server with multiple threads on a multi-core CPU
- **Parallel + Distributed**: Distributed systems where each node does parallel processing
- **All three**: A cloud platform with distributed machines, each running concurrent services on parallel cores

## Key Takeaways

1. **Sequential**: Simple but slow for I/O or multiple tasks.
2. **Concurrent**: Fast for I/O-bound workloads; adds complexity.
3. **Parallel**: Fast for CPU-bound workloads; requires multi-core.
4. **Distributed**: Unlimited scalability; highest complexity.
5. **All non-sequential models share the same fundamental problem**: indeterminism, race conditions, and need for synchronization.
6. **Choose based on your constraints**: hardware, workload type, scale, fault tolerance, and acceptable complexity.
