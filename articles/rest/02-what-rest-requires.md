# What REST Requires

![Static Badge](https://img.shields.io/badge/date-2026-blue)

![Static Badge](https://img.shields.io/badge/software--architecture-purple)
![Static Badge](https://img.shields.io/badge/rest-purple)

<p align="center">
  <img src="02-what-rest-requires.png"
       alt="Introduction to REST cover"
       width="600">
</p>

[REST](01-introduction-to-rest.md) defines five mandatory architectural constraints:

- **Client–Server**
- **Stateless**
- **Cacheable**
- **Layered System**
- **Uniform Interface** (which is actually a set of constraints itself: resource identification, representations, self-descriptive messages, and HATEOAS)

It also includes one optional architectural constraint:

- **Code on Demand**

## Client / Server

Separation of concerns between **clients** (User Interface, User interaction, Client State and Platform-specific concerns) and **servers** (Business logic, Data Storage, Data consistency and Scalability concerns) through  a well-defined **interface**.

### Benefits

This separation increases portability of the client across platforms, improves server scalability, and allows clients and servers to evolve independently as long as the interface remains stable.

### Trade-off

Additional network communication compared to in-process calls.

## Stateless

Each request from a client must contain all the information needed to understand and process it. The server does **not** store any session or client context from previous requests.

### Benefits

This enables **visibility** (intermediaries can understand and monitor requests), improves **reliability** and **fault tolerance** (making it easier to recover from partial failures), and supports **scalability** through simple horizontal replication.

### Trade-off

Higher per-request overhead.

> Note: **Cookies** are compatible with statelessness if they store all needed state on the client. They break statelessness when used only as a session ID pointing to server-side state.

## Cacheable

Server responses must be explicitly or implicitly labeled as **cacheable** (a client or intermediary has the right to reuse that response for subsequent identical requests) or **non-cacheable**.

### Benefits

Improves **network efficiency**, decreases **server load**, and enhances **scalability** and **performance**.

### Trade-off

Balancing **data freshness** versus reliability; designing proper **cache-control semantics** adds complexity.

## Layered System

Components are arranged in hierarchical layers where each layer only sees the next layer it interacts with (i.e., a component does not need to know whether it is talking to the origin server, a proxy, cache, or other intermediary) and is unaware of the system’s overall topology.

Typical layers:

- Client initiates requests, unaware of intermediate layers
- Security Gateway handles authentication, authorization, rate limiting
- Cache / CDN reduces latency
- Load Balancer distributes requests across multiple servers
- Origin Server processes requests

### Benefits

This architecture allows introducing **intermediaries** such as gateways, proxies, load balancers, and shared caches, which enhance **scalability**, **manageability**, and help define and enforce organizational boundaries.

### Trade-off

Might introduce **overhead** from extra hops, add **operational complexity**, and obscure end-to-end behavior, making **debugging and optimization harder**.

## Uniform Interface

A single, consistent **contract** between components:

- [Resource Identification](03-rest-key-concepts.md#resource-identification)
- Manipulation of resources via [Representations](03-rest-key-concepts.md#representations)
- [Self-descriptive messages](03-rest-key-concepts.md#self-descriptive-messages)
- [Hypermedia As The Engine Of Application State](03-rest-key-concepts.md#hateoas) (HATEOAS)

### Benefits

A uniform interface simplifies the architecture, enhances **interoperability**, decouples components, and allows independent evolution without breaking compatibility. Interactions are easier to understand.

### Trade-off

**Standardization** can reduce efficiency and limit flexibility compared to **custom, domain-specific interfaces**.

## Code on Demand (Optional)

Servers may extend component functionality by transferring **executable code** (typically scripts) that the client can run to modify or extend its behavior.

### Benefits

Supports **dynamic functionality**, induces **extensibility**, allows client-side features to be updated without deployment, and can help **server scalability** by off-loading work to the client.

### Trade-off

Reduces **visibility** to intermediaries because the server is sending code instead of simple data. This is why this constraint is **optional** in REST.
