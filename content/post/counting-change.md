---
date: "2014-09-28"
tags: ["hugo", "theme", "command-line"]
title: "Counting Change"
---



## Hello World



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







## Don't be greedy

The "counting change" problem came up again during my first year of university. I was in an afternoon lecture on the theory of computation. My lecturer used my solution to the problem as a throwaway example of a "greedy algorithm". I thought the title rather appropriate.



Then he introduced a spanner into the works. Suppose we loosen the constraints on change and allow non-standard coins. Instead of 1p, 5p, 10p, coins, we might have 1p, 9p, 10p. How does our greedy algorithm hold up?

Not well.

```
input  : 27
output : 10, 10, 1, 1, 1, 1, 1, 1, 1
```

Our greedy program fails. For `27`, the minimal change is 3 coins, as $27 = 9 + 9 + 9$.

## Changing Perspectives

This problem came up a third time in my third year. It was time to think about jobs. For computer science undergraduates this means sweating blood solving problems like this so that, at an interview with a top tech firm, you can solve a problem like this.

That's the trick. When you encounter a new problem, it's much easier if you know the solution to a related problem.





