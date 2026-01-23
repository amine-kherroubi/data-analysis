# Item Core

The **core** of an [[Item|item]] is the [[Production Rule|production rule]] with the dot position, ignoring any lookahead information. For an [[LR(1) Item|LR(1) item]] $[A \rightarrow \alpha \cdot \beta, a]$, the core is $A \rightarrow \alpha \cdot \beta$.

---

## Equivalence

Two items have the **same core** (or are **core-equivalent**) if they have the same production and the same dot position, regardless of their lookahead symbols.

For example, $[A \rightarrow \alpha \cdot \beta, a]$ and $[A \rightarrow \alpha \cdot \beta, b]$ have the same core.

---

## Usage in LALR(1)

The concept of item cores is fundamental to [[LALR(1) Parser|LALR(1) parsing]]. [[LALR(1) Automaton|LALR(1) automaton]] construction merges [[LR(1) Automaton|LR(1)]] states that contain items with identical cores, combining their lookahead symbols into sets.

A set of [[State|states]] in the LR(1) automaton can be merged into a single LALR(1) state if and only if all the states have the same set of item cores.