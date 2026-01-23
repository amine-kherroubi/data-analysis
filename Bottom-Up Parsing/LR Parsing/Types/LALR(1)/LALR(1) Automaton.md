# LALR(1) Automaton

An **LALR(1) automaton** (Lookahead LR automaton) is a finite automaton constructed by merging [[State|states]] in the [[LR(1) Automaton|LR(1) automaton]] that have the same core items. It has the same number of states as the LR(0) automaton, making it more practical than [[LR(1) Parser|LR(1) parsing]]. However, merging states can introduce [[Reduce-Reduce Conflict|reduce-reduce conflicts]] that would not exist in the LR(1) automaton.

---

## Construction Methods

### Method 1: Merging LR(1) States

1. Construct the complete [[LR(1) Automaton|LR(1) automaton]].
2. Identify states with identical cores (same items ignoring lookaheads).
3. Merge these states by combining their lookahead sets.
4. The resulting automaton has states consisting of [[LALR(1) Item|LALR(1) items]].

### Method 2: Direct Construction

1. Start with the [[LR(0) Automaton|LR(0) automaton]].
2. Compute lookahead sets for each item using lookahead propagation and spontaneous generation.
3. This method avoids constructing the full LR(1) automaton.