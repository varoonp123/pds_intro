# Values, Immutability, and Accretion

Examples of Accretion/recording revisions vs "write over":

- Accounting. Double entry book keeping.
- Blockchain. This is what many have chosen when constructing a transaction system
  from scratch.
- Many databases encourage accreting and recording revisions over deletes and
  updates.
- Python benefits greatly from immutable strings. I claim that if Python strings
  required/encouraged inplace mutation, many of the programs I write would
  probably be worse. I further claim that most Python users would dislike the
  change also.
- Most numpy as pandas operations return new arrays and dataframes. These are
  frequently metadata that describe how to view/interpret a compact/contiguous
  array that is managed at another level of abstraction.

There are levels of abstractions of many problems that require mutation in place
and/or the appearance of in place mutation. Most mutation in most of the
programs I write is incidental complexity. We need a way to distinguish between
**identities** and **values**. Indirection is extremely useful. It is possible
for an identity in one context to be a value in another and vice versa.
Consider, for example, a struct/record/product type containing a pointer. Or a
tuple containing dicts of lists of sets of strings. When using values, "passing
by value" or "passing by reference" is an implementation detail. The notion of
value in this context is not the same as the "value" in "pass by value." Value
types permit nonblocking parallel reads. **With immutable objects in managed
memory settings, "pass by value" vs "pass by reference" is merely an
implementation detail.** 

As someone who started programming with Java, Python, R, etc. I could not agree
more with Rich Hickey. "As a default architecture, I think mutable objects
are...terrible." I would add **terrible for most things I work on**. Here is
what I care about:

1. Much of the data in my programs represents facts about the outside world at
   certain points in time. I want my programs to reflect that. 
2. In place mutation is a side effect and does not compose. I want to build my
   programs from composing small functions. 
3. Value types make concurrent programs much easier. Concurrency is much harder
   with in place mutation. I want to have the same mental model when writing
   concurrent and synchronous code.

## Example: Python Strings

As of Python 3.8, the main string type is a value type. None of the operations
return none and mutate in place. 

```python
ex_str = "Jeffrey Epstein Did not Kill Himself"
new_str = ex_str + "!"
```
This was decided because strings, are as fundamental as ints, bools, and floats
in most high level programs, and it is useful to never worry about in place
mutation. 

But as of February 2021, [CPython sometimes mutates strings inplace as an
optimization](https://web.eecs.utk.edu/~azh/blog/pythonstringsaremutable.html).
The address to the pointer for a string will sometimes change if one performs
some operation on a string, but only when CPython determines it is safe to do
so. There are some conditions related to the ref count and [string
interning](https://en.wikipedia.org/wiki/String_interning). This is an
optimization that must work with Python strings being values at the user level.
