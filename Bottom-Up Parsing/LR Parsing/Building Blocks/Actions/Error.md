# Error

Given a parser in [[State|state]] $s$ and input symbol $a$, **error detection** consists in identifying the absence of an action in the ACTION table for the pair $(s, a)$, signaling that the input string is not part of the [[Formal Language|language]] and potentially triggering [[LR Error Detection|error handling]] mechanisms.