# Diversity

Python 3 code to measure the similarity between two ELF files

Assumptions


Each instruction is uniquely identifiable by its position in a program sequence. The ordering of instructions is important to determine program similarity, the larger the coincidence in the order that instructions happen the larger the similarity between programs.

This measure determines  if there is a monotonic relationship in the instruction sequences of P1 and P2

Each instruction in P1 is labelled by its bytecode and the number of times it has occurred so far in P1. Each label is assigned as its rank the position in P1's sequence---the ranking of P1's instructions  is thus just the consecutive numbers from 1 to the number of instructions is P1.

Instruction in P2 are assigned a rank according to P1's labels: for instance, if instruction INST  occurs for the third time at position 100 of P1s program sequence then the third occurrence of INST in P2 will have label 100 regardless of where it appears in P2.
We then compute the correlation between both rankings

One drawback of these metrics is they are heavily affected by the insertion or deletion of instructions since they will cause offsets is only parts of the code (and the rotations rotate all the code).  Also, they only implicitly take into account instruction ordering (as identically ordered instructions sequences will yield high similarity) but random instruction coincidences can yield a similar score identical code snippets.


  - Our resent proposal consists of a transformation of the code (just like a rotation is a transformation) and then a measurement over the transformed programs. For these metric we assume that instruction ordering is important but try to reduce the impact of a few instruction insertions and/or deletions within fragments of code.
    - A very quick description (working on a better one and I don't want to clutter this document with it): Given two programs $P_1$ and $P_2$ we rewrite (transform) $P-2$ as a permutation of $P_1$'s instructions. After this transformation there are several ways to measure their similarity, we now employ Spearman's rank correlation  to assess how similar both representations are (Instruction Rank Similarity). There are still some things to work out but so far the results are good (we think).
  - Use diff (just an idea)





Let $P_1$ and $P_2$ be programs. Let each instruction in P1 be labeled with its bytecode and its ocurrance number in P1. We then rewrite P2 as a permutation of  our representation of P1.
- still to determine what with different sizes and non-intersecting elements

This representation assumes the order of instructions is important. That a bytcode is different depending on the number of times it has been executed. This representation highlights common sequences


Let $P_1$ and $P_2$ be programs. Let each instruction in P1 be labeled with its bytecode and its ocurrance number in P1. We then rewrite P2 as a permutation of  our representation of P1.
- still to determine what with different sizes and non-intersecting elements

This representation assumes the order of instructions is important. That a bytcode is different depending on the number of times it has been executed. This representation highlights common sequences
