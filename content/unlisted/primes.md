---
date: "2020-11-15"
tags: ["maths"]
title: "the prime lesson"
toc: false
---

For a little while after I left school, I taught GCSE maths. I dreaded two topics above all others. The first was adding fractions. 

The second was the primes.  Primes are introduced as "numbers that are divisible by themselves and 1". This immediately leads to two questions,

1. Is 1 a prime number? 
2. Why do we have to learn this?

The first is easy. No, 1 is not a prime. I struggled with the second. I knew the answer was, "because they're interesting", but I didn't have the tools to explain what I meant. My maths is better now, and they let anyone write a blog.



This is what I'd teach them now.

---

### what are numbers made of?

Take a number, any number you like. Let's say, 30.

![Alt](/pictures/primes/prime_factor_tree_start.png#center)

Then divide it by some other number. Let's start simple, and divide it by 2.

![Alt](/pictures/primes/prime_factor_tree_begin.png#center)

Now try and divide the numbers that remain. Let's start on the left side of this tree we have made.  We can divide 15 by 3 to get 5.

![Alt](/pictures/primes/prime_factor_tree_middle.png#center)

And now the right side. What can we divide 2 by?

Dividing 2 by 1 does nothing -- it just gives us 2. Dividing 2 by 2 is similarly unhelpful. We just get 1. If we were to repeat this process, it would never end. 

![Alt](/pictures/primes/prime_factor_tree_ohno.png#center)

The same thing happens with 3, and with 5. They can't be divided by any number other than themselves. You can try this with other numbers and the same thing will happen. Eventually you'll end up with numbers that you can't divide into smaller numbers.



### elements

We've discovered something important about numbers. Some of them, like 2, 3, and 5, cannot be broken down into smaller parts. We call these numbers the **primes**. They are the elements that we combine together to make other numbers. We call these other numbers **composite** numbers. Every number is either a prime, or a composition of primes multiplied together. 

![Alt](/pictures/primes/composite_primes.png#center)

This observation is important in mathematics. It's called the **fundamental theorem of arithmetic**. It was proved by Euclid, a mathematician from ancient Greece, in a book called *Elements*.



### finding the primes

Now we know what these fundamental elements are, how do we find more?

One way might be to pick a random number, and try and figure out if it is a prime number. We know that a prime number is one that cannot be divided evenly by any number less than itself. 5 is a prime number because it cannot be divided by 2, nor 3, nor 4. So we could pick a random number, let's say 87, and see if it can be properly divided by any number smaller than it.

![Alt](/pictures/primes/prime_finding_division.png#center)

Dividing 87 by 3 gives a whole number, 29, so 87 cannot be prime. In fact, no multiple of 3 can be prime because that number is divisible by 3. This applies to other numbers too. No multiple of 2 can be a prime, no multiple of 5 can be a prime, etc. 

Eratosthenes, another ancient Greek, used this idea to devise a method of discovering primes. He started by making a table of numbers. Let's pick a small table to start, the table of numbers between 1 and 30.

![Alt](/pictures/primes/erastothenes_base.png#center)

We start with the first prime, 2. Then we mark out every multiple of 2. None of these numbers are prime, because they can be divided by 2. 

![Alt](/pictures/primes/erastothenes_twos.png#center)

We repeat the process for the next number, 3. 

![Alt](/pictures/primes/erastothenes_threes.png#center)

Now, 4 has already been crossed off, so we move to the next number, 5. We repeat the process again, marking off any multiples of 5 that haven't already been coloured in.

![Alt](/pictures/primes/erastothenes_fives.png#center)

All the numbers that remain uncoloured are prime. 

You may be wondering if we have to repeat this process with 7, 11, 13, etc. The answer is no, we don't, if we are only counting up to 30. 

{{< expandable label="why not?" level="3" >}}

One way to think of factors is like weights.

{pictures to draw here}



{{</ expandable>}}

### to infinity, and beyond

A pattern starts to appear. In the first two rows of our sieve, there are lots of primes. But in the third row, there are only two. All the numbers between 23 and 29 can't be prime because they are multiples of smaller prime numbers. 

What happens if we run the sieve for larger and larger numbers? Will we eventually reach a point where _every_ larger number is a multiple of a smaller one? 

Who thinks we'll eventually run out of primes?

Okay, some hands raised. But numbers go on forever. If there are an infinite number of numbers, then surely there must be an infinite number of prime numbers. 

Who thinks we'll never run out of prime numbers? 

Alright, some of you are saying yes, and some are saying no. The answer is **no**, we will never run out of prime numbers. And you're going to prove -- beyond _any_ doubt -- why.



### your first proof

Proving something beyond any doubt requires that we be precise with our language. The nature of this precision leads us to use symbols instead of words. Take the number 6. It's made up of two primes, 2 and 3.  We can say, "6 is the product of 2 and 3", or we can express the same idea with symbols.

$$ 6 = 2 * 3 $$

More precisely, "6 is the product of two primes where the first prime is 2 and the second prime is 3". A better way of expressing this is,

$$ 6 = p_1 * p_2 $$

where,

$$ p_1 = 2 $$

$$ p_2 = 3 $$

Now suppose we take a number that we don't know just yet. Let's call it **N**. We know this number, as with all numbers, must be made up of one or more primes. 

$$ N = p_1 * p_2 * \cdots $$ 

We're not sure exactly how many primes, so let's just say there are **n** of them. **n** could be small (in the case of N=6, there are n=2 primes), or it could be large.

$$ N = p_1 * p_2 * \cdots * p_n $$

We're trying to prove the claim that there are an infinite number of primes. We'll start by immediately conceding.

Let's assume that Team Run Out Of Primes are right. There are a limited number of primes. There might be a lot of them, but they do eventually end. We shall label this limited, *finite*, number of primes, **m**.

$$ Primes = \\{p_1, p_2, p_3, \cdots, p_m\\} $$

Then we'll make a new number, one that is made up of all of these primes. 



Then we're going to add one. 

Because numbers are infinite, we can always add one. No matter how stupendously large N may be, there is always a bigger number.

$$ N = p_1 * p_2 * \cdots * p_m + 1 $$

Now we ask whether this new number, N + 1, is prime. We already know how to do this, we just try to divide it by every prime number smaller than it. 

![Alt](/pictures/primes/prime_finding_division_proof.png#center)

But every prime we attempt to divide N+1 with leaves us with a fraction, not a whole number.

{{< expandable label="example" level="3">}}

You can try this yourself with a number smaller than infinity. Let's take the number 30 again,

$$ 30 = 2 * 3 * 5 $$

and add one.

$$ 31 = (2 * 3 * 5) + 1 $$

Attempting to divide 31 by 2, 3, and 5 respectively will never yield a whole number

$$ \frac{31}{2} = 17.5 $$

$$ \frac{31}{3} = 10.33... $$

$$ \frac{31}{5} = 6.2 $$

{{< /expandable>}}

If a number cannot be broken down into smaller whole number parts, then it is a prime number. Therefore, N+1 is prime.

But wait a moment. We already have all the primes. There were only **m** of them. And N+1 


---

### answer

