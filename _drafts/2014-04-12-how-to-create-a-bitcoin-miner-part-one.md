---
layout: post
title: How to create a Bitcoin miner - Part one
date: '2014-04-12T12:00:00+00:00'
tags:
- bitcoin
---
## What is a Bitcoin miner?
In this series of articles I am going to guide you though making a computer program that can connect to the bitcoin network, [mine](https://en.bitcoin.it/wiki/Mining) new bitcoins and archive blocks of transactions in the bitcoin blockchain.

## Why is this important?
You probably won't get rich from mining bitcoins with the mining software you create but you will be doing a service that is vitally important to the robustness of the network. Even though the Bitcoin network is decentralised making it robust in the face of network disruption, it is not necessarily robust in the face of software bugs and exploits. This was brought to light with the recent [SSL heartbleed bug](http://heartbleed.com) that affected the security of a significant portion of Internet traffic. The reason it affected so much of the internet was because everyone affected was using the same OpenSSL software. Imagine if everyone used the same bitcoin mining software and an exploit was found, one person could take down the whole network by maybe double spending, reversing transactions or denial of service attacks. NASA reduced the chances of catastrophic software failure with their [Space Shuttle](http://en.wikipedia.org/wiki/Space_Shuttle#Flight_systems) by having several redundant flight control systems all monitoring each other just like the bitcoin network. This *also* included at least one flight control system designed and written by a completely separate team in the hopes that if the other systems were affected by a specific bug, this one would not.

Even though there are a fair amount of different mining programs out there, I imagine they all evolved from relatively few different code bases. I expect this is especially true of the ASIC embedded software. 

## Where to start?
Theoretically everything you need to create your own mining software is located within Satoshi Nakamoto’s paper [Bitcoin: A Peer-to-Peer Electronic Cash System](https://bitcoin.org/bitcoin.pdf) and the [Bitcoin Protocol Specification](https://en.bitcoin.it/wiki/Protocol_specification). I encourage you to read Nakamoto’s paper to gain an understanding of the fundamentals of Bitcoin and the Protocol Specification to see how the network actually communicates.

When I started to make my own mining software I realised it wasn’t so easy. There were several nuances that aren’t picked up within these documents and several software architecture holes I fell into. With this series of articles I hope to point out the nuances and give guidance on architecture. As far as software architecture goes, I rate [Conformal’s btcd](https://github.com/conformal/btcd/) highly and will be roughly following their design decisions.

I envisage the subjects of the upcoming posts will be as follows but with more granularity:

1. Communicating with the Bitcoin network.
2. Persisting the block chain.
3. Verifying the block chain.
4. Creating blocks.

I don’t expect there to be much code with each post but I hope to provide examples of key algorithms in a variety of different languages.
