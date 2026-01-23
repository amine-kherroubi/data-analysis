# LR(0) Automaton

An **LR(0) automaton** is a finite automaton whose [[State|states]] are sets of [[LR(0) Item|LR(0) items]] and whose [[Transition|transitions]] are defined by the [[GOTO Function|GOTO function]]. It recognizes [[Viable Prefix|viable prefixes]] of the [[Formal Grammar|grammar]].

---

## Construction

Given an [[Augmented Grammar|augmented grammar]] $G'$:

1. The start state $I_0$ is $\text{CLOSURE}({S' \rightarrow \cdot S})$ where $S'$ is the new [[Start Symbol|start symbol]].
2. For each state $I$ and each grammar [[Symbol|symbol]] $X$, if $\text{GOTO}(I, X)$ is non-empty and not already a state, add it as a new state.
3. Add a transition from state $I$ to state $\text{GOTO}(I, X)$ labeled with symbol $X$.
4. Repeat until no new states can be added.