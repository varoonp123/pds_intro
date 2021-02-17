# Performance of Persistent Data Structures

Persistent data structures use structural sharing to avoid deep copies. [These
images](https://hypirion.com/musings/understanding-persistent-vector-pt-1)
explain it better than I could.

Persistent data structures are absolutely slower than mutable/imperative
counterparts. Returning a new object under any mutation comes with a cost.
Moreover, most of these data structures are implemented as trees. Clojure vector
implemtation, for example, as log 32 lookup times. 

Seriously benchmarking Clojure's implementation against the Java counterparts is
[extremely
difficult](https://hypirion.com/musings/persistent-vector-performance). Rough
benchmarks show reasonably good lookup times when comapring Clojure's persistent
vector against `java.util.ArrayList`.
