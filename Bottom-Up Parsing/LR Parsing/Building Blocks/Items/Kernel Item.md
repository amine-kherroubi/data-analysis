# Kernel Items

A **kernel item** is an [[Item|item]] that belongs to the core set of items in a parser state. Kernel items form the basis from which [[Closure Item|closure items]] are derived.

The kernel of an item set consists of:

1. The [[Initial Item|initial item]] of the [[Augmented Grammar|augmented grammar]] (e.g., $S' \rightarrow \cdot S$) in the [[Start State|start state]]
2. All items with the dot not at the leftmost position (i.e., non-initial items that show progress in recognizing a [[Production Rule|production]])

Kernel items represent definite parsing progress and are used to identify distinct parser [[State|states]]. Two item sets with different kernels represent different parser states, while item sets with identical kernels represent the same state.

The kernel uniquely characterizes a state in LR parser construction, as the closure can always be deterministically computed from the kernel.