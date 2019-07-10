## Functional Programming Principles in Scala -- Martin Odersky

#### main programing paradigms:
  1. imperative programming: modify mutable variables, using assignments, control structures such as if else etc
  2. functional programming: in a restricted sense, a FP language is one which does not have mutable variables, assignments or imperative control structures; in a wider sense, a FP language enables the construction of elegant programs that focus on **functions**. Functions can be defined anywhere including inside other functions. Functions can be passed as parameters to functions and returned as results. There are a set of operators to compose functions.
  3. logic programming
  4. object-oriented programming (orthogonal to above 3)
### FP evaluation strategy
  1. call by value: not lazy, reduces variable first and then evaluate (x:Int), default use of scala
  
          dsdfsd
          dsfsdfs
          sfsfs
        
  2. call by name: lazy evaluation, evaluate first and then reduce (x: => Int), less efficient than CBV
