

# Similarity Metrics Review

Program diversity metrics, as we've seen so far, can be divided into two large groups: Measures that aim to categorize the diversity of a population of programs (useful to characterize diversity creating algorithms) and metrics that determine how different or similar  two programs are from one another. The latter pairwise similarity metrics are used as part of some of the former population diversity metrics.


## Pairwise Similarity Metrics:

### Semantical
Most of the proposals encountered so far look explicitly at the bytecodes that represent each program but there are a couple that look explicitly at their behavior
- Statistical and machine learning models: These approaches require training statistical or machine learning models on system calls emitted from different running versions of the code. This are used to detect deviations from normal (HMM and Anil's).

- Graph based similarity (Bindiff, Google): This technique attempts to recover and compare the control-flow and call graphs of a pair of binaries. As such it can only detect differences in the control-flow

## Syntactical

A natural way to compare the similarity between programs is by looking at the coincidences in their bytecodes, the obvious drawbacks being the failure to discover similarities in behavior due to synonymic instruction sequences as well as the "discovery" of false similarities due to the lack of context. They are  useful for assessing how different programs that have the same behavior look syntactically (this seems important for the question of how difficult it would be for code that infiltrates one to infiltrate the other).

A brief description and relationships of the metrics encountered so far.
- Instruction distributions: These metrics compare  the distributions of instruction frequencies between programs, they cam be computed for single instructions or for instruction n-grams. Examples of this metrics are instruction frequency [shell], n-gram frequency [shell], refined Jaccard [shell]. These metrics are a good first approximation, but fail to capture instruction sequence and can easily assign a high similarity score to two very distinct (both syntactically and semantically) programs.

- Instruction matching: These metrics measure instruction coincidence at specific positions within programs and as such capture more structure than the instruction distribution metrics.
For this, each program is considered as a point in n-dimensional space (usually these metrics add padding as needed so that the two programs being compared have the same length ( embedded in the same space)) and then some distance metric is applied such as:

    - Euclidean
    - Fractional (scales better that euclidean to higher dimensions)
    - and others like Mahalanobis and Hamming (whichever metric from linear algebra)

    One problem with this approaches is that they require a proper alignment between the instructions of the two programs. For this  a rotation $rot$ operator rotates the instructions in the program by an offset  with wraparound. This operator can be applied to any of the above metrics and in particular it gives rise to these two:

  - Rotated Euclidean [shell]: For each offset computes the (normalized) euclidean distance  between programs and returns the minimum value (corresponding to the rotation that creates the largest overlap)(a low value meaning high similarity)

  - Point match (from the slides): For each offset computes the fraction of instructions that coincide between the two programs (which is the normalized Hamming distance) and returns the highest value (corresponding the the offset that creates the largest overlap)(a low value meaning low similarity)

One drawback of these metrics is they are heavily affected by the insertion or deletion of instructions since they will cause offsets is only parts of the code (and the rotations rotate all the code).  Also, they only implicitly take into account instruction ordering (as identically ordered instructions sequences will yield high similarity) but random instruction coincidences can yield a similar score identical code snippets.


  - Our resent proposal consists of a transformation of the code (just like a rotation is a transformation) and then a measurement over the transformed programs. For these metric we assume that instruction ordering is important but try to reduce the impact of a few instruction insertions and/or deletions within fragments of code.
    - A very quick description (working on a better one and I don't want to clutter this document with it): Given two programs $P_1$ and $P_2$ we rewrite (transform) $P-2$ as a permutation of $P_1$'s instructions. After this transformation there are several ways to measure their similarity, we now employ Spearman's rank correlation  to assess how similar both representations are (Instruction Rank Similarity). There are still some things to work out but so far the results are good (we think).
  - Use diff (just an idea)



Preliminary observation: It seems to me that measures that look at behavior (semantics) are good for anomaly detection and syntactical metrics are good for assessing the diversity (as a proxy of vulnerability) of programs that are supposed to perform the same functions.

## Only odds and ends from here on down
## Population Diversity Metrics

- DHTD
- Shannon
- Jaccard
- Ediv


### Notes on Papers

1. Behavioral Distance Measurement Using
Hidden Markov Models:  This paper talks about identifying a malicious variant of a program. It does focus on the behavior of the program and operates by training a HMM on the normal occurring system calls during execution. The idea is to create a model using two simultaneous versions of the code running on the same input. I think the idea is that once the model is trained then to compare the similarity of two programs you execute them both and cheque for anomalies using the model if something is very different then you alert (as explained its not really a distance just alerts).

2. Similarity-based matching meets Malware Diversity: It has a summary of some similarity metrics. Jaccard and one that sounds like the chi-squared that I proposed. They mention another one, Bindiff, that is hard to compute but is graph based, focuses on program flow. They also explicitly look for common subsequences of size at least 10. Their technique effectively disrupts these.
 The paper talks about generating malware variants that are undetectable (it differ from regular programs in that they diversify static parts of the program data as well)

 3. On the Infeasibility of Modeling Polymorphic Shellcode: Introduces some divesity metrics to study the effectiveness of some malware techniques to generate hard to detect and hard to model variants. This diversity metrics include euclidean and spectral (seems more of a visualization to me, but in a way similar to mine). Variational and propagation strength are used to characterize the set of possible variants generated by a method rather than assessing the distance between to variants.
