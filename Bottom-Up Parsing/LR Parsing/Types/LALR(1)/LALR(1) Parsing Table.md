# LALR(1) Parsing Table

An **LALR(1) parsing table** is a [[Data Structure|data structure]] that guides the [[LALR(1) Parser|LALR(1) parser]]'s actions. It is constructed from the [[LALR(1) Automaton|LALR(1) automaton]] and has the same size as the [[LR(0) Parsing Table|LR(0) parsing table]].

---

## Structure

1. **ACTION table**: Maps (state, [[Terminal Symbol|terminal]]) pairs to parser actions
2. **GOTO table**: Maps (state, [[Nonterminal Symbol|nonterminal]]) pairs to next state

---

## Construction

For each state $I_i$ in the [[LALR(1) Automaton|LALR(1) automaton]]:

1. If $[A \rightarrow \alpha \cdot a \beta, L]$ is in $I_i$ and $\text{GOTO}(I_i, a) = I_j$ where $a$ is a terminal, set ACTION$[i, a]$ to "shift $j$".
2. If $[A \rightarrow \alpha \cdot, L]$ is in $I_i$ and $A \neq S'$, set ACTION$[i, a]$ to "reduce $A \rightarrow \alpha$" for each terminal $a \in L$.
3. If $[S' \rightarrow S \cdot, \{\$\}]$ is in $I_i$, set ACTION$[i, $]$ to "accept".
4. If $\text{GOTO}(I_i, A) = I_j$ where $A$ is a nonterminal, set GOTO$[i, A] = j$.

In step 2, $L$ represents the lookahead set for the [[LALR(1) Item|LALR(1) item]]. Reductions are added for all terminals in the lookahead set.