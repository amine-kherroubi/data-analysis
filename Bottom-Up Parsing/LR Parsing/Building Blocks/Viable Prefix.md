# Viable Prefix

A **viable prefix** is a prefix of a [[Right Sentential Form|right sentential form]] that does not extend past the right end of the [[Handle|handle]] of that right sentential form. It's called "viable" because we can continue parsing from this prefix to reach a valid sentence. No viable prefix has a handle to its right.

LR parsers use viable prefixes to determine valid parser states. The LR parser stack always contains a viable prefix.