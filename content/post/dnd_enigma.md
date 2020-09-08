---
date: "2014-09-28"
tags: ["hugo", "theme", "command-line"]
title: "dungeons, dragons, and decryption"
---

### you find yourselves in a dark forest

Some time ago, a friend of mine came to me with a peculiar problem. He is an avid D&D player, and his Dungeon Master at the time had decided to put some important information online. Unfortunately for my friend, it was encrypted. His DM had provided him with a program to decrypt the information, and told them they'd uncover the key inside the game.

Naturally, my friend contacted me and asked that I help him break the code. This blog post is about how we broke it.

---



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

[Explanation of compilation/decompilation]

[Diagrams of how it worked, introduce the two rings]

### working backwards

Randomness in cryptography is not easy, owing to the **correctness** principle. Correctness says that your decryption should work every time (need better words to describe correctness).

That means this random ring can't *truly* be random. It has to be in the same state when you encrypt as when you decrypt, or it would be garbage.

introducing: pseudorandom number generators. And the seed (which we will find)


