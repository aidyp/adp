---
date: "2020-09-29"
tags: ["games", "maths", "computers"]
title: "binary representation in video games"
toc: "false"
---

During the video game bracket (see emulators: a link to the past), we talked about some of the technological advancements that video games brought. This post explores one of the most famous, or at least famous within the programming community, technical tricks. 

---

## writing numbers down

During an algebra lecture, my professor went on an _extremely_ long tangent about how a group of three cats is equivalent to a group of three dogs because they both contained three of the same thing. The point of this, I think, was to demonstrate that numbers are abstract. If you want to express '3' in the physical world, you need some physical representation of this. 

This is a relevant problem for computers. They are required to work with abstract concepts, like numbers, in the physical world. To do so, they need to represent the number in some manner.

A **bit** (**b**inary dig**it**) is a switch that has a value of `1` or `0`.  You can have lots of these bits in a row, like `101001010`. Computers use groups of bits to represent numbers. 

// explain bits -> numbers

## changing ghandi 

In the 1991 game, Civilisation 3, there was this weird phenonemon where Mahatma Ghandi, renowned advocate for peace, would suddenly fall into a nuclear themed bloodlust when it came to the industrial revolution. It turns out that this bug (now fondly adopted as a feature by fans of the game).

Turns out the reason for ghandi's virtual change of heart is a problem with representation.

Every world leader was given a score to indicate how warlike they were. The higher the score, the more violent. Ghandi's score was set to `1`.

```
war score:
ghandi = 1 (v peaceful)
```

When moving to the Industrial Era, the game subtracted `2` from each leaders war score.

```
war score:
ghandi = -1 (v v peaceful)
```

Underneath the hood, something else happened. Consider the `war score` in its true form, it's binary representation

```
war score:
ghandi = 00000001 (same as 1)
```

Now consider -1 in its binary representation

```
war score:
ghandi = 11111111 (same as -1)
```

The game designers assumed that every war score was always positive. Ends up with `255`. Womp womp. 

## digital magic

Representation can be a good thing, too. With a little magic, we can exploit the manner in which numbers are represented in binary to speed up abstract calculations in different ways. Here's an example.

Suppose you wanted to multiply 15 by 2. On a computer, you could add the two numbers together.

```
15 + 15 = 30
00001111 + 00001111 = 00011110
```

You'll notice that the result of the calculation is 



## it's just not my type 

> 3 what? 3 bananas? 3 elephants!?
>
> -- your science teacher, probably

Science teachers, across the world, are always banging on about units. If you solve a problem and don't append the unit, you get a lot of red pen back. 

For computers this is very important. They're pretty dumb, so you need to give them explicit context for data. The distinction between the letter `a` and the number `65` is meaningless to a computer. Data only has meaning with context.

Computer scientists have a habit of changing the context. 

## fast inverse square root

I went through a phase of watching 



