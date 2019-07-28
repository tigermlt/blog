- Events: A thing that can happen

- Experiments: Have events as outcomes

- Inclusion-Exclusion Principle:
  - If outcomes can come from set A or B, then the total number of outcomes is:
    <img src="https://latex.codecogs.com/svg.latex?|A&space;\cup&space;B|&space;=&space;|A|&space;&plus;&space;|B|&space;-&space;|A&space;\cap&space;B|" title="|A \cup B| = |A| + |B| - |A \cap B|" />

- Product rule of counting:
  - If outcomes are generated via a process with r steps, where step i has ni outcomes, then the total number of outcomes is: n1 * n2 * ... * nr
  
- combinatorics:
  - sort objects (permutation)
    - distinct: if n is the total number of elements to sort, then the total number of ways to sort is n!
    - semi-distinct: if there are n objects, n1 are the same (within the group), n2 are the same (with in the group) ...
    then there are <img src="https://latex.codecogs.com/svg.latex?\frac{n!}{n_{1}!...n_{r}!}" title="\frac{n!}{n_{1}!...n_{r}!}" /> unique orderings (permutations)
  - choose k objects (combinations) from distinct items
    - for instance, there are n = 20 people, choose k = 5 people to get cake and the ordering of those 5 does not matter
    - <img src="https://latex.codecogs.com/svg.latex?\binom{n}{k}&space;=&space;\frac{n!}{k!(n-k)!}" title="\binom{n}{k} = \frac{n!}{k!(n-k)!}" />
  - put objects into r buckets
    - items are distinct: r^n where n is number of items
    - items are not distinct: <img src="https://latex.codecogs.com/svg.latex?\frac{(n&plus;r-1)!}{n!(r-1)!}" title="\frac{(n+r-1)!}{n!(r-1)!}" /> 
- Axioms of probability:
  - 0 <= P(E) <= 1
  - P(S) = 1
  - P(E^c) = 1 - P(E) or P(E union F) = p(E) + p(F) if event E and F are mutually exclusive
- conditional probability: it is the probability that E occurs given that F has already occurred
  - written as P(E|F) : means P(E, given F already observed)
  - <img src="https://latex.codecogs.com/svg.latex?P(E|F)&space;=&space;\frac{P(E\cap&space;F)}{P(F)}" title="P(E|F) = \frac{P(E\cap F)}{P(F)}" />, this holds even when outcomes are not equally likely
  - In general: <img src="https://latex.codecogs.com/svg.latex?P(E1E2...En)&space;=&space;P(E1)P(E2|E1)...P(En|E1E2...En-1)" title="P(E1E2...En) = P(E1)P(E2|E1)...P(En|E1E2...En-1)" />
  - ![paradigm](https://tigermlt.github.io/blog/conditional_paradigm.png)
  - machine learning is: probability + data + computers
- Law of total probability:
  - <img src="https://latex.codecogs.com/svg.latex?P(E)&space;=&space;\sum&space;_{i}P(B_{i}\cap&space;E)&space;=&space;\sum&space;_{i}P(E&space;|B_{i})&space;\times&space;P(B_{i})" title="P(E) = \sum _{i}P(B_{i}\cap E) = \sum _{i}P(E |B_{i}) \times P(B_{i})" />
  - Bi are mutually exclusive
- Bayes Theorem
  - <img src="https://latex.codecogs.com/gif.latex?P(E|F)&space;=&space;\frac{P(F|E)P(E)}{P(F)}" title="P(E|F) = \frac{P(F|E)P(E)}{P(F)}" />
  - <img src="https://latex.codecogs.com/gif.latex?P(E|F)&space;=&space;\frac{P(F|E)P(E)}{P(F|E)P(E)&space;&plus;&space;P(F|E^{c})P(E^{c})}" title="P(E|F) = \frac{P(F|E)P(E)}{P(F|E)P(E) + P(F|E^{c})P(E^{c})}" /> it is also equal to the above equation after applying low of total probability
  - update belief
    - we have some P(location) before observation, know P(observation|location), update P(location)
    - after observation, we want to compute P(L|O), use bayes theorem and law of total probability to compute it
- independence
  - if two events E and F are mutually exclusive, then P(E union F) = P(E) + P(F)
  - if two events E and F are not mutually exclusive, then P(E union F) = P(E) + P(F) - P(EF)
  - for three sets, P(E union F union G) = P(E) + P(F) + P(G) - p(EF) - p(EG) - P(FG) + P(EFG). So if there is no mutually exclusion, life will be hard
  - two events A and B are independent if P(AB) = P(A)P(B) otherwise they are dependent events. Another definition for indepedent is P(A|B) = P(A)
  - if events A and B are independent, prove that A and B compliment are independent
    
  
  
