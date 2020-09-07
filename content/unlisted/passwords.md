---
date: "2014-09-28"
tags: ["hugo", "theme", "command-line"]
title: "p4ssw0rd$"
---

## password_theory_123

### password space

Passwords exist in two dimensions, which we shall refer to as **length** and **depth**. A password is constructed from one or more elements, or **symbols** for the purpose of this blog. The length of a password refers to how many symbols it is made up of. The depth of a password refers to how many possible symbols you may choose from.

Take [the most common PIN](https://www.theguardian.com/money/blog/2012/sep/28/debit-cards-currentaccounts), 1234, as an example. There are 4 symbols, and each symbol is a number between 0 and 9.

```
1234  <-- 4 symbols long
|
[0,1,2,3,4,5,6,7,8,9] <-- 10 symbols deep
```

From these two dimensions we can determine how large the space of possible passwords is. 

### password strength

This idea of password _space_ gives us a way to think about password _strength_. If there are many possible options for a password, it follows that said password is stronger. 

This produces an important question. In order to maximise password strength, which dimension should we focus on? Is length or depth more important?

Length. 

// Graphs go here.

*Caveat*

This distinction between length and depth is not always so clear cut. Take xkcd's excellent comic on the subject,

How best to evaluate the strength of this password? `correcthorsebatterystaple` is 25 symbols long, from a symbol set of 26 letters. This is pretty strong. A computer would have a hard time guessing this. Using our equation, we get

$$P = 26^{25}$$ 

which is a very, very large number

But consider how the password is constructed. Four words are selected from a dictionary, let's say a book of 50,000 words. From this perspective, the password is 4 symbols long, from a symbol set of 50,000. This is still strong, but less so. This time the equation produces,

$$P = 50000^{4}$$

which is still a large number, but much less large than the above.

 

### he chose... randomly

The xkcd example illustrates a devastating weakness in password strength. The manner in which a password is constructed is more important 

## password.practice!

Your secrets are only as safe as the secret keepers. Before we can evaluate how best to manage passwords, we have to examine how your passwords are kept secret. 

Most of our passwords have only two secret keepers, us and the website to which our password permits us access. We'll talk about how you and I should manage our passwords a little later. First, we're going to explore how websites store passwords.

### make a hash of it

(make a mention of hash vs cryptographic hash) A cryptographic hash function is like a digital sausage machine. You put the pig in one end, and you get sausages out the other. This process cannot be done in reverse. There is no machine to recover a pig from a sausage. Hash functions provide the same assurance.

```
pig --> Magic Sausage Machine --> sausage
x   -->     Hash Function     --> y

One way only.
```

They have a few other important properties. First, a hash function returns values of a fixed length. No matter how large the pig going into the magic sausage machine, only one sausage comes out. The same sized sausage, every time. 

Second, a hash function promises that no two inputs will ever result in the same output. Take two different pigs, they will result in two different sausages.

### programmers are human too

The above is quite a dramatic simplification of the best practice to store passwords. Companies and organisations get it wrong, a lot. Big ones, too. In {date}, it was leaked that Apache were incorrectly storing their passwords. The details are a little technical, so I've hidden them away in the below tab

### memory 

This subsection talks about how many passwords we can reasonably be expected to remember with _different_ rules. We may have many different passwords, but it's likely we come up with one or two rules. From the theory perspective, it's clear how the strength of our rules can be used to predict passwords.

### cracking passwords

When defending, it pays to understand the attacker.

This section (may be moved) explores how password hashes are cracked. Will use some examples from (https://www.tunnelsup.com/getting-started-cracking-password-hashes/)

The idea is to show that it's not that hard to crack passwords, anyone can do it, and perhaps a little exploration as to _how_ passwords are cracked. The theme is to talk about how our password rules can be manipulated


## Summary

The blog post isn't there just to make people feel bad. The idea is to give some suggestions about what to do.  There are password managers. Although there should be some understanding that this takes a lot of getting used to.

There is the idea of tiered passwords, having a few very secret ones for your really important services that you don't re-use. 

The overall aim of the blog post isn't to shame people into good / bad password policies, it's just to share some intuitions that I find helpful when talking about passwords