# LALR(1) Parser

An **LALR(1) parser** (Lookahead LR parser) is a [[Bottom-Up Parsing|bottom-up parser]] that uses an [[LALR(1) Automaton|LALR(1) automaton]] to guide parsing decisions. It provides a practical compromise between [[SLR(1) Parser|SLR(1)]] and [[LR(1) Parser|LR(1)]] parsing.

---

## Properties

LALR(1) parsers are more powerful than [[SLR(1) Parser|SLR(1) parsers]] because they use precise lookahead information computed during automaton construction. They are less powerful than [[LR(1) Parser|LR(1) parsers]] because merging states with identical cores can introduce [[Reduce-Reduce Conflict|reduce-reduce conflicts]].

LALR(1) parsers never introduce new [[Shift-Reduce Conflict|shift-reduce conflicts]] compared to LR(1).

The key advantage is that LALR(1) parsers have the same number of states as [[LR(0) Parser|LR(0) parsers]], making them practical for real-world use. Most parser generators (such as Yacc, Bison) use LALR(1) parsing.