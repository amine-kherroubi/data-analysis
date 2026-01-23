# Bottom-Up Parsing

**Bottom-up parsing** constructs a parse tree from input tokens (leaves) upward to the start symbol (root). The parser reads input left-to-right and performs rightmost derivation in reverse, using shift and reduce operations.

---

## Core Operations

- **Shift**: Push next input token onto stack.
- **Reduce**: Replace top stack symbols matching a production's right-hand side with its left-hand side nonterminal.

---

## Algorithm Types

- **SLR (Simple LR)**: Basic LR parser with single lookahead symbol. Simple construction but limited grammar coverage.
- **LR (Canonical LR)**: Most powerful deterministic parser. Handles largest grammar class but generates large parsing tables.
- **LALR (Lookahead LR)**: Merges LR states with identical cores. Provides LR power with smaller tables. Industry standard.

---

## Key Properties

- Handles left-recursive grammars without modification
- Detects errors at earliest possible point in input
- Operates in linear time without backtracking
- Requires preprocessing to generate parsing tables
- Cannot handle all context-free grammars (only LR-class)

---

## Applications

- **Compiler Construction**: Standard approach for parsing programming languages (C, Java, Python). Tools like YACC and Bison generate LALR parsers automatically.
- **Database Systems**: SQL query parsing in DBMS engines.
- **Markup Processing**: XML and HTML parsers for document object model construction.
- **IDE Tools**: Real-time syntax checking and error detection in code editors.
- **Expression Evaluation**: Calculator programs and formula parsers with operator precedence.