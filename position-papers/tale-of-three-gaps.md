Saturn 2017 - Functional architecture workshop Position paper

 * [Einar Landre](https://www.statoil.com) | [@em_landre](https://twitter.com/em_landre)
 * [Jørn Ølmheim](http://olmheim.com) | [@joelmheim](https://twitter.com/joelmheim)
 * [Harald Wesenberg](https://www.statoil.com) | [@hwes](https://twitter.com/hwes)

# Tale of three gaps

Once upon a time the Great architect had made his most beautiful functional decomposition of a problem into systems, subsystems and subsystems-sub systems. Never in the history of UML, had such beauty seen the light of day.

Satisfied as he was with his creation he brought it the to the Master programmer for realization. The Master programmer, proud of his merits stated yes, I can do this. You will never find a more beautiful realization. I will call you and show you in some weeks. I will transform your blueprint into the most beautiful assembly of objects and functions.

Weeks and even months passed, but nothing was delivered. Finally, the architect went to the Master programmer and asked: why does it take so long? You promised it in weeks, now months has passed? The Master programmer broke down and cried: "Mr. Database forced me to transform my objects and functions into a network of global variables, all normalized to the fifth form. Now my code is broken and I’m not able to find a way out. When I change something here, something over there breaks. I am a failure; my reputation is ruined …"

So, what’s the story told here?

We believe, or to be more precise we are convinced that architects think in functional concepts, not just functionality, when they decompose a problem into systems and subsystems and even subsystems-subsystems.

Programmers easily map functional modules into source code packages, interfaces and services to be offered by the system, and which functions and classes are going to reside inside each package. The best of them master domain driven design, and sound design principles such as immutable functions, Single Responsibility Principle, and Liskov´s Substitution Principle and all are happy until they meet the relational database.

Our claim is that the relational database, if used improperly, is nothing more than a network of global variables, and that network is filled up with state changes where effects propagates from one end of the system to the other. Its underpinning philosophy of building a network of variables kills all thinking in functions and objects.

The effect is that we have a destructive loop with three gaps:

Architect -> Programmer -> Database -> Architect.

The effect is that both architects and programmers capsize in the meeting with the database and its evil power: “One database to find them, one database to bring them all and in the darkness bind them”.

So all we really have here is a functional antipattern: The Smart Database (or Central Database). We like Smart Database because that also describes the tendency to put logic in the form of stored procedures in the database. This is the database used as shared global state.

Microservices with private databases help here. So does NOSQL databases as these do not contain that many relationships – read dependencies. We also believe that containers and container orchestration will be central to how we think about architecture going forwards, and that this will help.

Now about the name. We have a feeling that "Functional architecture" is the best we can do for now. We did consider "Function-oriented architecture" for a hot minute, but we decided that it was neither more precise or more descriptive.

Also there is a question about the quality attributes, or more precisely what the effects of these functional concepts on the quality attributes. There is no denying that focusing on functional concepts like the immutability, idempotency and composability will lead to a certain set of system qualities, and they will end up pushing the quality attributes in a certain direction. Driving the architecture with these concepts will lead to a highly modular, loosely coupled, distributed and scalable system, so does that mean that functional architecture is only suitable for certain types of systems?

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

DDD: Domain-Driven Design
GOF: Design Patterns, Gang of Four
POA4: Pattern-Oriented Software Architecture Vol 4: A Pattern Language for Distributed Computing
