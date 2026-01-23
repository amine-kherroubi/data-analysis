# Lookahead Propagation

**Lookahead propagation** is a technique used in the efficient construction of [[LALR(1) Automaton|LALR(1) automata]] without first building the complete [[LR(1) Automaton|LR(1) automaton]]. It determines lookahead symbols for items through two mechanisms: spontaneous generation and propagation.

---

## Mechanisms

### Spontaneous Generation

A lookahead symbol $a$ is **spontaneously generated** for an [[Item|item]] in a state if it appears in that item during closure computation from a kernel item with a dummy lookahead symbol $\#$.

If $\text{CLOSURE}({[A \rightarrow \alpha \cdot, \#]})$ contains $[B \rightarrow \beta \cdot, a]$ where $a \neq \#$, then $a$ is spontaneously generated for the item $B \rightarrow \beta \cdot$ in that state.

### Propagation

A lookahead symbol **propagates** from one item to another if using a dummy lookahead $\#$ in the source item causes $\#$ to appear in the target item after a GOTO operation.

If $[A \rightarrow \alpha \cdot, \#] \in I$ and $[B \rightarrow \beta \cdot, \#] \in \text{GOTO}(I, X)$, then lookaheads propagate from the first item to the second.

---

## Algorithm

1. Build the [[LR(0) Automaton|LR(0) automaton]].
2. For each [[Kernel Item|kernel item]] in each state, determine which lookaheads are spontaneously generated and which propagate to other kernel items.
3. Initialize kernel items with spontaneously generated lookaheads and the start item with $\$$.
4. Propagate lookaheads until no changes occur.

This method is more efficient than constructing the full LR(1) automaton and then merging states.