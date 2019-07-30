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
  - the definition of conditional probability: <img src="https://latex.codecogs.com/svg.latex?P(E|F)&space;=&space;\frac{P(E\cap&space;F)}{P(F)}" title="P(E|F) = \frac{P(E\cap F)}{P(F)}" />, this holds even when outcomes are not equally likely
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
    <img src="https://latex.codecogs.com/gif.latex?P(AB^{c})&space;=&space;P(A)&space;-&space;P(AB)&space;=&space;P(A)&space;-&space;P(A)P(B)&space;=&space;P(A)[1-P(B)]&space;=&space;P(A)P(B^{c})" title="P(AB^{c}) = P(A) - P(AB) = P(A) - P(A)P(B) = P(A)[1-P(B)] = P(A)P(B^{c})" />
    - the first equality comes from lawy of total probability, P(A) = P(ABc) + P(AB)
  - general definition of independence is that for verey subset with r elements, P(E1...Er) = P(E1)...P(Er)
  - example, p is the probability to get head by rolling a dice, what is P(exactly k heads on n coin flips)
    - each coin flip are independent and the probablity of getting one case of k head is: <img src="https://latex.codecogs.com/gif.latex?P(F_{i})&space;=&space;p^{k}(1-p)^{n-k}" title="P(F_{i}) = p^{k}(1-p)^{n-k}" />
    - so the total probablity is: <img src="https://latex.codecogs.com/gif.latex?\binom{n}{k}p^{k}(1-p)^{n-k}" title="\binom{n}{k}p^{k}(1-p)^{n-k}" />
- set operations
  - DeMorgan's laws: <img src="https://latex.codecogs.com/gif.latex?(E\cup&space;F)^{c}&space;=&space;E^{c}&space;\cap&space;F^{c}" title="(E\cup F)^{c} = E^{c} \cap F^{c}" />; <img src="https://latex.codecogs.com/gif.latex?(E\cap&space;F)^{c}&space;=&space;E^{c}&space;\cup&space;F^{c}" title="(E\cap F)^{c} = E^{c} \cup F^{c}" />
- conditional independence:
  - in the conditional paradigm, the formulas of probability are preserved
  - <img src="https://latex.codecogs.com/gif.latex?P(A|BE)&space;=&space;\frac{P(B|AE)P(A|E)}{P(B|E)}" title="P(A|BE) = \frac{P(B|AE)P(A|E)}{P(B|E)}" />, similar to the previous equation, just add condition on E for every term
  - independence relationships can change with conditioning (if events E and F are independent, that does not mean they will still be independent given another event G)
  - two events E and F are conditionally independent given G if P(EF|G) = P(E|G)P(F|G) or P(E|FG) = P(E|G)
- Random variables: it is a variable will have a value but there is uncertainty as to what value
  - for instance, Y = number of "heads" on 3 coins. Y is a random variable, Y could be 0, 1, 2, 3
  - with random variable, we can do:
    - probability mass function P(X=a) which means the probability of a random variable take particular value. Can think of it as a function, given a value, output a probability. Notice the random variable is **discrete**
      - the notation could also be written as p(x) or <img src="https://latex.codecogs.com/gif.latex?P_{X}(x)" title="P_{X}(x)" />
    - expectation E[X]
      - for discrete random variable X: <img src="https://latex.codecogs.com/gif.latex?E[X]&space;=&space;\sum&space;_{x:p(x)>0}x*p(x)" title="E[X] = \sum _{x:p(x)>0}x*p(x)" />, in other words, sum over all values of x that have PMF bigger than 0
      - other semantic meaning: mean, weighted average, center of mass, 1st moment
      - properties:
        - linearity: E[aX+b] = aE[x] + b
        - expectation of a sum is the sum of expectations: E[X + Y] = E[X] = E[Y]
        - unconscious statistician: <img src="https://latex.codecogs.com/gif.latex?E[g(x)]&space;=&space;\sum&space;_{x}&space;g(x)p(x)" title="E[g(x)] = \sum _{x} g(x)p(x)" />
    - variance: Var(X)
      - think about value x and the difference to the mean E[x]. How to get the average difference, one way is to do average of the absolute value of difference (L1 norm); the other way is to average over the square of the difference (L2 norm)
      - we use L2 norm in variance and variance is a formal representation of "spread"
      - <img src="https://latex.codecogs.com/gif.latex?Var(x)&space;=&space;E[(x-\mu&space;)^{2}]" title="Var(x) = E[(x-\mu )^{2}]" /> for X being a random variable = <img src="https://latex.codecogs.com/gif.latex?\sum&space;_{x}&space;(x-\mu&space;)^{2}p(x)" title="\sum _{x} (x-\mu )^{2}p(x)" /> = <img src="https://latex.codecogs.com/gif.latex?E[x^2]&space;-&space;(E[x])^2" title="E[x^2] - (E[x])^2" />
      - standard deviation is the square root of variance
      - also called 2nd central moment
      - properties of variance:
        - var(aX+b) = a^2Var(x)
  - Bernoulli Random Variable
    
      
  
