# LR(0) Parsing Table

An **LR(0) parsing table** is a [[Data Structure|data structure]] that guides the [[LR(0) Parser|LR(0) parser]]'s actions. It consists of two parts: the **ACTION table** and the **GOTO table**.

---

## Structure

1. **ACTION table**: Maps (state, [[Terminal Symbol|terminal]]) pairs to parser actions
    - **Shift $j$**: Shift input symbol and push state $j$
    - **Reduce $A \rightarrow \beta$**: Reduce using [[Production Rule|production]] $A \rightarrow \beta$
    - **Accept**: Accept the input
2. **GOTO table**: Maps (state, [[Nonterminal Symbol|nonterminal]]) pairs to next state

---

## Construction

For each state $I_i$ in the [[LR(0) Automaton|LR(0) automaton]]:

1. If $A \rightarrow \alpha \cdot a \beta$ is in $I_i$ and $\text{GOTO}(I_i, a) = I_j$ where $a$ is a terminal, set ACTION$[i, a]$ to "shift $j$".
2. If $A \rightarrow \alpha \cdot$ is in $I_i$ and $A \neq S'$, set ACTION$[i, a]$ to "reduce $A \rightarrow \alpha$" for all terminals $a$.
3. If $S' \rightarrow S \cdot$ is in $I_i$, set ACTION$[i, $]$ to "accept".
4. If $\text{GOTO}(I_i, A) = I_j$ where $A$ is a nonterminal, set GOTO$[i, A] = j$.

If any entry has multiple actions, the grammar has a conflict and is not LR(0).