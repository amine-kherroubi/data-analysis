# Spontaneous Lookahead Generation

**Spontaneous lookahead generation** is a mechanism in [[LALR(1) Automaton|LALR(1) automaton]] construction where lookahead symbols appear in derived [[Item|items]] during [[Closure Function|closure]] computation, independent of the lookahead in the source item.

---

## Mechanism

To determine spontaneous generation for a [[Kernel Item|kernel item]] $[A \rightarrow \alpha \cdot X \beta]$ in state $I$:

1. Compute $\text{CLOSURE}({[A \rightarrow \alpha \cdot X \beta, \#]})$ using a special dummy symbol $\#$
2. Examine items in $\text{GOTO}(\text{CLOSURE}({[A \rightarrow \alpha \cdot X \beta, \#]}), X)$
3. For any item $[B \rightarrow \gamma \cdot, a]$ where $a \neq \#$, the lookahead $a$ is spontaneously generated for item $B \rightarrow \gamma \cdot$ in state $\text{GOTO}(I, X)$

---

## Example

Given item $[A \rightarrow \cdot BC, \#]$:

If the [[Formal Grammar|grammar]] has production $B \rightarrow d$ and $C$ can derive $e$, then during closure, we might generate $[B \rightarrow \cdot d, e]$.

The lookahead $e$ comes from $\text{FIRST}(C\#)$ and is spontaneously generated, not propagated from the original $\#$.

---

## Usage

Spontaneous generation is one of two mechanisms (along with [[Lookahead Propagation|lookahead propagation]]) used to efficiently compute lookahead sets in LALR(1) construction without building the full [[LR(1) Automaton|LR(1) automaton]].