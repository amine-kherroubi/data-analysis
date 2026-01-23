# Item

An **item** is a [[Production Rule|production rule]] with a dot ($\cdot$) indicating the current parsing position. The dot shows how much of the production has been recognized during [[Bottom-Up Parsing|bottom-up parsing]].

Items are the fundamental building blocks of LR parser construction. Different LR parsing methods use different types of items:

- [[LR(0) Item|LR(0) items]] consist of production with a dot position only.
- LR(1) items consist of production with a dot position and a lookahead symbol.
- LALR(1) items consist of production with a dot position and a set of lookahead symbols.

The position of the dot categorizes items as [[Initial Item|initial items]] (dot at the beginning), [[Complete Item|complete items]] (dot at the end), or intermediate items.