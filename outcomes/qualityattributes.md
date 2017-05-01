# Quality attributes at play

FP
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
