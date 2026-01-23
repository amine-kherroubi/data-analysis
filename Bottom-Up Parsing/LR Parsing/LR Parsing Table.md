# LR Parsing Table

An **LR parsing table** is a [[Data Structure|data structure]] that guides [[LR Parsing|LR parser]] decisions during [[Bottom-Up Parsing|bottom-up parsing]]. It consists of two components: the ACTION table and the GOTO table.

---

## Structure

### ACTION Table

Maps (state, [[Terminal Symbol|terminal]]) pairs to [[LR Parser Action|parser actions]].

### GOTO Table

Maps (state, [[Nonterminal Symbol|nonterminal]]) pairs to next state after a reduction. Used when a nonterminal is pushed onto the stack following a reduction.

---

## Table Size

The table has dimensions:

- Rows: Number of states in the automaton
- ACTION columns: Number of terminals (including $\$$)
- GOTO columns: Number of nonterminals

---

## Conflicts

If a table entry contains multiple actions, the grammar has a conflict, either a [[Shift-Reduce Conflict|shift-reduce conflict]] where both shift and reduce actions are possible, or a [[Reduce-Reduce Conflict|reduce-reduce conflict]] with multiple reduce actions.

Different LR methods ([[LR(0) Parser|LR(0)]], [[SLR(1) Parser|SLR(1)]], [[LALR(1) Parser|LALR(1)]], [[LR(1) Parser|LR(1)]]) differ in how they populate this table and how many conflicts they can avoid.