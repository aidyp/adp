---
date: "2014-09-28"
tags: ["hugo", "theme", "command-line"]
title: "p4ssw0rd$"
toc: false
---



There's a lot of password advice out there. Some if it is good, and some of it is really bad. This post is a quick dive into passwords; there's theory, practice, and even a bit of hacking. By the end you'll be able to give out your own, high quality, password advice.



---

## password_theory_123



### password space

Passwords exist in two dimensions, which we shall refer to as **length** and **depth**. A password is constructed from one or more elements, or **symbols** for the purpose of this post. The length of a password refers to how many symbols it is made up of. The depth of a password refers to how many possible symbols you may choose from.

Take [the most common PIN](https://www.theguardian.com/money/blog/2012/sep/28/debit-cards-currentaccounts), 1234, as an example. There are 4 symbols, and each symbol is a number between 0 and 9.

```
1234  <-- 4 symbols long
|
[0,1,2,3,4,5,6,7,8,9] <-- 10 symbols deep
```

From these two values we can determine how many PINs there could possibly be. Each digit is one of ten numbers, and there are four digits. The total number of

$$
N = 10^4 = 10000
$$
More generally,, with depth as D, and length as L
$$
N = D^L
$$

This gives us an equation to find the size of a password space. Here's an example: suppose your password was made out of the lowercase letters `[a-z]` and was 6 letters long. 


### go big or go home

This idea of password _space_ gives us the first pillar of password _strength_. If there are many possible options for a password, it follows that said password is stronger. The larger the password space, the more possibilities an attacker would have to try to get the correct one. With a large enough haystack, searching for the right needle becomes virtually impossible.

This produces an important question. In order to maximise password space, which dimension should we focus on? Is length or depth more important?

Length. By miles.

// Graphs go here.

*Caveat*

This distinction between length and depth is not always so clear cut. Take xkcd's great comic on the subject,

// Link to comic goes here

How can we evaluate the strength of this password? `correcthorsebatterystaple` is 25 symbols long, from a symbol set of 26 letters. This is pretty strong. A computer would have a hard time guessing this. Using our equation, we get

$$N = 26^{25}$$ 

which is a very, very large number. Searching this space is infeasible. An attacker would have to (at minumum!) guess every different combination of 25 letters.

But consider how the password is constructed. Four words are selected from a dictionary, let's say a book of 50,000 words. From this perspective, the password is 4 symbols long, from a symbol set of 50,000. This is still strong, but less so. This time the equation produces,

$$N = 50000^{4}$$

which is still a large number, but much less large than the above. If the attacker knows you're using this rule to construct your passwords, they can use the same rule to guess what that password might be. This is not to say the advice is bad. I just want to give it some context.

A good password is one that comes from a large password space. Too small, and an attacker will have no problem trying all possible passwords from that space to find the correct one. This inelegant method is called **brute forcing**.

 

### he chose... randomly

Suppose that when constructing my four random words from the dictionary, I chose words based on what I could see at the time, On my desk at the moment there is a,

- keyboard
- mouse
- lamp
- plant

This only gives me a few passwords. 

What's gone wrong? In theory I have the whole dictionary to choose from, but in practice I end up choosing the same few words every time. An attacker doesn't have to try every possible combination of words because she can safely predict that I'll only have used four words. This notion of 'prediction' is important in the study of passwords, and information theory more generally. It's called **entropy**. Entropy describes how uncertain the outcome of an event is. If I picked randomly from the dictionary, an attacker would have trouble predicting which words would be in the password. The resultant password would therefore have high entropy. If I just picked words based on what I could see, the attacker can be certain that the password will be a combination of `[keyboard, mouse, lamp, plant]`. This makes it trivial for her to crack, because the password has low entropy.

Predicting the nature of passwords is a common approach for attackers. They know that people use 0 for O, or 1 for i. People may choose their favourite football team or their child's name in the password. All of these things are leveraged by an attacker to reduce the number of guesses they need to crack the password. A low entropy password allows the attacker to narrow the space of possible passwords.  Entropy, therefore, is the second pillar of password strength. A password that has high entropy is much harder to crack than one with low entropy. 



### the strongest passwords ever

All the above confirms the strongest password: a long sequence of numbers and letters chosen at complete random. Here's an example,

`long-ass password here`

Great password. Ultimately, It's strong because it's _hard to guess_. It's also really, really hard to memorise. There's no use having a great password if I can't remember it when I need to log on to a website. We need a way to make the password _easy to remember_. This maxim, _hard to guess, easy to remember_, is the foundation of how we think about passwords. 

[Gosh this is hard to write, it's all over the place]

---

## p4ssw0rd_pr4ct1ce

Your secrets are only as safe as the secret keepers. It's all well and good having great passwords in theory, but how are they managed in practice?

Most of our passwords have only two secret keepers, us and the website or application to which our password permits us access. We'll talk about how you and I should manage our passwords a little later. First, we're going to explore how those websites and applications store our passwords.

### a big list

In order to divide up a website's material for different users, there must be a list of those users corresponding with which bit of the website they are allowed to access.
```
== User List ==
user   | account type
...
calvin | admin
hobbes | user
```

Clearly, only `calvin` should be allowed to access the `admin` part of the website. The website needs to make sure that someone claiming to be `calvin` really is him. To do this, there must be a secret shared only by `calvin` and the website. This secret, that permits him to pass, is a password.

```
== User List - Top Secret! ==
user   | account type | password
...
calvin |    admin     | schoolsux
hobbes |    user      | ilovetuna
```

Every time a user logs on to the website, they present their username and password. The website then checks the password against this list of passwords. If it's correct, access granted. If not, try again. To prevent an attacker from just trying every password under the sun, we can design our website to lock access after someone has failed to log in multiple times.

This, while necessary, is not sufficient protection. Anyone with access to the password list can pretend to be any user. This password list must be protected. Disaster would strike if it fell into the wrong hands. You could encrypt the password list, but you'd still need to have the encryption key somewhere. This is a little harder for an attacker, as she now must steal both the key and the list, but it still isn't a meaningful barrier. These kinds of breaches happen [all the time](https://en.wikipedia.org/wiki/List_of_data_breaches)

The question is, how can these passwords be protected, even if (when) this master list is stolen?

### make a hash of it

Consider a sausage machine. You put the pig in one end, some stuff happens, and you get sausages out the other. This process cannot be done in reverse; there is no machine to recover a pig from a sausage. **Cryptographic hash functions** provide much the same assurance. You put the input, `x`, in one end and you get `y` out the other. Like the sausage machine, this process cannot be reversed.

```
pig -->   Sausage Machine   --> sausage
x   -->    Hash Function    --> y

One way only.
```

Hash functions possess a few additional important properties. First, a hash function returns values of a fixed length. Imagine if, no matter how large the pig fed into the sausage machine, only one sausage comes out. The same sized sausage, every time.  Second, a hash function promises that no two inputs will ever result in the same output. Take two different pigs, they will result in two dramatically different sausages.[^1]

[^1]: These two properties are contradictory. Because hash functions map an infinitely large space (the set of *variable* length input strings) to a finite space (the set of *fixed* length output strings), it follows that there will be inputs that map to the same output. This is formally called a **collision**. The hash functions used in the modern web are not perfectly collision free, it's just so hard to find a collision that they may as well be.

This gives websites a safer way to store your passwords. Suppose your password was `password`, the website will now only store the hash of that, `H(password) = a3jX...Jxk`. Instead of the big list earlier, they instead store the hash of the password. 

```
== user accounts - TOP SECRET! == 
calvin | xjaj...ded  (The hash of 'schoolsux')
hobbes | 3jfj...ttb  (The hash of 'ilovetuna')
```

When a user wants access to the website, they present their password. The website hashes the password, and checks if the resultant hash matches the password list. If it does, the password is correct. If it doesn't, try again.

If all this is done correctly (along with a few other tricks, but that's out of scope for this post) then even if the password list is stolen, the thief can't read the passwords. Good hashes are irreversible so the only way they can crack the password is by trying every possible combination and seeing if the hash matches. This is no better than a brute force attack. If your password is good, it will survive this attack.

### programmers are human too

The above is an over simplification of the best practice to store passwords. Making sure that passwords remain safe while still offering a smooth service to thousands of users is difficult. Companies and organisations get it wrong, a lot. Big ones, too. In {date}, it was leaked that Apache were incorrectly storing their passwords. The details are technical, I've hidden them away in the below tab, but the point remains; you cannot always rely on websites to keep your passwords safe. 

Security doesn't really make money. Given a choice between a feature that will attract more users, or improving password security, organisations will lean to the former. 

But let's give websites and their designers the benefit of the doubt (we shouldn't) and assume they always use the latest and greatest in password protection (they don't). How safe are your passwords under this assumption?

### cracking passwords

Password cracking is a popular field in computing because (1) it can make you a lot of money and (2) it's interesting. 

This section (may be moved) explores how password hashes are cracked. Will use some examples from (https://www.tunnelsup.com/getting-started-cracking-password-hashes/), john the ripper, hashcat etc. Just give the reader an idea of what the benchmarks are.

The idea is to show that it's not that hard to crack passwords, anyone can do it, and perhaps a little exploration as to _how_ passwords are cracked. The theme is to talk about how our password rules can be manipulated

### memory 

It's clear that, in order to protect passwords, they mustn't be re-used. The solution then, is to have a unique password for every account. How many accounts is that?

The average person on the internet has

- 8.5 social media accounts
- accounts for utilities, the council, etc
- e-mail
- subscriptions: netflix, spotify, wired, etc
- online shopping

We all have so many passwords. In the past few years there has been an effort to merge lots of different identities into one. You can now sign into some websites via facebook, effectively one password for two websites, but only facebook has to keep your password safe. However, even with this trend, we all still have too many passwords to remember.

The average person remembers something like [However many passwords they can remember, I bet it's not much]. Clearly, there's no way to remember the number of distinct passwords we need. Even if those passwords are 'easy to remember' Most people reach for one of three resolutions

- Come up with a 'password rule' that doesn't mean you have to remember different passwords
- Write all the unique passwords down somewhere
- ~~Use the same password more than once~~

Let's review each in turn. 

#### password rule

It's tempting to come up with a set of rules that means you never have to remember a password. This might be something simple like `name of website + 123`, which will produce a set of unique passwords. It might be more complex like.

Seems perfect. All you have to remember is the rule, and then every time you visit the website you can reconstruct your password on the fly. As long as the rule is sufficiently complex, there's no way an attacker could guess your rule.

Maybe. There's no guarantee that your idea is unique. Your carefully crafted rule might be something an attacker has (1) already thought of or (2) encountered it before in another victim. As we saw earlier, once attackers have discovered a rule, they can easily leverage it in an attack.

#### write them down 

Writing passwords down means that you never have to remember them. You can make them as strong as you like, without having to worry about remembering them. Problems are if it gets stolen (or more likely, lost), your secrets are vulnerable and you can't access your websites. 

Could write them down on a computer, but if your computer breaks or you lose it, then you've lost all your passwords. Also you have to keep them safe. That's what password managers do. They're a list of passwords protected by a single password that you remember.  As long as the main password stays safe, all your other passwords are safe. Only problem remaining is you have to make sure that it's accessible from different devices -- which means it has to be hosted somewhere on the internet (LastPass -- can be compromised)

[Really starting to add up on the content here]

This subsection talks about how many passwords we can reasonably be expected to remember with _different_ rules. We may have many different passwords, but it's likely we come up with one or two rules. From the theory perspective, it's clear how the strength of our rules can be used to predict passwords.




## Summary

The blog post isn't there just to make people feel bad. The idea is to give some suggestions about what to do.  There are password managers. Although there should be some understanding that this takes a lot of getting used to.

The overall aim of the blog post isn't to shame people into good / bad password policies, it's just to share some intuitions that may be helpful when talking about passwords.