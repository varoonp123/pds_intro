# Tradeoffs

Just because persistent data structures are useful in many programs I write,
they are not a silver bullet to concurrency or anything. They are just another
tool.

## Pros
- Persistent data structures allow one to treat aggregate types as values. These
  data structures have relatively efficient recipes for creating new, slightly
  modified, versions of themselves without a deep copy. 
- Transformations compose. Mutations do not. Referential transparency
- Very similar lookup times when compared to imperative counterparts
- Permit fast functional versions of core data structures, while minimizing
  the added cost of mutation.
- MUCH better for concurrency. See
  [CP.1](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Rconc-multi),
  [Con.1](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Rconst-immutable), 


### Cons
- Nonstandard
  ([SL.2](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Rsl-sl))
- Some cases lend themselves well to data structures with transparent memory
  models and footprints
- Difficult to implement
- There can be more memory allocations than expected, depending on the
  implementation. See
  [R.13](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Rr-single-alloc)
- Most operations have worse asymptotic complexity, but similar/slightly worse
  performance on data structures that are, say, less than 1,000,000 elements.
- Get seriously complicated without garbage collection
