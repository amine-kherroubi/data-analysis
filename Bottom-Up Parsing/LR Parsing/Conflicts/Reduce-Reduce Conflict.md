# Reduce-Reduce Conflict

A **reduce-reduce conflict** occurs in LR parsing when the parser cannot decide which [[Production Rule|production rule]] to use for reduction. This happens when a [[State|state]] contains two or more [[Complete Item|complete items]] $A \rightarrow \alpha \, \cdot$ and $B \rightarrow \beta \, \cdot$.

The parser must choose which production to reduce, but has insufficient information to make the decision.

---

## Resolution

Different parsing methods handle reduce-reduce conflicts:

- **LR(0)**: Cannot resolve; the [[Formal Grammar|grammar]] is not LR(0).
- **SLR(1)**: Uses [[FOLLOW Set|FOLLOW sets]] to determine which reduction is valid for the current lookahead.
- **LR(1)**: Uses lookahead symbols computed during automaton construction.

Reduce-reduce conflicts typically indicate [[Grammar Ambiguity|grammar ambiguity]] or poor grammar design and are less common than [[Shift-Reduce Conflict|shift-reduce conflicts]].