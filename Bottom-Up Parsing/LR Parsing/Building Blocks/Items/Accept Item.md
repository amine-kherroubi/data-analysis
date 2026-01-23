# Accept Item

An **accept item** is a special [[Complete Item|complete item]] in LR parsing that indicates successful completion of parsing. It has the form $S' \rightarrow S \cdot$ (for [[LR(0) Item|LR(0)]]) or $[S' \rightarrow S \cdot, \$]$ (for [[LR(1) Item|LR(1)]] and [[LALR(1) Item|LALR(1)]]), where $S'$ is the start symbol of the [[Augmented Grammar|augmented grammar]].

---

## Behavior

When an LR parser encounters an accept item:

1. The entire input has been successfully parsed.
2. The stack contains the start state and the original start symbol $S$.
3. The lookahead symbol is $\$$ (end of input marker).
4. The parser halts and accepts the input string.

---

## Location

The accept item appears only in states reachable after recognizing the complete input derived from the original [[Start Symbol|start symbol]] $S$. In most grammars, this occurs in exactly one state.

---

## Distinction from Complete Items

Unlike regular [[Complete Item|complete items]] that trigger reduction actions, the accept item triggers the accept action, terminating parsing successfully rather than continuing with further reductions.