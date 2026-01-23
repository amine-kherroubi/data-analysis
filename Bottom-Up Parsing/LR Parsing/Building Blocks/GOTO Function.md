# GOTO Function

The **GOTO function** is an operation used in LR parser construction that computes the transition from one [[State|state]] to another when a [[Formal Grammar|grammar]] [[Symbol|symbol]] is recognized. It defines state transitions in the LR parser's finite automaton.

---

## Definition

Given an item set $I$ and a grammar symbol $X$:

$$\text{GOTO}(I, X) = \text{CLOSURE}({A \rightarrow \alpha X \cdot \beta \mid A \rightarrow \alpha \cdot X \beta \in I})$$

---

## Algorithm

1. For each [[Item|item]] in $I$ with the dot immediately before [[Symbol|symbol]] $X$ (i.e., $A \rightarrow \alpha \cdot X \beta$), move the dot past $X$ to create $A \rightarrow \alpha X \cdot \beta$.
2. Collect all such items into a set $J$.
3. Return $\text{CLOSURE}(J)$ using the [[Closure Function|closure function]].