# Handle

A **handle** is a substring of a [[Right Sentential Form|right sentential form]] that matches the right side of a [[Production Rule|production rule]], and whose reduction to the [[Nonterminal Symbol|nonterminal]] on the left side of that production represents one step in the reverse of a rightmost [[Formal Language Theory/Building Blocks/Derivation|derivation]].

If we have a right sentential form $\alpha \beta \omega$ where $\beta$ is the handle, then reducing $\beta$ to the nonterminal $A$ (where $A \rightarrow \beta$ is a production) gives the previous right sentential form in the rightmost derivation.

There is exactly one handle in each right sentential form (for [[Grammar Ambiguity|unambiguous grammars]]).

Finding handles is the central task of [[Bottom-Up Parsing|bottom-up parsing]].