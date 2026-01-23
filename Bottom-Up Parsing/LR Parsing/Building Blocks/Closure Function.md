# Closure Function

The **closure function** is an operation used in LR parser construction that takes a set of [[Item|items]] and expands it by adding all [[Closure Item|closure items]] derivable from that set. It computes all [[Production Rule|productions]] that could be recognized starting from the current parsing position.

---

## Algorithm

Given an item set $I$, compute $\text{CLOSURE}(I)$:

1. Add all items in $I$ to $\text{CLOSURE}(I)$.
2. For each item $A \rightarrow \alpha \cdot B \beta$ in $\text{CLOSURE}(I)$ where $B$ is a [[Nonterminal Symbol|nonterminal]], for every [[Production Rule|production]] $B \rightarrow \gamma$ in the [[Formal Grammar|grammar]], add the [[Initial Item|initial item]] $B \rightarrow \cdot , \gamma$ to $\text{CLOSURE}(I)$ if not already present.
3. Repeat step 2 until no new items can be added.