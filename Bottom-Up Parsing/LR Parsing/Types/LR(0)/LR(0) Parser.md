# LR(0) Parser

An **LR(0) parser** is a [[Bottom-Up Parsing|bottom-up parser]] that uses an [[LR(0) Automaton|LR(0) automaton]] to guide parsing decisions. It determines actions based solely on the current state without lookahead.

---

## Parser Actions

The parser maintains a stack of states and performs two types of actions:

1. **Shift**: Push the next input [[Symbol|symbol]] and the corresponding next state onto the stack.
2. **Reduce**: Apply a [[Production Rule|production]] $A \rightarrow \beta$ by popping $|\beta|$ states from the stack, then push $A$ and $\text{GOTO}(\text{top}, A)$.
3. **Accept**: Accept the input when the [[Complete Item|complete item]] $S' \rightarrow S \cdot$ is reached.

---

## Limitations

LR(0) parsers cannot handle [[Formal Grammar|grammars]] with [[Shift-Reduce Conflict|shift-reduce conflicts]] or [[Reduce-Reduce Conflict|reduce-reduce conflicts]]. Most practical grammars require lookahead, making LR(0) parsers limited in applicability.