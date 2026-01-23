# SLR(1) Parser

An **SLR(1) parser** (Simple LR(1) parser) is a [[Bottom-Up Parsing|bottom-up parser]] that extends the [[LR(0) Parser|LR(0) parser]] by using [[FOLLOW Set|FOLLOW sets]] to determine when reductions are valid. The "1" indicates one [[Symbol|symbol]] of lookahead.

Unlike LR(0) parsers, SLR(1) parsers only reduce using [[Production Rule|production]] $A \rightarrow \alpha$ when the next input symbol is in $\text{FOLLOW}(A)$. This resolves many [[Shift-Reduce Conflict|shift-reduce]] and [[Reduce-Reduce Conflict|reduce-reduce conflicts]] that occur in LR(0).

The parser uses the same [[LR(0) Automaton|LR(0) automaton]] as LR(0) but constructs the [[SLR(1) Parsing Table|parsing table]] differently by considering lookahead symbols.