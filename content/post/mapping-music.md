---
date: "2020-06-25"
tags: ["exploration", "music", "maps"]
title: "mapping music"
---

## mapping music
{{< spotify "7G6oI8xoGa62LGvE2xjtfP">}}

During lockdown, like you, I've listened to *a lot* of music. I've listened to so much music that I've found myself thinking about how to think about the ways to listen to music. This blog is a dive into one of those ways.

### oh man you're gonna *love* this next track 

At parties, when we used to have parties, I remember finding myself listening to a song and immediately thinking about what song I wanted to listen to next. The second track hadn't been on my mind before the first track played; it came to me only upon hearing that first one.

This, to me, defines a connection between two songs. It reminds me of the word association game, where one person says a word and you say the first related word that comes to your mind. The same is true of songs. This naturally leads to the question, "How do *I* connect songs together?".

Let's find out.

------

#### theory (better title needed)

{{< spotify "3gRlmtdCyNoKiyozn2pqc9">}}

We can formalise this link between two arbitrary songs, A and B. Let's say `A --> B` means, "After song `A`, I want to listen to song `B`". 

// Drawing here

Now that we have defined this relationship we can use some mathematics to examine its nature.



**symmetry**

A relation (let's call it `r`) is symmetric if `A r B` implies that `B r A`. In our case, if you want to listen to song `B` after listening to `A` , does it follow that you'd be happy to listen to `A` after `B`?

In my opinion, yes. The link between the two songs can be traversed in either direction.

**transitivity**

A relation (`r` again) is transitive if you can connect disjoint relations. Specifically, suppose we have 3 elements with 2 relations. The relations are, `A r B` and `B r C`. A relation is transitive if, from these, it follows that `A r C`.

I argue that, for our relation, it shouldn't be transitive. I might want to listen to {song 2} after {song 1}. Then, after {song 2}, I might be feeling {song 3}. That doesn't mean I'd want to jump straight from {1} to {3} -- it'd to be too jarring!



We can use this relation to link different songs together. Here's what it might look like, 

![Alt](/pictures/banger_graph.png "when i touch that track it turns into gold")

How does this differ from a playlist? In a playlist, when you hit shuffle, the songs are arranged in a random order. With respect to our relation, it means that any song might follow any other. So a playlist, as a song graph, looks like,

![Alt](/pictures/mesh_graph.png "all together, right now")

The theory now cemented, we need now to work out how to:

1. Build this graph of songs
2. Play it

To do the first we need a large, easy-to-search database of songs. To do the second, we need a tool that can play songs from that database. Spotify provides both.

------

###  spotify

The people behind Spotify are saints. They've exposed an **API** (**A**pplication **P**rogramming **I**nterface) so that professionals and amateurs alike can write code to extend on their product. Usage of this API, as long as you don't go nuts, is completely free.

Through this API, we can leverage (*read*: abuse) a whole suite of Spotify's features. The first, and perhaps most important, is the search feature. We can search for any song by title, artist, album, etc. Spotify gives every song a unique identifier, which is part of the information that's returned by a search. That's what we'll store. The second is the playback feature. We can play any song on Spotify by referencing its unique identifier. With a list of unique identifiers that we searched for earlier, we can play them back in any order we choose.

{{< expandable label="Application Programming Interfaces" >}}

Any large computer program is a collection of smaller programs and systems working in harmony to solve problems. Application Programming Interfaces are one of mechanisms use to glue these different parts together.

Spotify presents a **R**E**ST**ful (**R**epresential **S**tate **T**ransfer) API. The terminology is a little opaque, but the intuition isn't so tricky. The idea is analogous to a vending machine. You provide the vending machine with a code and, if the code is valid, the vending machine provides you back with the drink corresponding to that code. We use the digit code as an interface between ourselves and the vending machine. That interface was designed by the vending machine manufacturer to make it easy to get the drink you want.

![Alt](/pictures/vending_machine.png "thirsty work")

Spotify's API is similar. The developers have designed an interface that allows computer programs to easily get the data they want. If we want to search a song, we enter a specific search code. Then, if the code is valid, Spotify provides you back with the results of your search. In Spotify's case, this result is the unique identifier for the song. 

![Alt](/pictures/spotify_vending_machine.png "hello from the other side")

With a little computer code, you can write software that lets you quickly search Spotify's music database and do with the results whatever you please. If you'd like to know more about APIs, there really is no better introduction than [Tom Scott's video on the subject](https://www.youtube.com/watch?v=BxV14h0kFs0)

{{< /expandable >}}

------



### building the graph

{{< spotify "0pQskrTITgmCMyr85tb9qq">}}

We need a way to build the graph. We could do it by hand; We write down all the songs we can think of and then draw lines between them. This is tedious, and boring. The art of programming is mostly the art of laziness, and this sounds like far too much effort.

A better way is to leverage the principle from which this idea sprung. Listen to a song as normal, and then queue up whichever song appeals next. By keeping track of the song currently playing, and the one that's just been queued up, a link can be formed automatically. That way, we can build the graph just by listening to music. Easy, and fun.

![Alt](/pictures/listen_along.png)

This poses an immediate problem. What if, while listening to a song, you queue up multiple songs to listen to after. How do they fit into the graph? Suppose we queue up 3 seperate songs, `B`, `C`, `D`. Clearly they should all be related to the first song, `A`.

![Alt](/pictures/listen_along_multiple.png)

What of the relations between `B, C, D`? Personally, I reasoned that they ought all be connected to each other. That is,

![Alt](/pictures/listen_along_multiple_meshed.png)

If you're comfortable listen to `B` and `C` after `A`, it follows that you're comfortable listening to `C` after `B`. Track `A` put you in a state of mind that lead you to `B` and `C`. I argue that `B` will probably put you in the state of mind that leads you to `C`, and vice versa.

Then we can listen along, adding songs as we see fit. As we listen, the graph will naturally start to fill out, the connections aligning to which tracks were queued relative to which tracks were being played at the time.

![Alt](/pictures/listen_along_fleshing_out.png)

I wrote some code to implement this idea, and then borrowed some graph visualisation software online to see what the results would look like. After a few nights of listening, my final song graph had grown to be pretty big!

![Alt](/pictures/song_constellation.png "ground control to major song")

Now that I look at it, it is less like a graph and more like constellations. Here's a closer look at one such constellation, with track labels preserved.

// Graph of a zoomed in version with the labels attached.

Now that the graph exists, the next task is to determine the natural way to play it back.

------



### exploring the graph
{{< spotify "6Qb7gtV6Q4MnUjSbkFcopl">}}

Now that the song graph is constructed, how best to play it back?

We have to start somewhere. In fact, we can start anywhere. So it's best to pick randomly. 

// Diagram of song graph with one coloured in

There are two classic ways to explore a graph. Go wide, or go long. Specifically,

// Diagram of song graph with the 'wide' version and also the 'deep' version.

In this case, the 'depth' approach makes the most sense. One song ought naturally follow another. We want to follow one line to its end, rather than start many different lines all at once. 

// Should I include code for a depth-first search? 



------


### onwards and upwards
{{< spotify "5R96PHcqOGjgj23D98F6mf">}}

There remains a lot unformed in this idea