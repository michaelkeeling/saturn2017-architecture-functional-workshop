# Quality attributes at play

It's hard to talk about FP and QAs, but it is possible to look at specific ideas within FP, so we looked at Immutable Data, Referential Transparency, and Functional Composition.  We then brainstormed some quality attributes that were promoted and inhibited.

- Immutable data
  - Scalability (promotes)
  - Auditability (promotes)
  - Integrity (promotes)
  - Performance (promotes)
  - Confidentiality (inhibits)
  - Security (inhibits)
  - Affordability (inhibits)
- Referential Transparency
  - Reliability (promotes)
  - Performance by way of caching (promotes)
  - Predictability (promotes)
  - Maintainability (neutral)
  - Affordability (promotes)
  - Testability (promotes)
  - Scalability (inhibits)
- Functional composition
  - Extensibility (promotes)
  - Modifiability (promotes)
  - Adaptability (promotes)
  - Security (inihibits)
  - Performance (neutral)
  - Predictability (neutral)

Below here are raw notes that led to the above

Promotes:  
 - Scalability, with immutability as a driver for concurrency, multicore, cloud compute
 - Integrity, with data, business rules, immutability
 - Modifiability, with mutability is clearly expressed
 
Inhibits:
- Modifiability, with need new tactics to dal with immutability, must support event forever

Do more with less
- FP has less code
- Less to maintain
- more knowledge required to read / understand
- less risk of introducing defects

What improves readability?
- Referential transparency (same input --> same output, easier to test, reason, predictability
- Caching opportunities

Performance
- What about state?  How big are the books?  Above a certain point, inhibits performance due to moving state around.
- Maybe a tradeoff, because we get predictability and scalability.
- Better not to tstore state
- Easy to run out of resources

Data separate from functionality
  - Data does not change
  - How you interpret the data may change
  - Assumes the "core domain" is stable over time, let the "edge" change (e.g Reports)

Where do we use F.P?
  - Event Sourcing
  - UI -> decorated view model
  - Map/reduce
  - pipe + filter
  - works best when not "pure"

Reliability
  - functions w/o side-effects vs take advantage of side effects ... leaky abstractions?
  - Immutable infrastructure - e.g. deploy container on Kubernetes
  - confidence increases

functional != functions

How to get those properties?
  - It's complicated...
  - Deductive vs. Inductive thinking

Affordability

Functional APIs - functional composition

Security
  - Auditability improved through immutability, Predictability
  - Immutability inhibits security, e.g PII
      Replay and surgically remove \_map to a new stream
        - pure functional hurts availability during map
