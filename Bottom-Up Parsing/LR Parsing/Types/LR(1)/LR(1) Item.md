# LR(1) Item

An **LR(1) item** (also called a **canonical LR item**) is an [[Item|item]] consisting of a [[Production Rule|production rule]] with a dot position and a lookahead [[Terminal Symbol|terminal symbol]]. It is written as $[A \rightarrow \alpha \cdot \beta, a]$ where $A \rightarrow \alpha \beta$ is a production and $a$ is the lookahead symbol.

The lookahead symbol $a$ indicates that the production should only be reduced when $a$ is the next input symbol. This provides more precise parsing decisions than [[LR(0) Item|LR(0) items]] or [[SLR(1) Parser|SLR(1) parsing]].

---

## Semantics

An LR(1) item $[A \rightarrow \alpha \cdot \beta, a]$ is valid for a [[Viable Prefix|viable prefix]] $\gamma$ if:

1. $\gamma = \delta \alpha$ for some $\delta$,
2. There exists a [[Formal Language Theory/Building Blocks/Derivation|derivation]] where $A$ appears with lookahead $a$ following it.

The lookahead symbol is only relevant when $\beta = \epsilon$ (i.e., when the item is [[Complete Item|complete]]), indicating that reduction should only occur when the next input is $a$.