# racket-problems

## This repository contains functional programming problems and solutions using Racket

### What is Racket?

Racket is a general-purpose programming language as well as the world’s first ecosystem for developing and deploying new languages.

### Getting started with Racket!
You can find the full information [here](https://docs.racket-lang.org/getting-started/index.html), but here's the summary
 1. Download and Install Racket, get it [here](http://racket-lang.org/download/)
 2. Once installed launch Dr. Racket
 
 1. On Windows, you can start DrRacket from the Racket entry in the Start menu.
 2. On Mac OS, double click on the DrRacket icon. It is probably in a Racket folder that you dragged into your Applications folder.
 3. On Unix (including Linux), the drracket executable can be run directly from the command-line if it is in your path, which is probably the case if you chose a Unix-style distribution when installing.

 ## The Problems!

 Listed below are a few relatively simple problems that need to be solved in scheme using only the following:
 ``cons, car, cdr, define, lambda, if, null, null?, cond, map, = ``and the basic numerical operators ``(+, -, *, /, modulo)``.

  ## 1. Prime numbers
 
##### First of all, what is a prime number?
A prime number is a natural positive number which greater than one, and is divisible only by itself and one.

##### How do we prove that a number is a prime number? 
Well its relatively simple, we could use a brute force algorithm to check if the number (x) is divisible by the iteration (y). If it is divisible by only one and itself then its prime.. otherwise its not. While the brute force algorithm will eventually return the result we're looking for it has the potential to be expensive in time and complexity the higher the (x) value gets. Numbers which are not prime will be divisible by the following prime numbers: [2, 3, 5, 7].

##### So how could we lower the cost? 
To lower the complexity and cost of the algorithm we could just check the numbers less than the square root of (x) this will still return the same result at a fraction of the cost. Why is this less expensive? We're checking only a fraction of numbers which the brute force algorithm will check.

[Here](https://en.wikipedia.org/wiki/Prime_number) is some information from Wikipedia if you want to know more about prime numbers...

#### Conclusion
For the problem these were my solution:
````
  (define m 2)

  (define (decide-prime n)
        (if (< n 2) ; 1
          #f
          (recursion n m))); 2

  (define (recursion n m)
    (if (= m n); 3 - expensive (brute force)
        #t
        (if (= (modulo n m)0) ; 4
            #f
            (recursion n (+ m 1))))); 5

  ; optimal (check only while m is greater than the square root of n)
  (define (decide-prime-optimal n)
        (if (< n 2) ; 1
          #f
          (recursion-optimized n m))); 2

  (define (recursion-optimized n m)
    (if (> m (integer-sqrt n)) ; 3 (optimal)
        #t
        (if (= (remainder n m)0) ; 4
            #f
            (recursion-optimized n (+ m 1))))); 5
````


 ## 2. Collatz Conjecture
Write, from scratch, a function in Racket that takes a positive integer n0 as input and returns a list by recursively applying the following operation, starting with the input number: (Equal: n/2 | Odd: n * 3 + 1) until n is 1.

##### What is Collatz Conjecture
Collazt Conjecture is a mathematical conjecture concerning a sequence that begins with a positive natural number which will always end at 1 following the algorithm above.

##### How do we prove that this is true? 
Collatz Conjecture has yet to be proven true, most research into the conjecture come to same conclusion that it is true via experimental evidence and heuristic arguments in support of it.

##### Experimental & Heuristics
 * As of December 16th 2017, the conjecture has been checked for all starting values ranging up to 87*2^60 using computers. All of these values eventually would end in a cycle {4, 2, 1}. The full article can be found [HERE](http://www.ericr.nl/wondrous/)

 * If we only account for the odd numbers within' the sequence generated by the process, each odd number will be, on average ^3/_4 than the previous odd value. The heuristic argument suggests that on average the process iterates in a trajectory that will tend to shrink in size, so that divergent trajectories should not exist. This argument can be found [HERE](http://www.cecm.sfu.ca/organics/papers/lagarias/paper/html/node3.html) 

#### Conclusion 
While it is yet to be proven true, the research done shows strong evidence that suggests it is. In anycase here is my solution:

````
  (define (collatz-list n)
  (cond
    ((= n 1) '(1)); 1
    ((= (modulo n 2) 0) ; 2
      (cons n (collatz-list (/ n 2)))); 3
    ((= (modulo n 2) 1) ; 4
      (cons n (collatz-list (+ (* 3 n) 1)))))); 3

````

## 3. Left & Right list cycles
Write, from scratch, two functions in Racket. The first is called lcycle (left cycle). It takes a list as input and returns the list cyclically shifted one place to the left. The second is called rcycle (right cycle), and it shifts the list cyclically shifted one place to the right.

#### How do we do it?
To solve the problem we need to make a couple of auxiliary functions which will do the following: get the last element of a list (``peek()``), remove the last element of a list (``pop()``), & append to a list (``append()``). These functions will simplify the code within' lcycle and rcycle functions. So lets take a look at each one individually.

##### Peek
The ``peek`` function should mimic that of a ``List`` function that you may have used in Java. So lets look at that:
> "The java.util.LinkedList.peek() method retrieves, but does not remove, the head (first element) of this list."

Ok we already have ``car`` which will do this for us, so we'll create one for the final element. We recursively check until we reach the end of the list once we do return the value.

 ##### Pop
 Similar to ``peek`` the ``pop`` function, but instead of returning the the final element it will return every other element of the list.

 ##### Append 
Since the requirements specify that we cannot use the built in ``append`` function, we'll made our own using cons. Recurse through the list adding the first element on each iteration into the returned list.

Now that we have all these auxiliary functions we can tackle both of the cycles

##### Left cycle
Use `cdr` of the non empty list l = '(1 2 3) for example, to return another list '(2 3), the 2nd element of the a pair: where the first element is the `car` of l and the second is the `cdr` of l. We then get the `car` of l to return the first element of the pair = 1. Wrap those two functions in the `append` function and we should get: '(2 3 1)

##### Right cycle
`cons` the `peek` onto the `pop`: where the `peek` of a list '(1 2 3) = 3 and the `pop` = '(1 2) which gives us the the result '(3 1 2)

#### Conclusion
The best solution I found to this problem was the following solution:

````
  (define (lcycle l)
    (app (cdr l) (car l)))

  (define (rcycle l)
    (cons (peek l) (pop l)))
    
  (define (peek n)
    (if (null? (cdr n)) 
      (car n)          
      (peek (cdr n)))) 

  (define (pop n)
    (if(null? (cdr n))
      '() 
        (cons (car n) (pop (cdr n)))))

  (define (app l m)
    (if (null? l) '(l)
      (cons (car l) (app (cdr l) m))))
````

 
