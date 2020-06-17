---
date: "2014-09-28"
tags: ["hugo", "theme", "command-line"]
title: "Counting Change"
---



### Hello World



When I first began my formal study of computer science at high school, my teacher asked me to write a program that would dispense change from a self-service machine. 

The rules were simple. My machine would only have 10p, 5p, 2p, and 1p coins. When it was time to dispense change, my program would take as input the amount in pennies and dispense the minimum number of coins required to make the correct change. For the sake of pedagogy, my teacher told us that we would never run out of coins.

A quick sketch of a sample input might look like this,

```
input  : 31  // 31p to dispense in change
output : 10, 10, 10, 1 (4 coins)
```



Easy. We subtract the largest coin from the number, until we can no longer do so without getting a negative number. Then we subtract the second largest coin from the number. So on, and so forth, until the machine has dispensed all the change.

```
input : 31
31 - 10 = 21 (OK, drop a 10)
21 - 10 = 11 (OK, drop a 10)
11 - 10 = 1  (OK, drop a 10)
1  - 10 = -9 (NO, try a lower coin)
1  -  5 = -4 (NO, try a lower coin)
1  -  2 = -1 (NO, try a lower coin)
1  -  1 = 0  (OK, drop a 1)
0            (DONE)
```

I coded this algorithm up, e-mailed it to my teacher, and continued on to the next problem. 

{{< expandable label="Code Snippet #1" level="2" >}}
Code snippets in this blog post are written in the Python programming language. It's not necessary to have a background in programming to understand the ideas in this blog, but these tabs will include code snippets for those curious.

```
def drop_coin(n):
	# Dummy function, emulates machine dropping a coin

def make_change(amount):
	count = 0
	while amount >= 0:
		if amount - 10 >= 0:
			# Drop a 10p coin
			drop_coin(10)
			amount = amount - 10
		elif amount - 5 >= 0:
			# Drop a 5p coin
			drop_coin(5)
			amount = amount - 5
        elif amount - 1 >= 0:
        	# Drop a 1p coint
        	drop_coin(1)
        	amount = amount - 1
		count = count + 1
		
```

{{< /expandable >}}



------



### Don't Be Naive

The "counting change" problem came up again during my first year of university. I was in an afternoon lecture on the theory of computation. My lecturer used my solution to the problem as a throwaway example of a "greedy algorithm". I thought the title rather appropriate.



Then he threw a wrench. Suppose we loosen the constraints on change and allow non-standard coins. Instead of 1p, 5p, 10p, coins, we might have 1p, 3p, 4p. How does our greedy algorithm hold up?

```
input  : 6
output : 4, 1, 1
```

Not well. For `6`, the minimal change is 2 coins (as 6 = 2*3).  Our greedy algorithm spits out 3.

I thought about this for a bit. The problem was more complex, so it follows that the solution should be as well. There are multiple ways to make 6 out of the coins (1,3,4). All we have to do is pick the minimum.

```
coins : [1,3,4]
input : 6
---
6 | 1, 1, 1, 1, 1, 1
6 | 1, 1, 1, 3
6 | 4, 1, 1
6 | 3, 3
```

I tried to think about an efficient way to compute all these possibilities, but the class moved on. The next topic was Dijkstra's Algorithm, finding the shortest path from A to B. Dijkstra made sure to avoid any distractions, so I decided to do the same.

------



### Changing Perspective

This problem came up a third time in my third year. It was time to think about jobs. For computer science undergraduates this means sweating blood solving puzzles like this so that, at an interview with a top tech firm, you can solve a puzzle like this. The change counting problem was one of them. I coded up the naive solution, and submitted it. My program ran, and failed. It had taken too long. As the number to make change from increases, the number of possible coin combinations grows exponentially. A new approach is needed.

Recall the conditions that crashed our greedy algorithm, `coins = [1, 3, 4], input = 6`.  The correct answer is 2 coins, as 6 = 2 * 3. You didn't have to calculate this, you knew it. To be precise, you _remembered_ it. When you were younger, primary school maths teachers drilled times tables into your memory. They installed a lookup table into your brain, that you could call on whenever needed.

This lookup table is not very large. For most of us, it stops at 144 = 12*12. The human mind has far more interesting things to commit to memory than a table of numbers. Computers, on the other hand, do not. And they can remember a great deal. 

This idea, that it is faster to remember an answer than to calculate it, is a fundamental pillar in algorithmic thinking. We call it "time versus space complexity." When multiplying, is it easier to make space to remember the answer, or take the time to calculate it?

The answer is somewhere in between.

This intuition can be applied to the change counting problem. Instead of calculating each answer, it may be better to have a lookup table. We can construct a table of coins (**C**) against value (**V**). The leftmost column are the denominations available. The top row is the possible change the coins should be able to make. Each entry in the row is set of pairs (*coin*, *number*) that represents the **minimum** set of coins needed to make change.

| C/V   |   1   |  2   |   3   |   4   |  5   |  6   |
| ----- | :---: | :--: | :---: | :---: | :--: | :--: |
| **1** | (1,1) |  -   |   -   |   -   |  -   |  -   |
| **3** |   -   |  -   | (3,1) |   -   |  -   |  -   |
| **4** |   -   |  -   |   -   | (4,1) |  -   |  -   |

At first our table contains only the basics. By inspection, we can fill out some of the other entries in the table. Here's my one,

| C/V   |   1   |  2   |   3   |      4         |   5   |   6    |
| ----- | :---: | :--: | :---: | :------------: | :---: | :----: |
| **1** | (1,1) | (1,2) |   (1,3)   |      (1,4)      |   (1,5)   |  (1,6)  |
| **3** |   -   |  -   | (3,1) | (3,1)<br />(1,1) |   (3,1)<br />(1,2)   | (3,2) |
| **4** |   -   |  -   |   -   |    (4,1)       | (4,1) (1,1) |  (3,2)<br />  |

I want to draw your attention to three cells, **[2,1]**, **[5,4]**, and **[6,3]**. 

#### [2,1] | 2 = 1 + 1

If you have one penny, add a second and you get two pennies. Abstracting a little, we know the optimal solution for the *value*, 1. It follows that the optimal solution for the *value*, 2, is the optimal solution for 1, add one extra coin. 

Suppose we define a function, O(), that takes as input the change you want, and returns the minimum number of coins. We know,

$$O(1) = 1$$

because you just use the *coin*, 1. Then,

$$O(2) = O(1) + \text{one coin}$$

In that case, that extra coin is the 1p coin. So,

$$O(2) = 2$$

####  [5,4] | 5 = 4 + 1

Let's apply the same logic as before. We know,

$$O(4) = 1$$

because you use the *coin*, 4. Then,

$$O(5) = O(4) + \text{one coin}$$

Again, that extra coin is the 1p coin. So,

$$O(5) = O(4) + 1 = 2$$

#### [6,3] | 6 = 3 + 3

It first seems that the method breaks down. 

$$O(5) = 2$$

from above. Then,

$$O(6) = O(5) + \text{one coin} = O(5) + 1 = 3$$

Which we know to be wrong. Looking a little closer we realise that,

$$O(3) = 1 $$

and,

$$O(6) = O(3) + \text{one coin}$$ 

This time, that extra coin is the 3p coin. It's still just _one_ extra coin though, so,

$$O(6) = O(3) + 1 = 2$$

#### [?,?] | m = n + 1

There is a common theme with all of these examples. Each one is obtained by adding one coin to a value we already know. Expressing it another way,

$$\text{new change needed} = \text{old change needed} + \text{one extra coin}$$

Or,

$$O(m) = O(n) + 1, \quad n < m$$

Every value of change is one coin more than another, smaller value of change. We have three coins to choose from, (1, 3, 4). This leaves us with three cases. 

$$\text{(1)} \quad O(m) = O(m - 1) + 1, \quad \text{For the 1p coin}$$

$$\text{(2)} \quad O(m) = O(m - 3) + 1, \quad \text{For the 3p coin}$$

$$\text{(3)} \quad O(m) = O(m - 4) + 1, \quad \text{For the 4p coin}$$

One of these three cases will leave us with the smallest number. 





This concept, called "dynamic programming", isn't easy. To quote a friend of mine who works in software for an investment bank, 

> "dynamic programming is super hard. [$$$ Bank] doesn't ask dynamic programming questions for interviews"



