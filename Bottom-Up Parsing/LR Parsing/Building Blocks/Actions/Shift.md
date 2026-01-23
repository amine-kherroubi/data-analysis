# Shift

Given a parser in [[State|state]] $s$ with the current input symbol $a$, **shifting** consists in the parser pushing $a$ and $s$ onto the stack and advancing the input pointer to the next symbol. This action is applicable when the current input symbol is part of a [[Handle|handle]] that can be extended according to the parsing table.