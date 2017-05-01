# FP in the small vs FP in the large

Immutable Data, infrastructure
Referential Transparency
Functional composition

Embrace change!
  - immutable change, map don't mutate
    - but this is expensive
    - caching, updating a cache
  - Are your forcing things? too dogmatic of application?

Conts vs var -> But should/does it extend to Architecture? Mutable data stores...
  - FP does not include immutable data stores
  - Data always changing
  - How many globals do you want in your program?
  - How many Bullet holes do you want?
    - context dependent?

FP is a tool

Break dogma or "cheat" (just this once)?
  - make a choice, don't let it happen
  - Consider cost/benefit

Decide on dogma/guidance, allow exceptions
  - But somethings cant be given up
      e.g. Blockchain - make it mutable changes the benefits
  - There are some things you can't change because it breaks the Purpose

Architectural styles have principles
  - if you change it too much ... pick a different styles

Are there gotchas in Architecture when using FP?
  - principles become dogmas - why?
  - Expert Beginners - need more guidance/recipies/dogmas

Why stick to dogma?
  - enhance reasoning, fewer exceptions
  - Don't add more complexity to the system

Modules - Bucket of functions at Design time

Data flow diagram - clean mapping to FP
  - works w/ OO too
  - Applies to all systems in the broadest sense - something in, something out
  - Working in the problem space
  - Just a view of Data and responsibility, applies to any styles

Never will have a "pure functional" architecture
  - larger systems will allways mix paradigms / styles

Same ideas, patterns at play - Different focus on concerns

Functional ideas learned in the small, apply in the large, but always blended w/ other patterns

Microservice provides a result, How it does it is not important
  - Assumes stateless
  - "Functional microservices"
  - Fault tolerence
    - state w/in microservice, no shared state
    - one thing crashing does not kill system
      - Isolation
    - Transform, don't mutate

No such thing as Functional architecture?
  - Functional tactics
  - F.P. cannto be applied universially

Function vs Module vs Objects (Objects are identity/state/behavior, Models are behavior)
  - Encapsulation
  - Reasoning

Thinking about responsibilities - what goes in comes out
