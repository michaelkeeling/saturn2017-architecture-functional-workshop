# Functional Programming and Software Architecture

Sebastian von Conrad ([@vonconrad](https://twitter.com/vonconrad))
April, 2017

## Introduction

When I interviewed for my current role as Software Architect, I was asked to create a Venn diagram of Software Architecture and Software Development. The idea was to understand my view of what was Architecture, what was Development, and what the overlap between the two was.

At the time, I put the concept of "programming languages" squarely in the Software Development circle. It did not have anything to do with Software Architecture, I thought. And I further thought that because the programming _paradigm_ (OO, functional, imperative, etc) was largely a characteristic of whatever programming _language_ you used, it also had nothing to do with Software Architecture.

Some time has passed since then, and now I'm not so sure. In this paper, I'll talk through a couple of examples of how Functional Programming (FP) concepts might impact Software Architecture.

## FP Concepts and Software Architecture

### Immutability

I believe that **immutability enables concurrency**.

George Fairbanks argues in his [position paper](functional-programming-invades-architecture-george-fairbanks.md) that immutability is slower than mutable state. Far be it from me to disagree with George on anything (except he appears to prefer double spaces after periods--_shudder_) and in this case I do think he's right, but only for a single thread.

I don't think the correlation between immutability and concurrency is controversial. Shared mutable state often suffers from race conditions and other issues. In today's world of cloud computing and elastic scaling, I believe immutability allows us to take better advantage of the hardware that is available and achieve better performance overall. If we can achieve seamless horizontal scaling, it will beat vertical scaling every time. Immutability is key to this.

FP is inherently immutable, immutability greatly influences scalability, scalability is a very important quality attribute.

> As a side note, who would have thought it would be the _JavaScript community_ that got excited about and started the mainstream immutability trend? I find it kind of incredible.

### Referential transparency

I believe that **referential transparency enables caching**.

When an expression can be replaced by its output without changing a program's behaviour, it opens up opportunities. The output of expensive functions can be memoised without the need to implement elaborate cache invalidation strategies. Better parallelisation can achieved as well.

Caching and parallelisation are, of course, well-established strategies for improving performance and scalability of systems.

### Idempotence

I believe that **idempotence enables asynchronicity**.

When a system, component, or function is idempotent, it can receive a message multiple times while only executing the desired functionality once. This, in turn, means that we can implement `at-least-once` messaging without fear. In a world where it is difficult to guarantee `exactly-once` messaging between systems, this is really useful. `at-least-once` messaging without fear of multiple executions, in turn, makes it easier to implement asynchronous communication.

A friend of mine recently said: "if your microservices rely on making synchronous http calls to each other, you actually just have a distributed monolith." This is a really interesting line of thought that's provoked a lot of conversation. With temporal coupling between services, a service relies on its dependencies in order for it to function properly. If one service goes down--particularly near the bottom of the dependency chain--it can have cascading and potentially devastating consequences.

If we default to asynchronous communication and `at-least-once` messaging, on the other hand, we can avoid temporal coupling. We can delay execution of functionally, if necessary, and/or gracefully degrade the experience for users. Our system as a whole becomes more resilient.

### Event Sourcing

I believe that **Event Sourcing enables integrity**.

I didn't intend to talk about Event Sourcing in this position paper, but seeing how others have mentioned it in their position papers--particularly in how it's related to immutability--I think I should address it.

At the heart of the Event Sourcing is just an idea: an append-only stream of events that act as the source of truth for your system. If your source of truth is immutable, you can protect it from improper alterations and guarantee its consistency. At minimum, being able to revoke any `UPDATE` or `DELETE` privileges on the application database is great. But we can take it even further, using `write-once-read-many` media to prevent malicious use.

Now, it's true that Event Sourcing is based on the fundamental principles of immutability. It's also true that the functional programming paradigm caters exceptionally well to event sourcing. Greg Young, who many consider the "father of Event Sourcing," argues that Event Sourcing is effectively nothing but a `left-fold`. I agree with him. This can be inherently described as a functional concept.

But it's not the only way to do it, and that's important to acknowledge. I would discourage the notion that Event Sourcing is coupled to the functional paradigm, because I don't believe that's true. The technical solution to having append-only event stream as source of truth (be it functional or not) is an implementation detail. This is why I initially didn't intend to address Event Sourcing in this paper.

My own experience with Event Sourcing is mostly OO, but some fundamental notions of FP--such as immutability-as-first-class-citizen and pure functions--make me very interested in continuing my recent pursuit of building event sourced systems in the functional paradigm. I have some experience with this, and I think it can address a number of issues I've observed while doing Event Sourcing over the last few years.

### Pure functions

I believe that **pure functions enable _a whole bunch of things_**.

Pure functions are often, but not always, idempotent. They are always referentially transparent. As a result, the benefits that those concepts have, such as memoisation and parallelisation, also often apply to pure functions.

In addition to that, though, pure functions are deterministic. Determinism leads to better predictability. Determinism also make things easier to reason about, which I think has an positive impact on maintainability.

Pure functions are also stateless in that they rely only on their input. Like immutability, this greatly enables concurrency and scalability.

Other benefits of pure functions, such as composability and ease of testing, can also have an impact on the maintainability and evolvability of a system. I think this falls somewhat closer to the Software Development side of the spectrum, though, so it may not be a clear Software Architecture concern.

## Conclusion

I believe there are a lot of concepts in FP that influence Software Architecture, however it's important to remember that there are always other ways to achieve the desired quality attributes. Many of the concepts I've discussed (immutability, idempotence, etc) are not unique to FP. It's possible to achieve them in other programming paradigms as well, although often at a cost. FP, however, make these simple to achieve. They are often first class concerns. We get them for "free" (though nothing is ever free).

As an architect, I often encourage the teams I work with to build systems and services with FP. I feel FP is a good way to achieve many desirable quality attributes at lower relative cost. That, to me, is reason enough to put place programming paradigms in the overlap of the Venn diagram of Software Development and Software Architecture.
