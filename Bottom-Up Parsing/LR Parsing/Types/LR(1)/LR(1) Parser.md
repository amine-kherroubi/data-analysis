# LR(1) Parser

An **LR(1) parser** (also called a **canonical LR parser**) is a [[Bottom-Up Parsing|bottom-up parser]] that uses an [[LR(1) Automaton|LR(1) automaton]] to guide parsing decisions. It is the most powerful deterministic [[Bottom-Up Parsing|bottom-up parsing]] method that uses one [[Symbol|symbol]] of lookahead.

---

## Parser Actions

The parser maintains a stack of states and performs actions based on the current state and lookahead symbol:

1. **Shift**: Push the next input symbol and the corresponding next state onto the stack
2. **Reduce**: Apply a [[Production Rule|production]] $A \rightarrow \beta$ by popping $|\beta|$ states from the stack, then push $A$ and $\text{GOTO}(\text{top}, A)$
3. **Accept**: Accept the input when the [[Complete Item|complete item]] $[S' \rightarrow S \cdot, $]$ is reached

---

## Properties

LR(1) parsers can parse all deterministic [[Context-Free Grammar (CFG)|context-free grammars]] with one symbol of lookahead. They resolve more conflicts than [[SLR(1) Parser|SLR(1) parsers]] because lookahead symbols are computed precisely during automaton construction rather than using [[FOLLOW Set|FOLLOW sets]].

The main disadvantage is the large number of states in the automaton. [[LALR(1) Parser|LALR(1) parsing]] addresses this by merging compatible states.