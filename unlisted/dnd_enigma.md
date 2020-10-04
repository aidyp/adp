---
date: "2014-09-28"
tags: ["hugo", "theme", "command-line"]
title: "dungeons, dragons, and decryption"
---



Some time ago, a friend of mine came to me with a peculiar problem. He is an avid D&D player, and his Dungeon Master had decided to put some important information online. Unfortunately for my friend, it was encrypted.

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

M (as we shall refer to my friend) brought me up to speed with the situation. He sent me a copy of the decryption program his DM had written, `enigma.exe`, and we got to work.




### breaking in

We weren't getting anywhere playing fair, so we decided to crack open `enigma.exe` to see what was in the insides.

The `.exe` extension on a file stands for an **executable** file. An executable is a sequence of low level computer instructions. They're hard to read, they mostly describe the movement of individual blocks of memory from one area of the processor to the other. They're also very difficult to write. Nowadays, programmers write code in a programming language that's much easier to read and write. The programmer writes their code in this higher level language. When a programmer wants to run their code, they first pass it through a translator that turns it into low level computer instructions. This process is called **compilation**. And it can be undone, with a **decompiler**.

We ran the program through a decompiler and recovered the original code used to create `enigma.exe`. After some time looking out the code, we figured out how the scheme worked.

The scheme made use of encoding rings. Imagine
// Caesar Cipher ring picture here

By changing the ordering of the letters on the outer ring, you can come up with lots of different encoding rings. For compactness, I want to represent this ring another way. The `(in)` letter is the letter you start with. In this example, it's `a`, which is the first letter in the alphabet. You pick the corresponding first letter in the ring, which is `d`. That would be the encrypted result.
```
 a (in)
 |
[d e f g h i j k l m n o p q r s t u v w x y z a b c]
 |
 d (out)
```

`enigma.exe` makes use of two encoding rings chained together. The `(out)` of the first ring becames the `(in)` of the second. We named this stage as the `(intermediate)` step. Starting with `a` again, the intermediate result is `d`. When it goes through the second encoding ring

```
 a (in)
 |
[d e f g h i j k l m n o p q r s t u v w x y z a b c]
 |
 |---- d (intermediate)
       |
[d e f g h i j k l m n o p q r s t u v w x y z a b c]
       |
       g (out)
```

We were dealing with two rings, 
```
(in)

[ ? ? ? ? ? ? ? ? ] (ring 1)

(intermediate)

[ ? ? ? ? ? ? ? ? ] (ring 2)

(out)
```
All we had to do is figure out the nature of the rings. How were they forged?

The first ring, which we named the 'key ring', was formed according to the following steps
```
1. Split up the letters of the key, e.g "key -> k e y"
2. Put the letters of the key into the key ring
3. Fill up the remainder of the key ring with the rest of the alphabet

example: key = "keyword"
letters: (k e y w o r d)
keyring: [k e y w o r d a b c f g h i j l m n p q s t u v x y z]
```

The second ring, which we called the 'scrambler ring', is changed after each character has been encoded. This is where the randomness we observed came from. Suppose the same character was encoded twice -- you wanted to encrypt `aa`. The scrambler ring would be in a different position for each `a`. 

### working backwards

Randomness in cryptography is not easy, owing to the **correctness** principle. Correctness is a roundabout way of saying "if you encrypt something, you should be able to decrypt it". 

That means this random ring can't *truly* be random. At least not random in the way we commonly think about it. It has to be in the same state when you encrypt as when you decrypt, or it would be garbage. 



### advanced counting

Having undone the effects of the scrambler ring, we could now get to work undoing the effects of the key ring. Recall the key ring looks like this
```
(in)
[k e y a b c d f ...]
(out)
```

Now that the random element is done away with, there are some tricks we can use to crack the code. The first is **frequency analysis**. 

Language is, by necessity, well structured. Certain letters come up more often than other letters. For English, if you were to plot the frequency of different letters on a graph, you'd get something like this.

![Alt](/pictures/eng_freq.png#center)



### guesswork

`enigma.exe` preserved other parts of the structure of the message. Namely, the length of letters were preserved. 

### in of by as ok



### you reflect on your journey

One observation that may have occurred to you as `enigma.exe` was unravelled. Namely,

_What if we used part of key, which is secret, as the seed to the random number generator?_

This makes sense. That way, there's no way to find out the state of scrambler ring without the key, so the approach we took wouldn't have worked at all. You'd be correct. You'd also have stumbled upon a core principle in cryptography, and mastered the intuition behind a whole class of encryption algorithms that are used today. 

The principle is **Kerckhoff's principle**. This principle was best stated by Claude Shannon, the father of information theory. 

> one ought to design systems under the assumption that the enemy will immediately gain full familiarity with them

The only thing that can ever be really kept secret is the key. You can't keep the encryption machine hidden away and hope that nobody will find it. Like with the historical enigma, the inner workings of `enigma.exe` became known to the attackers.

The algorithm class is a **stream cipher**. The idea is that you take a key, and feed it into a psuedorandom number generator.

### post session