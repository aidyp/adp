---
date: "2020-12-07"
tags: ["cryptography", "theory"]
title: "the ends of end-to-end encryption"
toc: false
---



The Social Dilemma (Netflix) has been a huge hit.

One thing that stood out to me was the treatment of Whatsapp. In the show, it was included as one of a plethora of applications that Facebook use to gather data. Some of my friends noticed that too. Whatsapp is end-to-end encrypted. How could it ever be a source of data? The principle of end-to-end encryption, as marketed and widely understood, is that nobody but you and the person you are talking to can read the messages. 

This blog post is about why that's not _exactly_ accurate.

---

I want to set out by saying I am _not_ accusing Facebook of reading your Whatsapp messages. I am saying that you cannot trust cryptography alone to protect your messages. First, what _does_ cryptography protect?

## end-to-end encryption



