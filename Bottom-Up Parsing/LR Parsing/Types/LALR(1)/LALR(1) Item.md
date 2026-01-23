# LALR(1) Item

An **LALR(1) item** is an [[Item|item]] consisting of a [[Production Rule|production rule]] with a dot position and a set of lookahead [[Terminal Symbol|terminal symbols]]. It is written as $[A \rightarrow \alpha \cdot \beta, {a_1, a_2, \ldots, a_k}]$ where $A \rightarrow \alpha \beta$ is a production and ${a_1, a_2, \ldots, a_k}$ is the lookahead set.

LALR(1) items are formed by merging [[LR(1) Item|LR(1) items]] that have the same [[Item Core|core]] but different lookahead symbols. The lookahead symbols from all merged items are combined into a set.