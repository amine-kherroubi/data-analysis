# LR(1) Automaton

An **LR(1) automaton** (also called a **canonical LR automaton**) is a finite automaton whose [[State|states]] are sets of [[LR(1) Item|LR(1) items]] and whose [[Transition|transitions]] are defined by the [[LR(1) GOTO Function|LR(1) GOTO function]]. It recognizes [[Viable Prefix|viable prefixes]] with precise lookahead information.

---

## Construction

Given an [[Augmented Grammar|augmented grammar]] $G'$:

1. The start state $I_0$ is $\text{CLOSURE}({[S' \rightarrow \cdot S, \$]})$ where $S'$ is the new [[Start Symbol|start symbol]] and $\$$ is the end marker.
2. For each state $I$ and each grammar [[Symbol|symbol]] $X$, if $\text{GOTO}(I, X)$ is non-empty and not already a state, add it as a new state.
3. Add a transition from state $I$ to state $\text{GOTO}(I, X)$ labeled with symbol $X$.
4. Repeat until no new states can be added.

---

## Characteristics

The LR(1) automaton typically has significantly more states than the [[LR(0) Automaton|LR(0) automaton]] because items with the same core but different lookahead symbols create distinct states. This increased state count is the primary drawback of canonical LR parsing.