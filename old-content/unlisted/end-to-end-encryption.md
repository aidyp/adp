---
date: "2020-12-07"
tags: ["cryptography", "theory"]
title: "the ends of end-to-end encryption"
toc: false
---



End-to-end encryption is in the news, again. Some people have asked me for my opinion on the issue but, truth be told, I'm not really sure which side I stand on. So I've decided to write a blog post to help people understand the technical issues involved, and then they can come to their own conclusion.

Technology -- at least the computer science part -- is mostly the science of solving problems that come about as the result of solving other problems. So in order to understand the issues around encrypted messaging apps, we're going to design one ourselves and see what problems we run into.

---



## diy: messaging app

So you want to build a mobile app that lets two people send messages to each other over the internet. 

![Alt](/pictures/make_an_app_start.png#center "diagram not to scale")

Sounds easy enough. Let's work through the problems we'll need to solve to build this application.

### problem: routing
{{< warning >}}

How does the green phone know where to send messages so that they will reach the red phone?

{{< /warning >}}

Each phone would need to know _where_ on the internet the other was in order to send the message. Devices are located on the internet through their **I**nternet **P**rotocol address. In order to send a message, Red needs to know Green's IP, and vice versa.

![Alt](/pictures/make_an_app_start_ip.png#center "ips may be different at home")

Okay, great. So all we need to do now is figure out a way to let each phone about each other's IP address. 

{{< warning >}}

How does the green phone know the IP address of the red phone?

{{< /warning >}}

Unfortunately the red phone can't just send its IP to the green phone because in order to send a message containing its IP address it would have to already know green's IP address. In the bizniz this is called a **bootstrap** problem. 

To make matters harder IP addresses are linked to the network you are on, not the device itself. This means that when a phone changes network -- home WiFi, cafe WiFi, 4g network -- it will have a different IP address.  Mobile phones move around a lot so this is going to be a recurring problem.

The most sensible solution to these problems is to have another device fixed somewhere on the internet. It's IP address will not change[^1]. This fixed point can act as a broker between devices. 

[^1]: This is not strictly true because we have DNS which gives some layer of abstraction but the important part is that its location on the internet is fixed

* Build an app for phones that allow people to message each other
* How do we deliver the messages from person to person?
* Okay we're going to need a server.
* But what if we want to keep the messages offline or whatever
* The server is going to have to do them





## encryption

The internet is not safe. It is a complex web of routers, switches and cables. You want to protect your users from having their message snooped.

* 