## Functional Programming Principles in Scala -- Martin Odersky

#### main programing paradigms:
  1. imperative programming: modify mutable variables, using assignments, control structures such as if else etc
  2. functional programming: in a restricted sense, a FP language is one which does not have mutable variables, assignments or imperative control structures; in a wider sense, a FP language enables the construction of elegant programs that focus on **functions**. Functions can be defined anywhere including inside other functions. Functions can be passed as parameters to functions and returned as results. There are a set of operators to compose functions.
  3. logic programming
  4. object-oriented programming (orthogonal to above 3)
### FP evaluation strategy
  1. call by value: not lazy, reduces variable first and then evaluate (x:Int), default use of scala
  
          sumOfSqures(3, 2+2)
          sumOfSqures(3,4)
          squre(3) + squre(4)
          3*3 + square(4)
          9 + square(4)
          9 + 4*4
          9 + 16
          25
        
  2. call by name: lazy evaluation, evaluate first and then reduce (x: => Int), less efficient than CBV
  
          sumOfSqures(3, 2+2)
          square(3) + square(2+2)
          3*3 + squre(2+2)
          9 + square(2+2)
          9 + (2+2) * (2+2)
          9 + 4 * (2+2)
          9 + 4 * 4
          25
### conditions and value definitions
  1. **def** is like CBN, its right hand side is evaluated only when it is used
  2. **val** is like CBV, its right hand side is evaluated immediately
          Implement and(x,y)
          
          Def and(x: Boolean, y:=> Boolean) = {
            if (x) y
            else false
          }
          
          Notice that y is CBN because if we call (false, loop), the CBV version will go into infinite loop
          
### square roots with Newton's method

  **A typical way to code algorithms in FP is to go step by step. So we take a small task and formulate it as a function and then probably that task will need further tasks that would be defined in their own function.**
  
  For instance, to implement the square function using Newton's method, we can sketch as follows:
  
    def sqrtIter(guess: Double, x: Double): Double = {
      if (isGoodEnough(guess, x)) guess
      else sqrtIter(improve(guess,x), x)
    }
    
    and then we can continue to fill isGoodEnough and improve
    side note: recursive functions need an explicit return type in scala (: Double in sqrtIter) in scala. For non-recursive functions, the return type is optional

### Tail recursion

  If a function **calls itself** as its last action, the function's stack frame can be reused. This is called tail recursion. Tail recursive functions are iterative processes.
  Broadly speaking, if the last action of a function consists of calling a function (which may be the same), one stack fram would be sufficient for both functions. Such calls are called tail calls.
  
  
    def gcd(a: Int, b: Int): Int = {
      if (b==0) a
      else gcd(b, a%b)
    }
    this is tail recursion.
    
    Factorial is not tail recursion, because besieds calling itself, it also multiply a number: factorial(n) = n*factorial(n-1)
    

### Higher-order functions
  Functions that take other functions as parameters or that return functions as results are called higher order functions.
  For example, write a general sum function (take function as parameter)
  
    def sum(f: Int=>Int, a: Int, b: Int): Int = {
      if (a>b) 0
      else f(a) + sum(f, a+1, b)
    }
    
    we can then write:
    def sumInts(a: Int, b: Int) = sum(id, a, b) where def id(x: Int): Int = x
    def sumCubes(a: Int, b:Int) = sum(cube, a, b) where def cube(x: Int): Int = x*x*x
    def sumFactorials(a: Int, b: Int) = sum(fact, a, b) where def fact(x: Int): Int = if(x==0) 1 else x*fact(x-1)
  
  Anonymous function:
  

          
