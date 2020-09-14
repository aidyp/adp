---
date: "2020-06-14"
tags: ["networks", "exploration"]
title: "internet explorer"
toc: false
---

This blog post is an exploration into the wiring of the internet. When we move information across the internet, what does its path look like? 

---

## to boldly go where many have gone before

### addressing the internet

When I was fifteen I had (or I thought I had) this very witty desktop background,

![Alt](/pictures/google_bit.jpg)

Now, years later, I have a better background and a computer science degree. But it turns out that the internet is not so different from the postal system. The two ideas, at a high level, are analogous. Consider how the postal service works. If you want to send a letter to someone else, you first wrap it an envelope. On that envelope you write the all the details the postman will need to deliver it: usually just the address of the recipient. You pop it in the post box, where it is subsequently taken to a mail centre. If the address is near by to the mail centre, that mail centre will deliver it. If not, it is moved to another mail centre from where it can be delivered. The internet works like this too. Just as the postal system is a collection of postboxes and mailrooms all intertwined, the internet is a collection of computers and routers connected to each other. When data is sent from some computer, `A`, to another computer, `B`, it travels across this web of connected devices.

![Alt](/pictures/internet_explorer_1.png "Blue sky thinking")

A machine's **IP** (**I**nternet **P**rotocol) address dictates where it can be found within this logical mesh. [some more explanation on IP addresses here, maybe extend the analogue from the postal system]

When we want to send a message between two points on the internet, the message is first divided up into **packets**. These packets are then labelled with the destination address, the source address, and other useful information. The labelled packets are then sent to the nearest routing station, called a router. This router examines the label on the packet, and forwards it on to either another router or, if the destination address is nearby, the destination itself. 



### building a cyber space ship
To explore these vast reaches of cyberspace, we're going to need an appropriate vessel. The packets I introduced earlier look like this.

![Alt](/pictures/ip_packet.png)

The data part is for the content of the message. The header contains those labels mentioned earlier, such as the destination address. Much like the address on the envelope of a letter, it is the headers that routers examine to move traffic through the internet. We'll dig into the headers further, and examine their structure. IP headers follow a strict format -- computers do not handle ambiguity well -- that looks like this,

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
The numbers on the top describe the size in bits of each header. The first four bits of an IP header are always the `Version`. The next four bits is always the `IHL`, which stands for **I**nternet **H**eader **L**ength. It describes how big the header is going to be. Just as you can have envelopes of different sizes, IP headers can be differently sized depending on which `Options` are present. The next eight bits is for the `Type of Service`, and so on and so forth.

The headers contain all the information the router needs to shift the packet along. The `Destination Address` describes where its going, and the `Source Address` describes where it came from. There are some other headers, like `Header Checksum` and `Fragment Offset` that we'll put aside for now. Our focus will be on `Source Address`, `Destination Address`, and `Time to Live`.

We're most interested in the **TTL** (**T**ime **T**o **L**ive) field. Packets move across the internet by traversing between routers. Each of these traversals is called a 'hop'. The `TTL` value for a packet determines how many hops a packets is permitted to make. Each router the packet passes through reduces this value by one. When the `TTL` reaches zero, the packet is dropped and its journey comes to an abrupt end. When a packet is dropped in this way, the router that jettisoned it will send a message back to the `Source Address`in the (now non-existent) packet. That message, enveloped in a fresh new IP packet, includes the IP address of that router. 

It's this property that we will leverage to map out paths along the internet.

### traceroute

We want to trace the route between the source IP address, `IP A`, and destination IP address, `IP B`. 

We begin by constructing an IP packet with `TTL=1`. This is forwarded to the first router, R1. When it reaches R1 the TTL will be decremented by one, leaving `TTL=0`. The packet will be dropped and the router will send back an error message. Because this error message *originated from* the router, it contains the IP address of the first router. 

The next step is to construct an IP packet with `TTL=2`. This is forwarded to the first router, R1. When it reaches R1 the TTL will be decremented by one, leaving `TTL=1`. The packet will then be forwarded to the second router, R2. When it reaches R2 the TTL will be decremented by one again, so `TTL=0`. The packet is dropped, an error message is returned, and we now have the IP address of the second router.

![Alt](/pictures/traceroute.png)

This process repeats until we've made a packet with a `TTL` high enough that it reaches the destination before being dropped. There's a neat piece of software that can do all of this, called `traceroute`. It's this that we'll use to light up the logical path that packets take between point A and point B.

An example `traceroute` output looks like this,
```
picard@voyager: traceroute 93.184.216.34
/* prior hops */
7  62.6.201.145 (62.6.201.145)  16.680 ms  14.572 ms  14.565 ms
/* later hops */
```
The first number, `7` is the index of the hop. In this case, it's the 7th router along the path. `(62.6.201.145)` is the address of that router. `16.860ms 15.572ms 14.565ms` are the total times taken between sending the packet from the source machine, and receiving the error message from the router. The internet is sometimes unreliable, so `traceroute` sends multiple packets for accuracy.


### houston, we need a visual

`traceroute` is a great tool to discover the *logical* nature of the path taken, but it doesn't give us much insight into the *physical* path. We want to know where in the world the packet has gone. Luckily, IP addresses are (sort of) tied to geography. When public IP addresses are registered, they're registered to a particular place. With a little code, we can tie together the geographical location of an IP address and plot it on google maps. For example, `www.example.com` (which has an IP address of `93.184.216.34`) is located here,

![Alt](/pictures/example_com_map.png)

If we take the IP address from each router obtained by `traceroute` and plot its geographical location on a map, we can visualise the physical path that the packets take between two locations on the internet.

{{< expandable label="Domain Name System" level="3" >}}
Locations on the internet are described by their IP address, but that's not how we see them in everyday usage. Instead of typing in `93.184.216.34` into a browser, we type `www.example.com`. This is a **domain name**. The early inventors in the internet realised that it just wasn't feasible to have people remember IP addresses. Moreover, an IP address only describes a machine on the internet. [need to explain this a bit better]

To solve this problem, early pioneers of the internet invented **DNS** (**D**omain **N**ame **S**ystem). It's like a phone book. Your computer looks up the name of the website you want to visit in this phone book and gets back its IP address. That's the IP address used for all the packets sent to the website.  Your browser handles this process automatically when you make a connection to a website. You'll only ever notice this when it goes wrong. 

{{< /expandable >}}


## warp speed

With mission control set up, we can set Time To Liftoff to `0` and blast off.

### mission: new zealand

I've never been to New Zealand and I've always wanted to go. Covid-19 has put a stop to travel, but not to our regular programming. Let's go!

The New Zealand government website seems a good destination. Their website, `www.govt.nz`, is found at `103.28.251.187`.

### plotting the route

First step is to run the `traceroute` program. This will give us the logical path between us and New Zealand
```
picard@voyager: traceroute www.govt.nz
/* the first few IP addresses are redacted,
I don't want you to know where I am! */
 5  31.55.187.180 (31.55.187.180)  15.513 ms  16.553 ms  17.264 ms
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

The final part of each line is the time taken between my computer sending the `traceroute` packet and the corresponding error message to return. This is called the **latency**. Between,

`10  ae-4.r24.londen12.uk.bb.gin.ntt.net (129.250.4.125)  16.257 ms` 

and,

`11  ae-7.r21.sngpsi07.sg.bb.gin.ntt.net (129.250.7.65)  202.559 ms`

there is a big jump in latency. Hop `11` is when the packet left the UK and travelled across the globe.

### pinning the IP

By running each IP from our `traceroute` output against a geo-location service, we get the longitude and latitude for each IP. To make it easier to pin to the map later, I've re-indexed the IPs by letter.

```
A | 31.55.187.188   | (51.5074, -0.127758)
B | 213.121.192.112 | (51.6143, -0.7944)
C | 62.172.103.166  | (54.4, -7.5833)
D | 166.49.128.32   | (51.5074, -0.127758)
E | 212.119.4.136   | (51.5061, -0.071338)
F | 129.250.4.125   | (51.5074, -0.127758)
G | 129.250.7.65    | (1.35208, 103.82)
H | 129.250.3.74    | (1.35208, 103.82)
I | 129.250.7.67    | (22.3119, 114.257)
J | 129.250.6.92    | (22.3119, 114.257)
K | 192.80.16.50    | (38.5816, -121.494)
L | 103.28.250.187  | (-33.8591, 151.2002)

```

Then we overlay this data on Google Maps to get the physical path.

![Alt](/pictures/ldn_nzl_big.png "Bit of a trek")


Some of the letters are a bit jumbled up on google maps. Hop `D` and hop `F` have the same co-ordinates. This is because some of the hops on the journey are in different logical parts of the network, but the same physical part of the network.

### bouncing around britain

![Alt](/pictures/ldn_bzl_brt.png "First stop, High Wycombe?")

My internet service provider is BT, who divide their infrastructure up into 'code' nodes and 'metro' nodes. 


### transatlanticism

Once our tiny packets reach a peering node,  they traverse seas and oceans on their journey to New Zealand. That they are able to do this is no small feat of engineering. There are enormous cables laid across the ocean floor that connect the networks of individual countries and continents together. These cables are [mapped here](https://www.submarinecablemap.com/). With a bit of guesswork, we might be able to figure out exactly _which_ of these submerged data pipes our packets travelled down.

### coming full circle

`traceroute` can only tell us the path packets take on the way to the destination. It can't determine which route the packets take to return to us. Routing on the internet is complicated and dynamic; different days will yield different routes. Perhaps `www.govt.nz`  sent the reply back the same way, or it may have decided on a different route. There are routers along on the west coast of the US. From there, packets would have travelled across the states to the east coast. Then they'd have boarded a transatlantic cable to the UK. After bouncing around London for a little while, the packets would have eventually reached my laptop.



## one small hop for data, one giant leap for information

The total time it took for the packet to go to New Zealand and back was 236 milliseconds. For context, the average human reaction time is 215 milliseconds. It takes approximately the same time for information to cross the globe as it does for you to react to it. This is _fast_. But it can get faster. Today's internet aims to reduce the time you have to wait for the content you want. Why should I have to travel all the way to New Zealand just to view a website? 

They can meet me in the middle. Digital information can be copied almost instantaneously. A website that was created in New Zealand can be copied and replicated on a server closer to home. This principle of replicating data closer to the consumer is called **caching**. Take `netflix.com`. It's an American company, so in theory I have to send data to America to get movies. In practice, `netflix.com` has servers in the UK that have copies of all the content. If I want to watch a movie, it will be downloaded from a server much closer to my computer. Netflix did something like this when it was a DVD rental company. They'd have depots from which they could distribute physical disks. In the digital world, it's much easier to clone information. Services like Netflix can proliferate around the globe with ease.

I read once that one aim of human progress is to compress the space and time between people. I think the internet is part of that. The wires and cables that connect it are the threads that tie the modern world together.  Zooming out to look at the whole, it's extraordinary just how well the world is connected. And we're not done yet.

![Alt](https://upload.wikimedia.org/wikipedia/commons/9/91/Starlink_Mission_%2847926144123%29.jpg)


