---
date: "2014-09-28"
tags: ["hugo", "theme", "command-line"]
title: "counting change"
---



## hello world

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

```python
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



## i can do everything

The "counting change" problem came up again during my first year of university. I was in an afternoon lecture on the theory of computation. My lecturer used my solution to the problem as a throwaway example of a "greedy algorithm". I thought the title rather appropriate.



Then he lobbed a wrench at it. Suppose we loosen the constraints on change and allow non-standard coins. Instead of 1p, 5p, 10p, coins, we might have 1p, 3p, 4p. How does our greedy algorithm hold up?

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
6 | 3, 1, 1, 1
6 | 3, 3
6 | 4, 1, 1
```

The final step is to figure out a way to calculate all these possibilities. The sketch from above gives a suggestion. Use the same greedy approach, but restrict the coins available to use

```
coins : [1,3,4]
input : 6
---
6 | 1, 1, 1, 1, 1, 1 (using 1)
6 | 3, 3 (using 1 & 3)
6 | 4, 1, 1 (using 1 & 3 & 4)
```

This seems to work okay, what about a larger number?

```
coins : [1,4,6]
input : 45
---
45 | 1, ... [44 more]  (using 1)
45 | 4, ... [10 more], 1  (using 1 & 3)
45 | 6, ... [6  more], 1, 1, 1 (using 1 & 3 & 4)
```

It gets to the right answer, but there's so much needless work. This method tries lots of different options before settling on the best one. 

I tried to think about a more efficient way to compute all these possibilities, but I was out of time. The class was moving on. The next topic was Dijkstra's Algorithm, finding the shortest path from A to B. Dijkstra made sure to avoid any distractions, so I decided to do the same.



{{< expandable label="Code Snippet #2" level="2" >}}
Code for the naive brute force algorithm

```python
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



## changing perspective

#### back to school

This problem came up a third time in my third year. It was time to think about jobs. For computer science undergraduates this means sweating blood solving puzzles like this so that, at an interview with a top tech firm, you can solve a puzzle like this. The change counting problem was a popular example. I coded up the naive solution, and submitted it. My program ran, and failed. It had taken too long. As the number to make change from increases, the number of possible coin combinations grows exponentially. I needed to take a step back.

Recall the conditions that crashed our greedy algorithm, `coins = [1, 3, 4], input = 6`.  The correct answer is 2 coins, as 6 = 2 * 3. You didn't have to calculate this, you knew it. To be precise, you _remembered_ it. When you were younger, primary school maths teachers drilled times tables into your memory. They installed a lookup table into your brain, that you could call on whenever needed. 

This lookup table is not very large. For most of us, it stops at 144 = 12*12. The human mind has far more interesting things to commit to memory than a table of numbers. Computers, on the other hand, do not. And they can remember a great deal. 

This idea, that it is faster to remember an answer than to calculate it, is a fundamental pillar in algorithmic thinking. We call it "time versus space complexity." When multiplying, is it better to make space to remember the answer, or take the time to calculate it?

The answer is somewhere in between. Suppose you are asked to answer a multiplication question, 14 times 12. You - certainly I - would struggle to remember the answer. Adding 12 together 14 times in your head is a little too time-consuming. The most efficient way is to recall from your mental lookup table that 12\*12 = 144 and then add the remaining 2*12 = 24, to get 14 * 12 = 168. As an algorithm, it might look something like this,

1. Remember the answer to a smaller multiplication
2. Combine the remembered answers to answer the bigger multiplication

#### cashing it in

This intuition can be applied to the change counting problem. Instead of calculating each answer on the fly, it may be better to have a lookup table. We can construct a table of coins (**C**) against value (**V**). The leftmost column are the denominations available. The top row is the possible change the coins should be able to make. Each entry in the row is set of pairs (*coin*, *number*) that represents the **minimum** set of coins needed to make change.

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

##### [2,1] | 2 = 1 + 1

If you have one penny, add a second and you get two pennies. Abstracting a little, we know the optimal solution for the *value*, 1. It follows that the optimal solution for the *value*, 2, is the optimal solution for 1, add one extra coin. 

Suppose we define a function, C(), that takes as input the amount you want to make change from, and returns the minimum number of coins required. We know,

$$C(1) = 1$$

because you just use the 1p coin. Then,

$$C(2) = C(1) + \text{one coin}$$

In that case, that extra coin is the 1p coin. So,

$$C(2) = 2$$

#####  [5,4] | 5 = 4 + 1

Let's apply the same logic as before. We know,

$$C(4) = 1$$

because you use the *coin*, 4. Then,

$$C(5) = C(4) + \text{one coin}$$

Again, that extra coin is the 1p coin. So,

$$C(5) = C(4) + 1 = 2$$

##### [6,3] | 6 = 3 + 3

It first seems that the method breaks down. 

$$C(5) = 2$$

from above. Then,

$$C(6) = C(5) + \text{one coin} = C(5) + 1 = 3$$

Which we know to be wrong. Looking a little closer we realise that,

$$C(3) = 1 $$

and,

$$C(6) = C(3) + \text{one coin}$$ 

This time, that extra coin is the 3p coin. It's still just _one_ extra coin though, so,

$$C(6) = C(3) + 1 = 2$$

##### [?,?] | m = n + 1

There is a common theme with all of these examples. Each one is obtained by adding one coin to a value we already know. Expressing it another way,

$$\text{new change needed} = \text{old change needed} + \text{one extra coin}$$

Or,

$$C(m) = C(n) + 1, \quad n < m$$

The new solution will be one coin more than an old solution. We have three coins to choose from, (1, 3, 4). This leaves us with three cases. 

$$\text{(1)} \quad C(m) = C(m - 1) + 1, \quad \text{for the 1p coin}$$

$$\text{(2)} \quad C(m) = C(m - 3) + 1, \quad \text{for the 3p coin}$$

$$\text{(3)} \quad C(m) = C(m - 4) + 1, \quad \text{for the 4p coin}$$

One of these three cases will leave us with the smallest number.  Now, we can find the best solution to _any_ value, as long as we remember all the best solution for all the smaller values. Let's trace out how the table is populated for the values 7, 8, 9 and 10

```
input : 7
3 cases,
7 - 1 = 6. Best change for 6 is 2 coins (4,2)
7 - 3 = 4. Best change for 4 is 1 coin  (4,1)
7 - 4 = 3. Best change for 3 is 1 coin  (3,1)
=>
Best change for 7 is 2 coins (3,1) + (4,1) -> (4,1)(3,1)
...
input : 8
3 cases,
8 - 1 = 7. Best change for 7 is 2 coins (4,1)(3,1)
8 - 3 = 5. Best change for 5 is 2 coins (4,1)(1,1)
8 - 4 = 4. Best change for 4 is 1 coin  (4,1)
=>
Best change for 8 is 2 coins (4,1) + (4,1) -> (4,2)
...
input : 9
3 cases,
9 - 1 = 8. Best change for 8 is 2 coins (4,2) // From earlier!
9 - 3 = 6. Best change for 6 is 2 coins (3,2)
9 - 4 = 5. Best change for 5 is 2 coins (4,1)(1,1)
=>
Best change for 9 is 3 coins (4,1)(1,1) + (4,1)
...
input : 10
3 cases,
10 - 1 = 9. Best change for 9 is 3 coins (4,2)(1,1) // From earlier!
10 - 3 = 7. Best change for 7 is 2 coins (4,1)(3,1)
10 - 4 = 6. Best change for 6 is 2 coins (3,2)
=>
Best change for 10 is 3 coins (3,2) + (4,1) 
...
```

{{< expandable label="Code Snippet #3" level="2" >}}
Code for the dynamic programming solution. To Do!

```python
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



## exit(0)

This concept, named "dynamic programming", is one of the more elegant weapons in a programmer's armoury. The idea is simple. You just,

1. Remember the best solutions to smaller problems and,
2. Combine those solutions to solve bigger problem.

The change-making problem is just one application of this idea. Within computer science, there are many more applications. But I think that this idea has applications to other fields. To me, computer science is the discipline of layering ideas on top of each other. Dynamic programming is a wonderful manifestation of this process, so I decided to write a blog about it. 



### post script

Something about Nick White

Something about wikipedia

