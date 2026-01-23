# LR Parsing and Deterministic Pushdown Automata

LR parsers are equivalent to **deterministic pushdown automata (DPDAs)**. This theoretical connection provides insight into the power and limitations of LR parsing.

---

## Equivalence

An [[LR(1) Parser|LR(1) parser]] can recognize exactly the languages recognizable by deterministic pushdown automata with one symbol of lookahead. This class is strictly smaller than the class of all context-free languages.

---

## Stack and States

In an LR parser:

- The stack contains a sequence of states from the [[LR(0) Automaton|LR automaton]].
- These states encode the parsing history and guide future actions.
- The stack corresponds to the pushdown store in a DPDA.

---

## Determinism

LR parsing is deterministic because:

1. The ACTION table has at most one action per (state, symbol) pair.
2. No backtracking is needed.
3. The parser makes exactly one left-to-right pass over the input.

This determinism is what makes LR parsing efficient, running in linear time relative to input length.

---

## Limitations

Not all context-free languages can be parsed by LR methods. Some [[Context-Free Grammar (CFG)|context-free grammars]] are inherently [[Grammar Ambiguity|ambiguous]] or require unbounded lookahead, making them unparseable by any DPDA and therefore by any LR parser.

The LR(k) languages form a proper subset of deterministic context-free languages, which form a proper subset of all context-free languages.