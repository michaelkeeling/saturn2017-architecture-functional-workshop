# A Few Questions about the Functional Frontier

Position paper for SATURN 2017 Architecture and the Functional Frontier Workshop

[Michael Keeling](http://neverletdown.net), [@michaelkeeling](https://twitter.com/michaelkeeling)

## Position for Workshop

I had initially started this position paper with a quite strong
statement about what _functional architecture_ means to me.  As I re-read that
statement I realized that I really don't have a clue.  Instead of
making a bold assertion that I can't back up, I will instead ask some
questions that I think might be interesting to discuss during the workshop.

* What do we really mean by _functional architecture_?  Is it a collection of patterns,
  tactics, a design philosophy, all of these things?
* What quality attributes do we care about when we choose functional architecture
  patterns / tactics / philosophy?
* What quality attributes are in tension with functional architecture?
* What properties of a design are required to call something _functional_?  How many
  of these properties are required (or what combinations) to achieve the quality
  attributes promised by functional architecture?
* What existing patterns and tactics might fall under this umbrella (see brainstorm below).
* When is state considered _functional_ and when is it not?
* Can we come up with a better name than _functional architecture_?


## Brainstorm of Patterns

The following patterns come to mind as potentially being functional in nature
and more architecturally focused.

* Event Sourcing
* Immutable Infrastructure
* Map / Reduce
* Pipe and filter, at least under certain assumptions
* REST / Hypermedia
  * This might be a stretch, but a truly stateless self
    describing, resource-oriented, URI-based API seems
    in line with the as-yet-to-be-officially-defined functional
    constraints.
* Stateless microservices
* Lambda / Function as a service
* [12-Factor Apps](https://12factor.net/)
  * While not described on the website, the idea that the code we
    write is deterministically transformed into a configurable,
    stateless, deployable unit seems quite functional in nature.
* Several of the message transfer 


### Patterns and such that do not seem functional in nature

Sometimes it's useful to describe things that are outside of scope to get a
better understanding of what is in scope.  The following architectural styles
do not seem within scope of functional architecture.

* Layers
* N-Tier
* Publish-Subscribe, Event Bus
* General Service Oriented Architecture
* Call-Return
* Peer-to-Peer
* Client-Server
* Many more patterns than I want to enumerate here...

It seems that most architectural styles do not seem particularly _functional_.
Styles might be too general for this purpose.  Styles provide vocabulary and
can be implemented in a variety of ways.  To me this seems to say that
functional architecture makes assumptions about the world more specific than a
style.

In retrospect, and this may be obvious, but it also seems that _functional
architecture_ can never apply to module patterns.  I could come up with
several component and connector, allocation patterns that seemed functional
in nature.  Design time concerns --- specifically how code is organized ---
does not seem to fit the bill.

After further refelction, there may be interesting opportunities for describing
module relations using mondads.  The one example that comes to mind is a
_maybe allowed to use_ relation.  How is this relation used?  One practical
(albeit simple) example might be to describe dependancy injection at design time.
Perhaps an even more general example is to take a functional view on refinement.

An interesting implication of this conclusion is to see how far we can talk
functional allocation patterns.  Is _work assignment_ a transformation of idea
and labor to value with code as the output of the function?  This may be getting
too abstract, but if we take a functional philosophy to it's logical conclusion I
think we may find many opportunities to apply lessons from functional programming
to all aspects of design, not only C&C patterns.


## Architects on their way to a Workshop at SATURN...

![](https://upload.wikimedia.org/wikipedia/commons/4/41/The_Oregon_Trail.jpg)
