# Representation

We will work with sets of integers.

As an example to motivate our representation, how would you represent the set of all negative integers? You cannot list them all… one way would be so say: if you give me an integer, I can tell you whether it’s in the set or not: for 3, I say ‘no’; for -1, I say yes.

Mathematically, we call the function which takes an integer as argument and which returns a boolean indicating whether the given integer belongs to a set, the characteristic function of the set. For example, we can characterize the set of negative integers by the characteristic function (x: Int) => x < 0.

Therefore, we choose to represent a set by its characteristic function and define a type alias for this representation:
```scala
type Set = Int => Boolean
```
Using this representation, we define a function that tests for the presence of a value in a set:
```scala
def contains(s: Set, elem: Int): Boolean = s(elem)
```

# 2.1 Basic Functions on Sets

Let’s start by implementing basic functions on sets.

1. Define a function singletonSet which creates a singleton set from one integer value: the set represents the set of the one given element. Now that we have a way to create singleton sets, we want to define a function that allow us to build bigger sets from smaller ones.
2. Define the functions union,intersect, and diff, which takes two sets, and return, respectively, their union, intersection and differences. diff(s, t) returns a set which contains all the elements of the set s that are not in the set t.
3. Define the function filter which selects only the elements of a set that are accepted by a given predicate p. The filtered elements are returned as a new set.

## Answer
```scala
  /**
   * Returns the set of the one given element.
   */
    def singletonSet(elem: Int): Set =
      x => x == elem
  

  /**
   * Returns the union of the two given sets,
   * the sets of all elements that are in either `s` or `t`.
   */
    def union(s: Set, t: Set): Set =
      x =>  s(x) || t(x)

  /**
   * Returns the intersection of the two given sets,
   * the set of all elements that are both in `s` and `t`.
   */
    def intersect(s: Set, t: Set): Set =
      x => s(x) && t(x)

  /**
   * Returns the difference of the two given sets,
   * the set of all elements of `s` that are not in `t`.
   */
    def diff(s: Set, t: Set): Set =
      x => s(x) && !t(x)

  /**
   * Returns the subset of `s` for which `p` holds.
   */
    def filter(s: Set, p: Int => Boolean): Set =
      x => s(x) && p(x)
```

# 2.2 Queries and Transformations on Sets
In this part, we are interested in functions used to make requests on elements of a set. The first function tests whether a given predicate is true for all elements of the set. This forall function has the following signature:
```scala
def forall(s: Set, p: Int => Boolean): Boolean
```
Note that there is no direct way to find which elements are in a set. contains only allows to know whether a given element is included. Thus, if we wish to do something to all elements of a set, then we have to iterate over all integers, testing each time whether it is included in the set, and if so, to do something with it. Here, we consider that an integer x has the property -1000 <= x <= 1000 in order to limit the search space.

1. Implement forall using linear recursion. For this, use a helper function nested inforall.
2. Using forall, implement a function exists which tests whether a set contains at least one element for which the given predicate is true. Note that the functions forall and exists behave like the universal and existential quantifiers of first-order logic.
3. Finally, write a function map which transforms a given set into another one by applying to each of its elements the given function.

## Answer
For forall, the implementation is quite straight.  
For exits, notice that exists(s, p) is logically opposite to forall(s, inverse-p).  
For map, notice that if x is in the set map(s, f), there must be an element y in the set s which f(y) equals x.   
```scala
 /**
   * The bounds for `forall` and `exists` are +/- 1000.
   */
  val bound = 1000

  /**
   * Returns whether all bounded integers within `s` satisfy `p`.
   */
    def forall(s: Set, p: Int => Boolean): Boolean = {
    def iter(a: Int): Boolean = {
      if (a > bound) true
      else if (s(a) && !p(a)) false
      else iter(a + 1)
    }
    iter(-bound)
  }
  
  /**
   * Returns whether there exists a bounded integer within `s`
   * that satisfies `p`.
   */
    def exists(s: Set, p: Int => Boolean): Boolean =
      !forall(s, x => !p(x))
  
  /**
   * Returns a set transformed by applying `f` to each element of `s`.
   */
    def map(s: Set, f: Int => Int): Set =
      x => exists(s, y => f(y) == x)
```
