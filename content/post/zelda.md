---
date: "2020-11-07"
tags: ["exploration", "music", "maps"]
title: "emulation: a link to the past"
toc: false
images:
  - /pictures/king_of_the_nerds.jpg
---

![Alt](/pictures/king_of_the_nerds.jpg#center)

A friend of mine recently hosted an elimination tournament for the best video game of all time. We squared up the best games in a brutal winner-takes-all knockout bracket. As we advanced through the rounds, I noticed that the games performing best were the ones we'd played as kids. That's not surprising, nostalgia has a powerful draw. While my co-competitors were passionately making the case for older games, I found myself wanting to play them again. So, I decided to make a console that played those childhood games. That's what this post is about. 

Oh, and Pokemon Blue won the tournament.

---

## the tutorial

![Alt](/pictures/zelda_link_past_beginning.png#center)

### an ancient evil arises

It turns out that playing games from years past is not so trivial a process. To understand why, we're going to need to dip into some computational theory. A computer system, in its basic form, is made up of two parts: hardware and software. The hardware is the collection of electronic components that can perform different operations. Software is the list of instructions given to the hardware to co-ordinate all those operations into something useful. 

![Alt](/pictures/software_hardware.png#center)

A powerful idea to help visualise this is the notion of _state_. We say that, at any given moment in time, the hardware is in a particular state. That means that it has some particular data stored, it's showing a particular image on the screen, etc. By issuing instructions to the hardware, you order it to change its state. It will show something different on the screen, or change some of the data it has. By instructing hardware to move from state to state, we can design some incredible things.

Video game systems are computer systems too. The console itself, for example a Playstation, makes up the hardware of the system. The games are the software. A game is just a set of instructions to orchestrate all the various hardware parts of the console. The result of that orchestration is the game that plays out on the screen.

![Alt](/pictures/system_console_map.png#center)

The above diagrams aren't totally accurate. Not every computer is the same. Computers are designed with different hardware to solve different problems. Consoles especially so. Newer systems have different, and better, hardware than older ones. And because the hardware is different, the software to instruct it must also be different. The game software must be written _for_ the hardware, and will have a different structure as a result.

![Alt](/pictures/computer_jigsaw_puzzle.png#center)

This poses a problem. What if you want to take some software from `console 1` and run it on a different `console 2`? It won't work, because the two won't mesh. This was fine at the time. If you wanted to play some Nintendo games, buy a Nintendo console. Sega games, Sega console. So on and so forth. But "the time" was 30+ years ago. You can't get easily get those old consoles now. Even if you did manage to get one, it would be a real pain to get them to work with modern TVs. Technology has moved on. 

We're left with a boulder we can't move. Without some way to run software designed for one console on another, it is next to impossible to play an old Zelda game.

### enter emulation

This is a problem many programmers were keen to solve, on account of them loving video games and hating real work.  Their solution is elegant.

We know older games -- pieces of software -- are written for a specific hardware configuration. The software will issue instructions to the hardware, the hardware will do something based on the content of that instruction. Each "something" the hardware does, and by that I mean an operation, will change the hardware's state. It is the composition of those states, in the right order, at the right time, that makes up the game.

![Alt](/pictures/system_console_instruction.png#center)

From the software's perspective it just issues orders. It's the hardware's job to execute them. If it doesn't, too bad. The software is not responsible for that. The reason it won't work on a different computer is that the hardware of the different machine doesn't understand these orders. To solve this, we can build a translator. 

![Alt](/pictures/system_console_instruction_translated.png#center)

Now we can mirror the state that the older console would have been in after any given instruction. Because the game is simply a specific and timely ordering of different hardware states, it follows that we have also mirrored the game. That's the magic of emulation.

#### opcodes

Hardware is a broad term that covers a lot of things. The hardware in a video games console is a whole suite of components. There's graphics, memory, input/output, and more. We'll focus in on one component that's present in every set of hardware, the **CPU** (**C**ore **P**rocessing **U**nit). The CPU takes in instructions, and then executes commands based on those instructions. These orders to the CPU are called **opcodes** (**op**eration **codes**). We need to make sure that the opcode that the game issues is executed by our CPU. 

The translator takes the opcode, and maps it to the opcode that our machine can understand, _and will have the same effect on the hardware state_. It then executes the translated instruction.

{{< expandable label="all at once or one at a time?" level="3" >}}

Readers of [counting change](/post/counting-change) may notice that this approach could benefit from some 'time-vs-space' thinking. The game software is a list of pre-defined instructions. Instead of translating the instructions one by one, as they come, we could instead pre-translate them. This trade-off between converting the instructions in one go versus converting them on the fly is known as *static versus dynamic recompilation*.

Unfortunately, static compilation is very difficult. The software instructions often rely on results they can't know at the time. If you were playing Zelda, the game must first wait for you to decide which way you want to go before it can present the next part of the adventure. You could try and load all the possible results for all the possible decisions Link might make, but this rapidly becomes unfeasible. Additionally, sometimes software will generate _new_ instructions in response to certain results! These can be difficult to predict, and guessing them is nearly impossible

{{< /expandable >}}

The software that manages this translation, alongside the myriad of other things needed to execute this idea, is called an emulator. Making them is extremely tricky. For one thing, what I've drawn as one singular box named 'Hardware' is instead a tightly knit system of physical components. An emulator must manage translation for all of these components, and the way they interact with each other. Crucially, games don't just take their instructions from software. They must also take input from the player and process it. CPU's manage this through **interrupts**. Trying to handle those properly for an emulated game is a fiddly business. 

Additionally, this process of translation, as described, is very slow. Emulator engineers need to find some clever workarounds to speed it up to a rate that makes it usable. Were we to dive in to the design and development of these emulators, this blog post would be far longer and I fear I'd be unable to competently guide you.[^1]

[^1]: For those of you whose interest I have piqued, you should refer to [this work](http://www.xsim.com/papers/Bario.2001.emubook.pdf). It lays out the foundations of emulation thinking.

Luckily, someone else has already built one. There is a vast network of talented engineers who have worked to emulate dozens of different games consoles. We're going to use their work to make this console. We'll need two parts,

- Some hardware
- Some emulation software



## equipping some hardware

### electronics

We need some metal to run these games. Something solid. Some proper gear. You could use your laptop, but that's not much fun. It'd be cooler to have a dedicated device. But we don't want to spend too much. If we were going to spend a lot of money on a console, better to just buy an Xbox or a Playstation. We want a machine that's cheap, but powerful enough to simulate older consoles. 

We want a [Raspberry Pi](https://www.raspberrypi.org/)

![Alt](https://upload.wikimedia.org/wikipedia/commons/thumb/f/f1/Raspberry_Pi_4_Model_B_-_Side.jpg/2880px-Raspberry_Pi_4_Model_B_-_Side.jpg#center "The latest Raspberry Pi model")

This is a Raspberry Pi 4B. One of these small boys will set you back about £100 (with all the cables and whatnot). It's enough to make you think twice, but [nothing compared to the cost of the consoles it will be emulating.](https://www.ign.com/articles/2013/10/15/the-real-cost-of-gaming-inflation-time-and-purchasing-power) It's more powerful too. If we put the Pi side-by-side with a NES, the differences are stark

|                     | **Nintendo Entertainment System** | Raspberry Pi |
| :-----------------: | :-------------------------------: | :----------: |
| **Processor Speed** |              1.79MHz              |    1.5GHz    |
|     **Memory**      |                2KB                |     2GB      |
|   **Resolution**    |             256 x 240             |    4kp60     |

It's often hard to appreciate how fast computing power improves relative to other technologies. The NES had 2 KB of memory, which is `2 * 1024` bytes. The Raspberry Pi has 2 **GB** of memory, which is `2 * 1024 * 1024 * 1024` bytes. Not to overdo it, but the little Pi has _one million_ times more memory than the Nintendo Entertainment System.

This comparison speaks to something else about video games. A great video game isn't dependent on hardware. Some of the games we remember fondly don't require a big graphic load. Technology enables games, but is not the primary factor. 

### case

It's good to put a Pi in a case. Makes it easier to move around and keeps it safe. I decided to buy a case that looked like a NES, because of its iconic design and they were the easiest to find on Amazon.

![Alt](/pictures/nes_case.jpg#center)

It came with a little fan, designed to keep the computer cool while it runs. This fan was _very_ loud. I thought about replacing it, but ultimately decided against it. There was something about the noisy whirring of the fan that made the device feel more real. The case also came bundled with some controllers!

### retropie

We've got the box, but now we need the brains. 

As mentioned earlier, emulation is a popular field. Many programmers played _a lot_ of games as a kid. There are emulators for lots of different consoles available. 

There's an ongoing project to assemble as many emulators as possible into one neat software bundle. It's called [RetroPie](https://retropie.org.uk/). Installation is very easy, the instructions on their website are clear and thorough.

RetroPie acts as a sort of emulator librarian. Emulators are software, so can be stored in a file like any other application. That means we can store more than one emulator, as long as we have the storage for them. Then, when we need to use one for an old game, it's loaded from the file and ready to go. When we get bored, we can pick another one, load it, and play something else.

With a bit of computer plumbing, we can install RetroPie onto the Raspberry Pi.

{{< ticks >}}

* Some hardware
* Some emulation software

{{< /ticks >}}

The console completed, we need only some games.

## game time

Games for old consoles came in these physical cartridges. On board the cartridges was some hardware that stored all the software instructions that made up the game. 

![Alt](https://upload.wikimedia.org/wikipedia/commons/thumb/4/40/NES-Cartridge.jpg/542px-NES-Cartridge.jpg#center "Bit bigger than a USB stick!")

The older cartridges won't work with our console. You could find adaptors between the older interface and something more modern, like **USB** (**U**niversal **S**erial **B**us). But this would become infeasible pretty fast; you'd need a separate adaptor for every console. That's if you could find them at all!

Luckily, software can save the day again. These cartridges store a list of instructions. Someone can copy those instructions from the cartridge into a software file, and then share that file. These files are usually referred to as **ROM**s, named after the **R**ead **O**nly **M**emory that is present on game cartridges. 

Now, these ROMs are the intellectual property of the companies that created them. Distribution of them is a breach of copyright. Unfortunately, in many cases, it's also impossible to purchase them from the original vendor. I'm not going to point you in the direction of a place where you can obtain ROMs, but I will point out that others have made their own solution to this problem.

Once we've acquired some ROMs, all we need to is load them alongside the emulator.



## opening credits



That's it. We're done! Now the real work can begin. 

After all, my very own

![Alt](/pictures/pokemon_blue_beginning.png#center)

 