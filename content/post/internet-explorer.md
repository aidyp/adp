---
date: "2014-09-28"
tags: ["networks", "exploration"]
title: "internet explorer"
---

## to boldly go where many have gone before

This blog post is an exploration into the wiring of the internet. When we move information across the internet, what does its path look like? 

### addressing the internet

The internet looks a bit like this,

![Alt](/pictures/internet_explorer_1.png "Blue sky thinking")

A machine's **IP** (**I**nternet **P**rotocol) address dictates where it can be found within this logical mesh. When we want to send a message between two points on the internet, the message is first divided up into packets. These packets are then labelled with the destination address, the source address, and other useful information. The labelled packets are then sent to the nearest routing station, called a router. This router examines the label on the packet, and forwards it on to either another router or the destination itself.



{need a better segue here}. 

### building a cyber space ship
To explore these vast reaches of cyberspace, we're going to need an appropriate vessel. Every packet on the internet looks a bit like this,

// picture of IP packet

The headers contain those labels mentioned earlier, such as the destination address for that particular packet. We'll dig into the headers further, and examine their structure. An IP header looks like this,

```
 0                   1                   2                   3  
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|Version|  IHL  |Type of Service|          Total Length         |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|         Identification        |Flags|     Fragment Offset     |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|  Time to Live |    Protocol   |        Header Checksum        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                         Source Address                        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                      Destination Address                      |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Options                    |    Padding    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

We're most interested in the **TTL** (**T**ime **T**o **L**ive) field. Packets move across the internet by traversing between routers. Each of these traversals is called a 'hop'. The TTL value for a packet determines how many hops a packets can make. Each router the packet passes through reduces this value by one. When the TTL reaches zero, the packet is dropped and its journey comes to an end. When a packet is dropped in this way, the router that jettisoned it will send a message back to the `Source Address`. 

It's this property that we will leverage to explore the internet.

### traceroute

We want to trace the route between the source IP address, `IP A`, and destination IP address, `IP B`. 

We begin by constructing an IP packet with `TTL=1`. This is forwarded to the first router, R1. When it reaches R1 the TTL will be decremented by one, leaving `TTL=0`. The packet will be dropped and the router will send back an error message. Because this error message *originated from* the router, it contains the IP address of the first router. 

The next step is to construct an IP packet with `TTL=2`. This is forwarded to the first router, R1. When it reaches R1 the TTL will be decremented by one, leaving `TTL=1`. The packet will then be forwarded to the second router, R2. When it reaches R2 the TTL will be decremented by one again, so `TTL=0`. The packet is dropped, an error message is returned, and we now have the IP address of the second router.

// Diagram goes here

This process repeats until we've made a packet with a `TTL` high enough that it reaches the destination before being dropped. 



### houston, we need a visual
`traceroute` is a great tool to discover the *logical* nature of the path taken, but it doesn't give us much insight into the *physical* path. We want to know where in the world the packet has gone. Luckily, IP addresses are (sort of) tied to geography. When public IP addresses are registered, they're registered to a particular place. With a little code, we can tie together the location of an IP address and overlay it on google maps. For example, `www.example.com` (which has an IP address of `93.184.216.34`) is located here,

// Google maps of geo location of 93.184.216.34

## Warp speed

Now that we have mission control set up,  we can do some exploring.

### mission: new zealand

I've never been to New Zealand, and I've always wanted to go. Corona has put a stop to regular travel, but not to our regular programming. Let's go!

The New Zealand (government?) website seems a good start. 
// website & IP here

traceroute output
```
$ traceroute www.govt.nz
/* the first few IP addresses are redacted,
I don't want you to know where I am! */
 6  core1-hu0-6-0-6.southbank.ukcore.bt.net (213.121.192.72)  17.960 ms 
 7  peer7-et-3-0-2.telehouse.ukcore.bt.net (109.159.252.180)  10.041 ms  
 8  166-49-128-32.gia.bt.net (166.49.128.32)  16.323 ms
 9  212.119.4.136 (212.119.4.136)  16.294 ms
10  ae-4.r24.londen12.uk.bb.gin.ntt.net (129.250.4.125)  16.257 ms 
11  ae-7.r21.sngpsi07.sg.bb.gin.ntt.net (129.250.7.65)  202.559 ms
12  ae-7.r20.sngpsi07.sg.bb.gin.ntt.net (129.250.3.74)  200.066 ms 
13  ae-9.r24.tkokhk01.hk.bb.gin.ntt.net (129.250.7.67)  232.833 ms
14  ae-1.r02.tkokhk01.hk.bb.gin.ntt.net (129.250.6.92)  231.986 ms
15  192.80.16.50 (192.80.16.50)  275.643 ms
16  103.28.251.187.ip.incapdns.net (103.28.251.187)  236.324 ms

```





then geographical screenshots go here



// analysis of the geographies goes here

## Where is stuff

Joe mentioned a really good point about AWS. Now that there are so many things based on the cloud, there is a clustering of all the services. I don't have any comment about whether or not this is a good or a bad thing, I just think it's interesting.

Maybe something to do w/ netflix, where it's hosted, etc. Can talk about caching & other features (to the best of my knowledge, may be wrong)
