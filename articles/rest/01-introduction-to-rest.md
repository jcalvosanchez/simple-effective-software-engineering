# Introduction to REST

![Static Badge](https://img.shields.io/badge/date-2026-blue)

![Static Badge](https://img.shields.io/badge/software--architecture-purple)
![Static Badge](https://img.shields.io/badge/rest-purple)

![Cover Image](01-introduction-to-rest-cover-image.jpg)

## What is REST

**[REST](https://en.wikipedia.org/wiki/REST)** (**REpresentational State Transfer**) is a [Software Architecture style](https://en.wikipedia.org/wiki/Software_architecture#Architectural_styles_and_patterns), just like classic or gothic are architectural styles in building design, defined by Roy Fielding in his 2000 dissertation.

Rather than prescribing specific technologies or protocols, REST defines [a set of architectural constraints](02-what-rest-requires.md) (5 mandatory + 1 optional) to guide the evolution of **long-term and large distributed [hypermedia](https://en.wikipedia.org/wiki/Hypermedia) systems** such as the World Wide Web.

In a REST system, each request will be responded with the [representation](03-rest-key-concepts.md#representations) of a [resource](03-rest-key-concepts.md#resources) that will contain hypermedia **links** to other related resources and to make the **state of the system** change.

An important consequence is that the only [identifier](03-rest-key-concepts.md#resource-identification) that needs to be known is the identifier of the first resource requested, and all other identifiers will be discovered.

## When to use REST

These constraints are intended to produce distributed systems that supports **long-term evolution**

- [ ] Components can evolve independently
- [ ] Interoperability between components is required
- [ ] Resilience and fault tolerance are important
- [ ] Scalability is a concern

## Why to use REST

Today’s systems face the same pressures related to scalability, interoperability, uncertainty, and independent evolution, which makes REST’s core principles still highly relevant.

The **World Wide Web** remains REST’s most successful example: a system that has scaled globally, evolved continuously, and accommodated an enormous diversity of independently developed components over time.
