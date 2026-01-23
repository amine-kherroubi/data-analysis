# LR(1) Parsing Table

An **LR(1) parsing table** is a [[Data Structure|data structure]] that guides the [[LR(1) Parser|LR(1) parser]]'s actions. It has the same structure as other LR parsing tables but uses precise lookahead information from [[LR(1) Item|LR(1) items]].

---

## Structure

1. **ACTION table**: Maps (state, [[Terminal Symbol|terminal]]) pairs to parser actions
2. **GOTO table**: Maps (state, [[Nonterminal Symbol|nonterminal]]) pairs to next state

---

## Construction

For each state $I_i$ in the [[LR(1) Automaton|LR(1) automaton]]:

1. If $[A \rightarrow \alpha \cdot a \beta, b]$ is in $I_i$ and $\text{GOTO}(I_i, a) = I_j$ where $a$ is a terminal, set ACTION$[i, a]$ to "shift $j$"
2. If $[A \rightarrow \alpha \cdot, a]$ is in $I_i$ and $A \neq S'$, set ACTION$[i, a]$ to "reduce $A \rightarrow \alpha$"
3. If $[S' \rightarrow S \cdot, $]$ is in $I_i$, set ACTION$[i, $]$ to "accept"
4. If $\text{GOTO}(I_i, A) = I_j$ where $A$ is a nonterminal, set GOTO$[i, A] = j$

The key difference from [[SLR(1) Parsing Table|SLR(1)]] is step 2: reductions are only added for the specific lookahead symbol $a$ in the item, not for all symbols in $\text{FOLLOW}(A)$. This precision eliminates many conflicts present in SLR(1) tables.