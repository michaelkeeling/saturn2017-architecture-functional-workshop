Saturn 2017 - Functional architecture workshop Position paper

 * [Einar Landre](https://www.statoil.com) | [@em_landre](https://twitter.com/em_landre)
 * [Jørn Ølmheim](http://olmheim.com) | [@joelmheim](https://twitter.com/joelmheim)
 * [Harald Wesenberg](https://www.statoil.com) | [@hwes](https://twitter.com/hwes)

# Tale of three gaps
Once upon a time the Great Architect had made his most beautiful functional decomposition of a problem into systems, subsystems and subsystems-sub systems. Never in the history of UML, had such beauty seen the light of day.

Satisfied with his master piece the Great Architect went to the Master Programmer for realization. The Master Programmer, proud of his merits stated yes, I can build this. I will transform your blueprint into the most beautiful assembly of services, objects and functions the world has ever seen.

Weeks and even months passed, but the only thing the Great architect heard from the Master Programmer was silence. Finally, he went to the Master Programmer and asked: why does it take so long? You promised it in weeks, now months has passed?

The Master Programmer broke down and cried: "Lord Database forced me to interface my objects and functions with a network of global variables, all normalized to the fifth form. My code is broken and I’m stuck in a maze. I am a failure; my reputation is ruined …"

### The anti-functional data model
We believe, or to be more precise we are convinced that architects are functional thinkers. Functional decomposition of both problem space and solution space is the primary tool of their trade.

We also believe that programmers are familiar with functional thinking. They have decomposed problems into functions and objects since they wrote their first serious program. Most of them have no problem mapping functional subsystems into services, packages and interfaces maximizing cohesion and minimizing the coupling in their code base. The best of them master domain driven design, sound design principles and patterns.

Our claim is that the inherent functional thinking found in architecture and programming (even though programmed in an imperative way) breaks in the meeting with the data model and particularly how data has been modeled in enterprise contexts.

Seen from a functional perspective, an enterprise data model boils down to a network of interconnected global variables; a network where state changes propagate from entity to entity in an uncontrolled manner.

Its underpinning philosophy is anti-functional. The data and their relationships form a different world view, a world view that make life hard for functional thinking architects.

What we really have here is a functional anti-pattern: The Smart Database (or Central Database). We like the notion of Smart Database because that also describes the tendency to put logic in the form of stored procedures into the database.

Logic primarily used to tackle state changes and manage data integrity rules, this is the logic that makes the database a placeholder for shared global state.

### Three gaps and the way forward
We started this as a tale of three gaps, the gaps that occur due to the impedance mismatch and world view held by the different roles or archetypes: The Architect -> the Programmer -> the Data modeler -> Architect. Our belief is that these gaps must be eliminated, and that the best way to do so is by adopting functional thinking from the beginning to the end.

Microservices with private persistent storage helps here. The same does NOSQL databases as these do not capture that many relationships – read data dependencies. We also believe that containers and container orchestration will be central to how we think about architecture going forwards, and that this also will help. Here we can look at multi-agent architectures and principles. These have been around for decades and are used with success in many deployed systems.

### Naming the baby
Now about the name. We have a feeling that "Functional architecture" is the best we can do for now. We did consider "Function-oriented architecture" for a hot minute, but we decided that it was neither more precise nor more descriptive.

There is also a question about the quality attributes, or more precisely what the effects of these functional concepts on the quality attributes. There is no denying that focusing on functional concepts like the immutability, idempotency and composability will lead to a certain set of system qualities, and they will end up pushing the quality attributes in a certain direction. Driving the architecture with these concepts will lead to a highly modular, loosely coupled, distributed and scalable system, so does that mean that functional architecture is only suitable for certain types of systems?

### Patterns
Patterns that embody functional concepts:
* All the strategic patterns from DDD
* Value Objects (DDD) - Immutable
* Prototype (GOF) - Supports immutability
* Composite (GOF) - Supports composability
* Decorator (GOF) - Dynamical extension, composability
* Iterator (GOF) - Supports core functional implementation
* Visitor (GOF) - Basically a map-like operation
* All patterns from POA4

POA4 is very much recommended reading for all software architects.

DDD: Domain-Driven Design<br/>
GOF: Design Patterns, Gang of Four<br/>
POA4: Pattern-Oriented Software Architecture Vol 4: A Pattern Language for Distributed Computing
