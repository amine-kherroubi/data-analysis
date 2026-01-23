# Canonical Collection

The **canonical collection** is the complete set of [[State|states]] (item sets) in an LR automaton. It represents all possible configurations of the parser during [[Bottom-Up Parsing|bottom-up parsing]].

---

## Definition

For a [[Formal Grammar|grammar]] $G$, the canonical collection $\mathcal{C}$ is the set of all item sets reachable from the initial state using the [[GOTO Function|GOTO function]].

---

## Construction

Given an [[Augmented Grammar|augmented grammar]] $G'$:

1. Initialize $\mathcal{C}$ with the start state $I_0$.
2. For each state $I \in \mathcal{C}$ and each grammar [[Symbol|symbol]] $X$, compute $J = \text{GOTO}(I, X)$.
3. If $J$ is non-empty and not already in $\mathcal{C}$, add $J$ to $\mathcal{C}$.
4. Repeat until no new states are added.

---

## Variants

- [[LR(0) Automaton|LR(0)]] canonical collection uses [[LR(0) Item|LR(0) items]] and LR(0) closure/GOTO.
- [[LR(1) Automaton|LR(1)]] canonical collection uses [[LR(1) Item|LR(1) items]] with lookahead symbols.
- [[LALR(1) Automaton|LALR(1)]] canonical collection merges LR(1) states with identical [[Item Core|cores]].

The canonical collection forms the states of the LR automaton, which is then used to construct the parsing table.