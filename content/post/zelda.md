---
date: "2020-08-28"
tags: ["exploration", "music", "maps"]
title: "emulation: a link to the past"
toc: false
images:
  - /pictures/king_of_the_nerds.jpg
---

A friend of mine decided to hold an elimination tournament for the best video game of all time. We squared up great games in a winner takes all knockout bracket.

It was mostly nostalgia that got us going. Midway through the tournament I decided to make a console that played all those old games. An altar to nostalgia. This blog post is about that.

Oh, and Pokemon Blue won the tournament.

---

## level one



// Idea -- use screenshots from games to structure the post! Makes it good to read through I think

SMB3 screenshot?

A computer, in its basic form, is made up of two parts: hardware and software. The hardware is the collection of electronic components that can perform different operations. Software is the list of instructions to co-ordinate all those operations into something useful.

// Diagram of software on top of hardware 

The above diagram isn't quite accurate. Not every computer is the same. Computers are designed with different hardware to solve different problems. Newer computers have different, and better, hardware than older computers. 

// Diagram of "jigsaw" software + hardware

This poses a problem. What if you want to take some software from `computer 1` and run it on `computer 2`? It won't work. The software has been designed for a specific hardware configuration.

{{< expandable label="operating systems" level="3" >}}

This is part of the motivation behind operating systems. Instead of having software writers write software for each computer on the market, there is a layer between the hardware and the software.

// Diagram of 'operating system' software + hardware

This blog isn't about operating systems, but I love side quests.

{{< /expandable >}}

But this post is about video games.

Consoles from the past, like the Atari, Super Nintendo, Playstation and Dreamcast are all different computers with different hardware. The games -- which are just software -- won't run on other devices. It's hard to get your hands on these old consoles, so how to play all these old games?



### enter emulation

This is a problem many programmers were keen to solve, on account of them loving video games and hating real  work.  The idea is elegant. The older games are written for a specific hardware configuration. They'll make use of certain hardware commands and operations. If that configuration is known, you can write software that simulates it. 

// Diagram of 'emulator stack'

It's not easy to make an emulator. Luckily, someone else has already done it. In order to make this console, we need two parts. 

- Some hardware
- Emulation software



## equipping some hardware

Emulators notwithstanding, we need some hardware to run these games. You could use your laptop, but that's not much fun. It'd be cooler to have a dedicated device. But we don't want to spend too much. If we were going to spend a lot of money on a console, better to just buy an Xbox. We need a machine that's cheap, but powerful enough to simulate older consoles. We need a [Raspberry Pi](https://www.raspberrypi.org/)

// Include picture of a raspberry pi here

A raspberry pi 4 will set you back about £100 (cables and whatnot included). How does it compare to consoles in the past? (https://www.ign.com/articles/2013/10/15/the-real-cost-of-gaming-inflation-time-and-purchasing-power)

Hardware evolves over time. The general rule of thumb is that power doubles every 18 months.

|                     | **Nintendo Entertainment System** | Raspberry Pi |
| :-----------------: | :-------------------------------: | :----------: |
| **Processor Speed** |              1.79MHz              |    1.5GHz    |
|     **Memory**      |                2KB                |     2GB      |
|   **Resolution**    |             256 x 240             |    4kp60     |

It's often hard to appreciate how fast computing hardware moves relative to other technologies. The NES had (however many kb of memory). The Raspberry Pi has (however many _mega_ bytes). Even over 20 years. That's mad!

This comparison speaks to something else about video games. A great video game isn't dependent on hardware. Some of the games we remember fondly don't require a big graphic load. Technology is an enabler of games. It's not the defining feature. Game developers sometimes try to cash in on this nostalgia by re-mastering old games with better graphics. Sometimes it works, and sometimes it doesn't. 

### mini NES

It's good to put a Pi in a case. Makes it easier to move around and keeps it safe. I decided to buy a case that looked like a NES, because they were the easiest to find on amazon.

// picture of the open case now

It came with a little fan, designed to keep the computer cool while it runs. This fan was _very_ loud. I thought about replacing it, but ultimately decided against it. There was something about the noisy whirring of the fan that made the device feel more real. 



{{< ticks >}}

* Some hardware

{{< /ticks >}}

## retropie

Emulation is a popular field. Many programmers played _a lot_ of games as a kid. There's an ongoing project to assemble as many emulators as possible into one engine. It's called [RetroPie](https://retropie.org.uk/). 





{{< ticks >}}

* Some hardware
* Emulation software

{{< /ticks >}}



Now to play some games.

## game time

I haven't covered getting the games yet. Games for old consoles came in these physical cartridges. On board the cartridges was some computer hardware that stored all the information for the game. 

The older cartridges won't work with the Pi console. You could find adaptors between the older interface and something more modern, like USB. But this would become infeasible pretty fast; you'd need a separate adaptor for every console. 

Luckily, the principle of software-pretending-to-be-hardware can come to the rescue. Someone can copy the cartridge into software, and then share that file. These files are usually referred to as **ROM**s, named after the **R**ead **O**nly **M**emory that is present on game cartridges. 



## end credits

![Alt](/pictures/king_of_the_nerds.jpg#center)



Now the real work can begin.

// Screenshot of 'starting your very own pokemon adventure' screenshot

 