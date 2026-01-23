# Shift-Reduce Conflict

A **shift-reduce conflict** occurs in LR parsing when the parser cannot decide whether to shift the next input [[Symbol|symbol]] or reduce using a [[Production Rule|production rule]]. This happens when a [[State|state]] contains both an [[LR(0) Item|item]] $A \rightarrow \alpha \cdot a \beta$ (suggesting a shift) and a [[Complete Item|complete item]] $B \rightarrow \gamma \cdot$ (suggesting a reduce).

---

## Resolution

Different parsing methods resolve shift-reduce conflicts differently:

- **LR(0)**: Cannot resolve; the [[Formal Grammar|grammar]] is not LR(0).
- **SLR(1)**: Uses [[FOLLOW Set|FOLLOW sets]] to determine if reduction is valid for the current lookahead.
- **LR(1)**: Uses lookahead symbols computed during automaton construction.

Shift-reduce conflicts commonly arise in grammars with ambiguous operator precedence or the [[Dangling Else Problem|dangling else problem]].