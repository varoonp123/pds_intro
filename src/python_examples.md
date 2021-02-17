## Persistent Data Structures in Python

There are a couple of libraries that offer Python bindings to reasonably high
performance implementations of persistent data structures. For much of the code
I write, (hash/sorted) sets, maps/dictionaries, and vectors are the most important. 

In practice, the main difference between these are standard Pythond data
structures is that every method returns a new data structure. No methods return
`None`. 

## Pyrsistent

[Pyrsistent](https://github.com/tobgu/pyrsistent) implements many persistent
data structures via a C extension.

- (Checked)PVector, PList,  PDequeue
- (Checked)PSet, PBag (multiset)
- (Checked)PMap, PRecord, PClass

The `freeze` and `thaw` functions recursively visit a data structure,
transforming data structures between persistent and "regular" forms.

A python dict whose keys are themselves dicts. This is ok because persistent
data structures are hashable

```python
from pyrsistent import pmap, pvector, pset, freeze

base = {pmap({"key1": i, "key2": str(i + 3)}): {"data": [i, i * 2]} for i in range(10)}

deep_persistent = freeze(
    {"key": {"k1": 3}, "data": {"k1": [False, True], 34: (1, 2, 3)}}
)
```
These data structures support an _evolver_ which is "mutable view of the vector
with 'transaction like' semantics," which permits Python-style mutation. After
"thawing", one can "freeze" back into a persistent collection with the
`persistent()` method. 

## Pyrpds

This library attempts to mimic the Pyrsistent API. The main difference is that
the underlying data structures are implemented via the Rust library
[rpds](https://github.com/orium/rpds) rather than a C extension. The main user
level difference is the import. 

```python
from pyrpds import pvector

mypvec = pvector([1,2,3])
new_vec = mypvec.extend([1,5,6]) 
```

The Rust library is not nearly as extensive as the Pyrsistent C implementations.
However, the Rust implementations can be threadsafe when using the data
structures are generic over
[`Arc`](ihttps://doc.rust-lang.org/std/sync/struct.Arc.html) types. There is a
performance cost, however, over regular refcounted smart pointers because Rust
does not use garbage collection and this is a natural way within the standard
library, short of implementing some form of garbage collection. 
