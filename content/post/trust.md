---
date: "2014-09-28"
tags: ["hugo", "theme", "command-line"]
title: "trust, online"
toc: false
---

## the green padlock

This blog explores what "your connection is secure" means. 

Suppose you ran a bar and someone comes and orders a drink. You ask them for their ID, and they show it to you. 

Or suppose you receive a letter in the post. It's a bill from your gas company, claiming that you owe them some £££. How do you know that it's really the gas company that sent the bill, and not some 

What if you work at a bank, and someone comes in with a cheque that they claim was given to them by a third party. How do you know that really party really signed the cheque?

These problems are all present in the digital world too, with one notable exception. In the physical world, we can make things hard to copy. IDs can be made with watermarks so they're not easy to clone. Written signatures are hard to forge. In the digital world, anything can be copied, effortlessly and often without alerting anyone.

## encrypted != trusted

We're often told that our connection is secure because it is encrypted. This is partly true. Encryption is a necessary condition for a safe connection. But encryption alone doesn't speak to the trustworthiness of a connection.

Imagine you had a pen-pal on the other side of the world, and both of you were particularly paranoid. For whatever reason, you worried that people might read your messages, or even change the content of the message. You use special tamper-proof envelopes. You can be assured that whatever you send to your pen pal could not be read by anyone else. One day you receive a letter claiming to be from your pen-pal. 

But how do you know? Anyone could put a letter in a tamper-proof envelope and send it to you. The envelope is intact, so nobody tampered with it on its journey. But that the mechanism used to transmit the message is safe from tampering is not proof that the message itself is trustworthy. 

It's this trust property that the green padlock is also trying to solve. To enforce trust online, there is cryptography.

## introduction to (some) cryptography

On the internet, trust is enforced by leveraging some cryptographic functions. The green padlock makes use of **digital signatures**.

An analogue signature has two parts. There's the signature, which is public. Then there's the way you sign it, which is private. In theory just because you can see the signature scrawled on a piece of paper, doesn't mean that you can replicate it. Knowing the public part, the signature, doesn't give you knowledge of the private part - the signing.

The digital version operates on the same idea. There are two separate keys, a **public key** and a **private key**. 

{{< expandable level=3 label="mathematics of digital signatures" >}}

There are different mathematics used to achieve the necessary properties for digital signatures. We'll dig into a simplified version of **RSA**. 



At school, the number line goes left and right to either end of infinity. 

{{< /expandable >}}

### symmetric key encryption

One key

### asymmetric key encryption

A public key, and a private key. 



## trust, chained
With the above cryptographic tools under our belt, we're ready to build an encrypted connection between two points. 

The best way to encrypt a lot of data is to use a symmetric encryption algorithm. If both parties have the key, then we're good to go. 


## in {browser_name_here} we trust
These chains of trust have a root. That root is trusted by default. But who decides which roots are trustworthy, and which are snake-oil?

Your browser does. A browser comes bundled with a list of trusted root certificates. I went to a talk on cyber awareness once, and the presenter had just encountered this reality. He told us that he had gone through the list of root certificates in his browser and removed the ones he didn't like the look of.

I don't personally advise you do this. As long as you use a browser installed from a trusted vendor, your root certificate list should be pretty safe. That said, if you're ever asked to add a new certificate to your root certificate store please be wary[^1]

[^1]: Some company IT policies require you to do this. This is so that the IT department can monitor encrypted traffic flowing in and out of a corporate network. The unit of privacy for a company is larger than for an individual. If your IT department asks you to do this, it is safe. If anyone you _don't_ trust asks you to do this, it's not safe.






## future of trust

Computer science continues to make huge strides. As a result, we are better than ever at manipulating data and information. Where once we could watch a video of Obama speaking and reason that it must have been Obama speaking, now we have deepfakes. [Wording]. Basically I want to say that even the content we once thought inherently trustworthy is now vulnerable to manipulation and editing.

Trust is a really hard problem. It's also not a problem unique to the realm of computing.