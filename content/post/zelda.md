---
date: "2020-10-07"
tags: ["exploration", "music", "maps"]
title: "emulation: a link to the past"
toc: false
images:
  - /pictures/king_of_the_nerds.jpg
---

![Alt](/pictures/king_of_the_nerds.jpg#center)

A friend of mine hosted an elimination tournament for the best video game of all time. We squared up great games in a winner takes all knockout bracket. It was mostly nostalgia that got us going. Midway through the tournament I decided to make a console that played all those old games. An altar to nostalgia.

Oh, and Pokemon Blue won the tournament.

---

## level one



// Idea -- use screenshots from games to structure the post! Makes it good to read through I think

A computer system, in its basic form, is made up of two parts: hardware and software. The hardware is the collection of electronic components that can perform different operations. Software is the list of instructions given to the hardware to co-ordinate all those operations into something useful. 

![Alt](/pictures/software_hardware.png#center)

The above diagram isn't totally accurate. Not every computer is the same. Computers are designed with different hardware to solve different problems. Newer computers have different, and better, hardware than older computers. And because the hardware is different, the software must also be different. 

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

This is a problem many programmers were keen to solve, on account of them loving video games and hating real work.  Their solution is elegant.

We know older games -- pieces of software -- are written for a specific hardware configuration. The software will issue instructions to the hardware, and expect to see a result.

![Alt](/pictures/emulator-expectations.png#center)

From the software's perspective it issues orders and gets back results. The reason it won't work on a different computer is that the hardware of the different machine doesn't understand these orders. To solve this, we can build a translator. 

![Alt](/pictures/emulator_between.png#center)

The translator takes the hardware instruction, and maps it to the hardware instruction that the different machine can understand. It then passes the result of the operation back to the software in the expected format.

{{< expandable label="all at once or one at a time?" level="3" >}}

Readers of [counting change](/post/counting-change) may observe that this approach may benefit from some time-vs-space analysis. The game software is a list of pre-defined instructions. Instead of translating the instructions one by one, as they come, we could instead pre-translate them. This trade-off between converting the instructions in one go versus converting them on the fly is known as *static versus dynamic recompilation*.

Unfortunately, static compilation is very difficult. The software instructions often rely on results they can't know at the time. If you were playing Zelda, the game must first wait for you to decide which way you want to go before it can present the next part of the adventure. You could try and load all the possible results for all the possible decisions Link might make, but this rapidly becomes unfeasible. Additionally, sometimes software will generate _new_ instructions in response to certain results! 

{{< /expandable >}}

The software that manages this translation, alongside the myriad of other things needed to execute this idea, is called an emulator. Making them is extremely tricky. For one thing, what I've drawn as one singular box named 'Hardware' is instead a tightly knit system of physical components. An emulator must manage all of these components, and the way they interact with each other. For another, this process of translation is very slow. Emulator engineers need to find some clever workarounds to speed it up to a rate that makes it usable. Were we to dive in to the design and development of these emulators, this blog post would be far longer and I fear I'd be unable to competently guide you.

Luckily, someone else has already built these emulators. There is a vast network of talented engineers who have worked to emulate dozens of different games consoles. We're going to use their work to make this console. In order to do so, we're going to need two main parts.

- Some hardware
- Some emulation software



## equipping some hardware

### electronics

We need some metal to run these games. Something solid. Some proper gear. You could use your laptop, but that's not much fun. It'd be cooler to have a dedicated device. But we don't want to spend too much. If we were going to spend a lot of money on a console, better to just buy an Xbox or a Playstation. We want a machine that's cheap, but powerful enough to simulate older consoles. 

We want a [Raspberry Pi](https://www.raspberrypi.org/)

![Alt](https://upload.wikimedia.org/wikipedia/commons/thumb/f/f1/Raspberry_Pi_4_Model_B_-_Side.jpg/2880px-Raspberry_Pi_4_Model_B_-_Side.jpg#center "The latest Raspberry Pi model")

This is a Raspberry Pi 4B. A raspberry pi 4 will set you back about £100 (cables and whatnot included). How does it compare to consoles in the past? (https://www.ign.com/articles/2013/10/15/the-real-cost-of-gaming-inflation-time-and-purchasing-power)

Hardware evolves over time. The general rule of thumb is that power doubles every 18 months.

|                     | **Nintendo Entertainment System** | Raspberry Pi |
| :-----------------: | :-------------------------------: | :----------: |
| **Processor Speed** |              1.79MHz              |    1.5GHz    |
|     **Memory**      |                2KB                |     2GB      |
|   **Resolution**    |             256 x 240             |    4kp60     |

It's often hard to appreciate how fast computing power improves relative to other technologies. The NES had 2 KB of memory, which is `2 * 1024` bytes. The Raspberry Pi has 2 **GB** of memory, which is `2 * 1024 * 1024 * 1024` bytes. Not to beat a dead horse, but the little Pi has _one million_ times more memory than the Nintendo Entertainment System.

This comparison speaks to something else about video games. A great video game isn't dependent on hardware. Some of the games we remember fondly don't require a big graphic load. Technology is an enabler of games. It's not the defining feature. Game developers sometimes try to cash in on this nostalgia by re-mastering old games with better graphics. Sometimes it works, and sometimes it doesn't. 

### case

It's good to put a Pi in a case. Makes it easier to move around and keeps it safe. I decided to buy a case that looked like a NES, because of its iconic design and they were the easiest to find on Amazon.

// picture of the open case now

It came with a little fan, designed to keep the computer cool while it runs. This fan was _very_ loud. I thought about replacing it, but ultimately decided against it. There was something about the noisy whirring of the fan that made the device feel more real. The case also came bundled with some controllers!

### retropie

We've got the box, but now we need the brains. 

Emulation is a popular field. Many programmers played _a lot_ of games as a kid. There's an ongoing project to assemble as many emulators as possible into one software package. It's called [RetroPie](https://retropie.org.uk/). Installation is very easy, the instructions on their website are clear and thorough.

RetroPie acts as a sort of emulator librarian. Emulators are made from software. 

// I need to figure out a way to explain this in line with the explanation already given. Retropie loads a particular emulator with a video game attached.



{{< ticks >}}

* Some hardware
* Some emulation software

{{< /ticks >}}

The console completed, we need only some games.

## game time

Games for old consoles came in these physical cartridges. On board the cartridges was some computer hardware that stored all the software instructions that make up the game. 

![Alt](https://upload.wikimedia.org/wikipedia/commons/thumb/4/40/NES-Cartridge.jpg/542px-NES-Cartridge.jpg#center "Bit bigger than a USB stick!")

The older cartridges won't work with our console. You could find adaptors between the older interface and something more modern, like **USB** (**U**niversal **S**erial **B**us). But this would become infeasible pretty fast; you'd need a separate adaptor for every console. That's if you could find them at all!

Luckily, the principle of software-pretending-to-be-hardware can come to the rescue once again. These cartridges store a list of instructions Someone can copy the cartridge into software, and then share that file. These files are usually referred to as **ROM**s, named after the **R**ead **O**nly **M**emory that is present on game cartridges. 

Now, these ROMs are the intellectual property of the companies that created them. Distribution of them is a breach of copyright. Unfortunately, in many cases it's also impossible to purchase them from the original vendor. I'm not going to point you in the direction of a place where you can obtain ROMs, but I will point out that others have made their own solution to this "distribution problem".



## opening credits



That's it. We're done! Now the real work can begin. 

After all, my very own

![Alt](/pictures/pokemon_blue_beginning.png#center)

 