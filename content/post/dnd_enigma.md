---
date: "2014-09-28"
tags: ["hugo", "theme", "command-line"]
title: "dungeons, dragons, and decryption"
---



Some time ago, a friend of mine came to me with a peculiar problem. He wanted me to break a code. He is an avid D&D player, and his Dungeon Master at the time had decided to put some important information online. Unfortunately for my friend, it was encrypted.

This blog post is about how we broke the code.

---
### you find yourselves in a dark forest


**timeline**

- I was sent a copy of the decryption program, `enigma.exe`. 
- decompiled the program to get a look at the source code
- analysed it to figure out what was going on (write some pseudo code to explain what happens)
- draw out the diagram
- found the seed for the RNG
- computed all the possible ring states 
- ran the ciphertext back through the possible ring states
- still encrypted, but easier, now just a permutation
- cracked the permutation with some strategies (frequency analysis, guesswork)
- brief mention of offsets

### chapter one

M brought me up to speed with the situation. He sent me a copy of the decryption program, `enigma.exe`, and we got to work.




### breaking in

We weren't getting anywhere playing fair, so we decided to crack open `enigma.exe` to see what was in the insides.

The `.exe` extension on a file stands for an **executable** file. An executable is a sequence of low level computer instructions. They're hard to read, they mostly describe the movement of individual blocks of memory from one area of the processor to the other. They're also very difficult to write. Nowadays, programmers write code in a programming language that's much easier to read and write. The programmer writes their code in this higher level language. When a programmer wants to run their code, they first pass it through a translator that turns it into low level computer instructions. This process is called **compilation**. And it can be undone, with a **decompiler**.

[Diagrams of how it worked, introduce the two rings]

### working backwards

Randomness in cryptography is not easy, owing to the **correctness** principle. Correctness says that your decryption should work every time (need better words to describe correctness).

That means this random ring can't *truly* be random. At least not random in the way we commonly think about it. It has to be in the same state when you encrypt as when you decrypt, or it would be garbage.



### advanced counting

Having undone the effects of the scrambler ring, we could now get to work undoing the effects of the key ring. Remember the key ring looks like this,
```
 a b c d e f g h ...
[k e y a b c d f ...]
a -> [k] -> k
```

Now that the random element is done away with, there are some tricks we can use to crack the code. 

### in of by as ok





### post session

