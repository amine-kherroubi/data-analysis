# Closure Item

A **closure item** is an [[Item|item]] that is added to an item set through the closure operation, as opposed to being a [[Kernel Item|kernel item]].

Closure items are [[Initial Item|initial items]] that are derived when a kernel item has the dot immediately before a [[Nonterminal Symbol|nonterminal]] (e.g., $A \rightarrow \alpha \cdot B \beta$). For every such nonterminal $B$, all [[Production Rule|productions]] with $B$ on the left-hand side are added as initial items (e.g., $B \rightarrow \cdot , \gamma$ for all $B \rightarrow \gamma$).

The closure operation is applied transitively until no new items can be added.

Closure items represent potential parsing paths from the current [[State|state]]. Unlike kernel items, closure items can be deterministically recomputed from the kernel and are therefore not stored when characterizing parser states.