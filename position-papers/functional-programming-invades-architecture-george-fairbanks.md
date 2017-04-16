# Functional Programming In Software Architecture
Position paper for SATURN 2017 Architecture and the Functional Frontier Workshop

George Fairbanks
16 April 2017

# Abstract

Functional programming (FP) has been around for a while but has largely been treated as a concern inside a module, a Programming in the Small concern [1], rather than an architectural concern.  The collection of ideas within FP are also found in other places and many originate in mathematics.  So it is not surprising that those ideas at times were used in software architectures.  It is interesting to see how many ideas are now found in both architecture and FP, often due to the challenges of reasoning about and building distributed systems.

# How I became interested in Functional Programming

Perhaps, long ago, you learned that Functional Programming (FP) existed and for some reason chose to treat it as a curiosity that was not relevant to your interests.  That is what I have been doing for a few decades.  What I was exposed to didn’t seem like a good fit for the kinds of problems I worked on.  I was happy enough with the imperative and object-oriented languages I had.  I didn’t recognize the connection between the formality I was using in object oriented analysis (methods specified using pre/postconditions, precise models, and invariants) and declarative functions in FP.  I even thought that FP was doomed to failure because it required more consistency in the world than was really there in the business systems I built, with the incoherency of their requirements that could not be refactored to suit my sensibilities or to make my code more beautiful.  Plus there were no large-scale successful FP projects that drew my attention or forced me to re-evaluate my opinions.  I saw only programming language academics writing languages that were good at writing compilers for programming languages.

But FP won on micro-problems like iterating over a collection, filtering it, and transforming it.  I didn’t realize that the collection operators I was so fond of in Smalltalk (do, collect, select, reject) were commonplace in functional languages.  I think most programmers, once they are exposed to this style, never want to go back to raw FOR loops with i++ and termination conditions.  Consider the following code snippets that your coworkers may have written.

```Java
boats.forEach(boat -> boat.sail());
```
```Java
for (i = 0; i < boats.length(); i++) {
  boats[i].sail();
}
```

There’s an argument that the first is better because it’s more compact, but I believe people really like it better because it’s easier to reason about.  Did you have to double check if the termination condition in the second example should be “less than” or “less than or equal”?  Did you consider if the boat collection’s index starts with zero or one?  You probably did and if your company requires peer code reviews then you definitely did -- and it took you longer to be sure it was right.  It’s just easier to reason about the first version, easier to convince yourself that it works as intended.

Once you realize you don’t want to go back to the raw FOR loops, you become curious about what may be ahead.  Maybe some of the other academic formal crackpot stuff in FP has value.  That’s how my interest was piqued.  

But I’m also interested in software architecture, an topic that doesn’t seem to come up in most discussions of functional programming, as those papers tend to focus on a single program running on a single machine.  A common architecture concern is whether a bunch of code will behave as expected and exhibit the qualities expected.  This is the same question as above, just scaled up arbitrarily, and it’s notoriously difficult to reason about.  So perhaps there are ideas from FP make architecture concerns easier to reason about.  It seems worthwhile to identify what ideas from FP are being used at the scope that we’d call “architecture” or Programming in the Large. 

# FP ideas that carry over to architecture

When researching this paper, I was a bit surprised to learn that there is no canonical list of FP ideas.  For example, some consider lazy evaluation and pure functions to be central while others build FP languages without them.  A canonical list would be a convenient starting point but we shouldn’t expect all of the ideas to also show up in architecture.  That said, here’s my list of ideas that I recognize both in FP and in software architecture.

## Delcarativeness (not imperative)

Functional programming encourages stating what something *is* or how it relates to other things, rather than a series of operations that (may) get you to a desired state.  Like the forEach loop above, this makes it easier to reason about correctness.  If you look at the code and disagree with the declarative statements then you probably disagree with the outcome.  In contrast, you may need to read a lot of imperative code before a feeling emerges that it is on the wrong track.  

On the other hand, declarativeness works when you can safely ignore what implements it, but is a debugging nightmare otherwise.  The shared libraries of FP languages are polished clockwork so it’s rare to find bugs but that may not be the case at the scope of architecture.

Declarativeness has appeared in software architectures for a long time.  Perhaps the most common example is the SQL interface between Information Technology (IT) systems and relational databases.  Indeed, that one technology (with the corresponding query optimizer) has reaped huge dividends for IT programming because it largely allows data to be persisted today without worrying about access patterns tomorrow.  This is in sharp contrast to NoSQL systems that yield scalability only when access patterns are known in advance.

## Function composition

Functions that are understood in isolation can be combined and still be understood.  All large programs are composed of smaller chunks but, since the composition is not simple or regular, it can be hard to have confidence that it works as intended.  In FP, reasoning is easier because the composition operations are simple and regular.

Consider Unix pipes and filters.  Command-line users compose small programs without great effort because the composition is simple and there are few things to consider (i.e., the main stream and exception streams).  The graphs used to train neural networks have the same characteristic, as do the operators in reactive systems [2].  

Event-based systems do not, in my experience, behave this way.  Small ones are easy to reason about but larger ones may lose the simplicity and consistency that is desirable, making it hard to reason about the outcome unless you have scrutinized every path and condition in the event graph.

## Pure functions (no side effects)

When a function has no side effects, it’s easier to reason about what it does, in part because you can exclude from your thinking what it *doesn’t* do.  If it’s a function from its inputs to its outputs, that’s all you have to think about, not whether it also changes something in the environment.  Being dogmatic about this leads to difficulties when you specifically want side effects, like the console should now say “Hello, world!”

Pure functions can also be executed as many times as desired and yield the same output every time.  That’s inconvenient if you want to build a counter (which would need the current count changed as a side effect) but great if you have an addition service that depends only on its inputs.

It’s increasingly common to write servers that are themselves stateless.  These resemble pure functions in that they receive input messages and send output messages, with no change to their internal state (as there usually is none).  These servers often do have side effects in persistent storage, however, so calling the server with the same inputs repeatedly may yield different results.

A related point here is how we can compare the outputs.  If I have two outputs named “John Smith”, are they the same person?  In procedural and object-oriented programs, which can have a record or object with the data “John Smith”, generally those are considered two different records/objects and the programmer must decide to take the policy that identical fields means identical (i.e., write code for .equals(other)).  At an architectural scale, this is a bigger issue since it’s not trivial to decide if the “John Smith” seen in various databases, pipelines, user interfaces, etc. are all the same unless there is some universal key that identifies him.  There are patterns to handle this, but it’s not as easy as in a pure FP language.

Definition, not procedure.  Can convert code into definitions.  Example: how much paint do I need?  Function parameterized by length and width.

## Immutability

The idea with immutability is: if you must have state, keep it static.  That’s easier to reason about (and allow concurrent access to) than mutating state in-place.  It’s also slower, but given the abundance of RAM and CPU these days, we often prefer slower-but-accurate to faster-but-maybe-wrong.

The speed penalty of immutability is easier to accommodate when data sizes are smaller, which they are when programming in the small.  But the accuracy needs when programming in the large are making immutability seem less crazy every day.  Event sourcing and write-only datastores are patterns that are increasingly common.

## Idempotence

Idempotence, as it applies to software design, usually means being able to do the same thing multiple times and getting the same result.  Imagine an operation that shrinks a picture to half size.  That operation isn’t idempotent if it picks up the picture, shrinks it, and writes it back to the same file, because if that’s done twice you get something a quarter the size.  However, if the operation always reads from file A and writes to file B, you can do it twice and file B will still be half size.

Idempotence is a mathematical idea that I haven’t seen come up often in FP in the small but is used everywhere in the design of large scale distributed systems.  With failures inherent in such systems, it’s hard to be sure that scheduled work gets done exactly once.  Work is often parallelized across many machines, some of which will fail and some of which will appear to fail but actually just be going slower than expected.  In these conditions, it’s often easier to just do the work multiple times and rely upon it being idempotent.  

Patterns for idempotence are also helpful with object creation.  If a message like “Add John Smith to the DB” is sent twice (say because of a timeout waiting for a reply), you can have two records in the DB.  But if both messages share a unique identifier, the server can tell that they are related and only execute the request once.

# Where we see these ideas in architectures
The last section was a list of FP ideas that I’ve noticed used in architecture.  It’s also helpful to take a look at architectures and see what FP ideas are being used.  The descriptions here focus primarily on runtime architecture views.  The next section on DevOps infrastructure discusses deployment views.

## Client-side

I am not the only one who dislikes the current state of web development, especially writing code for in the web browser.  Current browser-side tech can be particularly maddening because of the competitive forces that affect browser standards.  FP languages and ideas can be a net-win even after adding the problems of cross-compiling to Javascript (JS) and learning a new language.  I’m no expert in this but I’ve dabbled.  

Functional patterns pervade JS development.  Several of the most popular frameworks use FP ideas at their core, including ReactiveX and Redux [4].  The patterns popularized in object-oriented programming like Model-View-Controller and variants are falling from favor.  My guess is there are a number of factors pushing this that include: the unstable world of shifting JS libraries, the simultaneous push and pull of needing to use external libraries and the immature tech for safely integrating them, and the language features of JS itself.

I found a helpful comparison of several architectural patterns used in modern web application development [3].  The focus was on “uni-directional” patterns that comprise a stateless path that reacts to events, ending in “render this” on the screen.  This paper was an “aha” moment for me, indicating that something was really changing and it wasn’t just a language preference.

## Server-side

“On the server” is a pretty broad characterization and not entirely accurate as not all systems are client-serve, but it’s a convenient bucket to group some ideas.  Above, we discussed so-called “serverless” pure functions that are high on the hype-cycle right now [5], but represent an interesting set of architectural patterns that are possible because running a big application in a browser is now possible and because cross-cutting services like persistence and security have become commoditized.

Reactive programming [6] is perhaps easiest to understand as the Observer pattern with two additional messages added as out-of-band signaling: onError and onCompletion.  There is a rich set of primitives to transform one Observable stream into another [7], rich enough that many simple tasks can be done just by composing the existing set.  

Note to attendees:  There is a lot of related work and terminology in this space (eg Event-based systems, concurrency models like the Actor model, finite state automata, and communicating sequential processes) and I would appreciate pointers to comparisons that help me relate them.

One of the tricks that large-scale web systems have been using is to make servers simple and push the work to be done into bite-sized chunks in event / work queues.  That way, if one task needs more computing resources, you can turn on more servers to work through the backlog (or just to reduce latency), and servers can be provisioned with resources suitable to the task (eg lots of RAM).  These patterns are not always functional, but they often are.

Both batch sequential & pipeline architectures have obvious ties to functional thinking.  “Big data” processing often relies on one of these two patterns (eg MapReduce, Tensorflow) but other technologies are a hybrid (eg Spark), Map-Reduce, Tensorflow and other machine learning graph-based functional transformation.

## Persistence

Event sourcing is often used along with event queues in event-driven systems.  The idea is to store state as an ordered list of mutations, like a change log.  If you want to know the current state of, say, a customer then you replay the changes from the beginning.  Or said another way, the current state of the customer is the composition of applying that list of functions, which is a very functional idea.  Martin Fowler has a description of the Command Query Response Segregation (CQRS) pattern that nicely relates it to event sourcing and eventual consistency [8].

# DevOps infrastructure
DevOps patterns, where the roles of Developer and Operator are combined, is interesting to view from the perspective of FP ideas.  We have taken for granted for a long while that source code should be checked into version control repositories.  What might be surprising is to think of that repository as Event Sourcing, which each delta or patch of source code being an event that is applied to the previous state.  

DevOps starts with this idea and goes further:  What if we took Bob the Sysadmin’s job, made him one of the developers, and wrote scripts to automate how he deployed the software or configured the hardware?  Of course we’d check those scripts into version control, just like the source code for the application itself.  

But things get really interesting when we do fully automated deployments (and rollbacks).  We have always considered the system we run in production to “be a function of” the code we check into version control.  When that deployment and monitoring is truly hands-off for the developers then we can take the quotes away from that functional sentiment.  

Before automated DevOps, we had to ask Bob the Sysadmin what code was running, if he’d patched the operating system, and what parameters he’d tuned in the DB for better performance.  But with DevOps and all those scripts checked into version control, what we run in production is a function of just a few variables, including: the source code repository, test results, and signals from runtime monitoring. If you neglect the latter (for simplicity, or perhaps because you are reckless), then the function is easy to describe, something like “Our production system is three deployed instances of the last green build/test in the version control repository from two days ago”.

Immutability makes an appearance in DevOps too, in the form of “immutable infrastructure”.  The idea is that if you need to change the configuration in a deployed instance, you don’t change the instance, you kill it and start a new one.  

Note to attendees:  Before this workshop, Len Bass warned me that people have been saying things like this about several aspects of software engineering, such as “our production system is a function of the requirements”.  I’m interested in feedback on whether, as they say, “it’s different this time”.

# Conclusion

Modern software architectures (programming in the large) share many ideas with functional programming (programming in the small).  Surely some of these are due to good ideas being adopted where they are sensible.  But there also seems to be a larger trend towards FP ideas becoming mainstream.  As I discussed in the introduction of my book on software architecture, our systems are always getting bigger and the challenges scale up with them (perhaps out of proportion, even), so as developers we are on the lookout for abstractions that help us control the complexity.  Functional programming ideas have germinated for many years and are increasingly mainstream, not just for fringe academics.  

Today you can rent over a hundred machines on a cloud provider for less than the salary of one junior developer, so techniques that trade CPU and RAM for skilled developers are economically viable.  We can see this in mainstream testing practices, where no code submission happens before tests are run against it many times, perhaps even hundreds of times, starting with local tests on a developer’s machine and progressing through centralized regression tests.  If FP ideas can help us structure our problems such that our reasoning is more effective, even if they may require more computation resources, it is no surprise that they have invaded our software architectures.

# References

[1] DeRemer and Kron, Programming-in-the large versus programming-in-the-small, ACM SIGPLAN Notices,
Volume 10 Issue 6, June 1975.

[2] Andre Staltz, The introduction to Reactive Programming you've been missing, https://gist.github.com/staltz/868e7e9bc2a7b8c1f754, Aug 2015.

[3] Andre Staltz, Unidirectional User Interface Architectures, https://staltz.com/unidirectional-user-interface-architectures.html, Aug 2015.

[4] ThoughtWorks Radar, https://www.thoughtworks.com/radar/languages-and-frameworks, April 2017.

[5] Mike Roberts, Serverless Architectures, https://martinfowler.com/articles/serverless.html, August 2016.

[6] Wikipedia, Reactive Programming, https://en.wikipedia.org/wiki/Reactive_programming.

[7] RxMarbles, http://rxmarbles.com. 

[8] Martin Fowler, CQRS pattern, https://martinfowler.com/bliki/CQRS.html, July 2011.
