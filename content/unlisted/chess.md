---
date: "2021-03-11"
tags: ["programming", "games"]
title: "the robot's gambit"
subtitle: "with ewan morgan"
toc: false
---

Written with [Ewan Morgan](https://ewanmorgan.journoportfolio.com/)

---

{{< blockquote author="Garry Kasparov" >}}

It's interesting that the greatest minds of computer science, the founding fathers, like Alan Turing and Claude Shannon and Norbert Wiener, they all looked at chess as the ultimate test.

{{< /blockquote >}}

A friend and I, like others during lockdown, have developed a chess obsession. So when we found ourselves with an abundance of free time (I was on holiday and he is a writer), we decided to make a computer program to play chess.

He recently [wrote an article](https://l.messenger.com/l.php?u=https%3A%2F%2Fslate.com%2Ftechnology%2F2021%2F02%2Fdeep-blue-garry-kasparov-25th-anniversary-computer-chess.html&h=AT1wyjL7NvQdVdDrfihLxfmAMRhKnMusFDENoxJVjJLwqxoknlhmIR4VIK4uCZcPN6j9Hq7N-nB7c8NMJzHF0gBtKcbDApsfHLII1in--uhkTQC0K643_aetMqafeMfHbqcSs-JDwFU) about the history of chess engines. So, when we found ourselves with an abundance of free time (I was on holiday and he is a writer), we decided to make our very own engine.

### just make a move

![Alt](/pictures/chess/make_a_move.png#center)

For simplicity, we decided to fix the roles of the players. In our program, the human always plays white and the machine always plays black. Our first task was to get the robot to make a move.





* Maybe a screenshot of the thingy
* Some chat about making a move
* Started with moving randomly



### the importance of a plan 

In the Soviet Chess Primer, readers are encouraged to have a chess plan. At the moment, our robot doesn't really have a plan. It just moves randomly and hopes for the best. 

* We gave it a plan to take a piece if it saw a piece
* And deliver checkmate if it can see checkmate


