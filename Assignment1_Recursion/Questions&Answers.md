# Exercise 1: Pascal¡¯s Triangle
The following pattern of numbers is called Pascal¡¯s triangle.
```
    1
   1 1
  1 2 1
 1 3 3 1
1 4 6 4 1
   ...
```
The numbers at the edge of the triangle are all 1, and each number inside the triangle is the sum of the two numbers above it. Write a function that computes the elements of Pascal¡¯s triangle by means of a recursive process.

Do this exercise by implementing the pascal function in Main.scala, which takes a column c and a row r, counting from 0 and returns the number at that spot in the triangle. For example, pascal(0,2)=1,pascal(1,2)=2 and pascal(1,3)=3.
```scala
 def pascal(c: Int, r: Int): Int
```

## Answer
When an element is the first or the last in a row, it should be 1, otherwise suppose it's in coloumn c and row r, the answer should be the sum of pascal(c - 1, r - 1) and pascal(c , r - 1). So it can be written as follows: 
```scala
 def pascal(c: Int, r: Int): Int =
      if(c == 0 || c == r) 1 else pascal(c - 1, r - 1) + pascal(c , r - 1)
```
# Exercise 2: Parentheses Balancing
Write a recursive function which verifies the balancing of parentheses in a string, which we represent as a List[Char] not a String. For example, the function should return true for the following strings:
* (if (zero? x) max (/ 1 x))
* I told him (that it¡¯s not (yet) done). (But he wasn¡¯t listening)
  
The function should return false for the following strings:
* :-)
* ())(
The last example shows that it¡¯s not enough to verify that a string contains the same number of opening and closing parentheses.

Do this exercise by implementing the balance function in Main.scala. Its signature is as follows:
```scala
def balance(chars: List[Char]): Boolean
```
## Answer
First, we scan the List one by one and maitain a variable called ' n ' which is the num of ' ( ' waiting to be closed. When Scanning, we check the char: 
* If it is ' ( ' then add n by 1.  
* If it is ' ) ' then minus n by 1, but if n is already 0  then it is unbanlanced
```scala
def balance(chars: List[Char]): Boolean = {
      def check(n: Int, chars: List[Char]):Boolean =
        if(chars.isEmpty)             n == 0
        else if(chars.head == '(')   check(n + 1, chars.tail)
        else if(chars.head == ')')   if(n == 0) false else check(n - 1, chars.tail)
        else                         check(n, chars.tail)
      check(0, chars)
    }
```
# Exercise 3: Counting Change

Write a recursive function that counts how many different ways you can make change for an amount, given a list of coin denominations. For example, there are 3 ways to give change for 4 if you have coins with denomination 1 and 2: 1+1+1+1, 1+1+2, 2+2.

Do this exercise by implementing the countChange function inMain.scala. This function takes an amount to change, and a list of unique denominations for the coins. Its signature is as follows:
```scala
def countChange(money: Int, coins: List[Int]): Int
```
## Answer
Given a list of coin denominations, ways you can make change for an amount could be separated into two parts:
* The ways that use the first kind of coin
* The ways that don't use the first kind of coin

And there are two terminate conditions for recursion:
* Money amount equals 0, that means this way works and it should be counted as 1
* There are no more kinds of coins or Money amount is negative, that means this way doesn't work and it should be count as 0
```scala
def countChange(money: Int, coins: List[Int]): Int =
      if(money == 0)                        1
      else if(coins.isEmpty || money < 0)   0
      else                                  countChange(money - coins.head, coins) + countChange(money, coins.tail)
  }
```
