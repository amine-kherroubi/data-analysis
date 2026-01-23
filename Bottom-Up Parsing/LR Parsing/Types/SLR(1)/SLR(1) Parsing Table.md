# SLR(1) Parsing Table

An **SLR(1) parsing table** is a [[Data Structure|data structure]] that guides the [[SLR(1) Parser|SLR(1) parser]]'s actions. It has the same structure as the [[LR(0) Parsing Table|LR(0) parsing table]] but uses [[FOLLOW Set|FOLLOW sets]] to refine reduction actions.

---

## Structure

1. **ACTION table**: Maps (state, [[Terminal Symbol|terminal]]) pairs to parser actions.
2. **GOTO table**: Maps (state, [[Nonterminal Symbol|nonterminal]]) pairs to next state.

---

## Construction

For each state $I_i$ in the [[LR(0) Automaton|LR(0) automaton]]:

1. If $A \rightarrow \alpha \cdot a \beta$ is in $I_i$ and $\text{GOTO}(I_i, a) = I_j$ where $a$ is a terminal, set ACTION$[i, a]$ to "shift $j$".
2. If $A \rightarrow \alpha \cdot$ is in $I_i$ and $A \neq S'$, set ACTION$[i, a]$ to "reduce $A \rightarrow \alpha$" for each terminal $a \in \text{FOLLOW}(A)$.
3. If $S' \rightarrow S \cdot$ is in $I_i$, set ACTION$[i, $]$ to "accept".
4. If $\text{GOTO}(I_i, A) = I_j$ where $A$ is a nonterminal, set GOTO$[i, A] = j$.

The key difference from LR(0) is step 2: reductions are only added for terminals in the FOLLOW set, not for all terminals.