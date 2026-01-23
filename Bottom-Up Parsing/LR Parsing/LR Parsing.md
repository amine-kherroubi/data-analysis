# LR Parsing

**LR parsing** is a family of efficient [[Bottom-Up Parsing|bottom-up parsing]] techniques for [[Context-Free Grammar (CFG)|context-free grammars]]. LR parsers read input from left to right and construct a rightmost [[Formal Language Theory/Building Blocks/Derivation|derivation]] in reverse by identifying [[Handle|handles]] and performing reductions. They use:

1. A stack to maintain [[State|states]] representing the parsing history.
2. A [[LR Parsing Table|parsing table]] (ACTION and GOTO) to determine actions.
3. An input buffer containing remaining [[Symbol|symbols]] to parse.