---
layout: post
title: Several reasons why Bitcoin is not perfect
date: '2014-01-22T12:10:00+00:00'
tags:
- works
- bitcoin
- distributed systems
---
Here's a non exhaustive list of reasons why [Bitcoin](http://bitcoin.org/) is not perfect. They are not necessarily in order of impact and also note that I am a big fan

## 51% attacks could be too easy

###Problem

The Bitcoin protocol's security works on the assumption that the majority of nodes implementing the protocol are legitimate and not trying to subvert the network. As it stands today, 22nd January 2014, [GHash.IO](https://ghash.io/) controls 34% and [BTC Guild](https://www.btcguild.com/) controls 24% of the world bitcoin mining operation. In fact last week GHash.IO controlled 45% of the mining operation for a few hours.

![Spread of Bitcoin Mining Pools](/images/mining-pools.png) 

This means that all I need to do is make friends with GHash.IO and BTC Guild to have control of the block chain. Having control of the block chain would allow me to reverse other peoples new transactions, exclude transactions and double spend. A big concern is that even with the way the situation is now GHash.IO or BTC Guild could hold Bitcoin users ransom for ever increasing transaction fees. Of course there are arguments why it is not in anyones best interest to mess with the block chain, but since when did people only do things in their own best interests?

### Solutions

* Use a memory-hard instead of CPU hard problems for [proof-of-work](http://en.wikipedia.org/wiki/Proof-of-work_system) like [Litecoin](https://litecoin.org/) to reduce the usefulness of Application Specific Integrated Circuits (ASIC). A memory-hard problem is basically one that requires lots of memory. The assumption is that memory-hard algorithms make scaling out ASIC farms more expensive and therefore less profitable.
* Discourage private mining pools by making it easier to for nodes to join decentralised ones.
* Use [proof-of-stake](https://en.bitcoin.it/wiki/Proof_of_Stake) or combine it with proof-of-work mining methodologies to further discourage someone trying to subvert the blockchain like [Peercoin](http://peercoin.net/).


## The blockchain has errors in it

### Problem

The block chain has a few errors in it from oversights with previous Bitcoin clients. The errors I am aware of are with blocks 91842 and 91880. The oversight was that duplicate transactions were not checked for within the blockchain up until [Bitcoin Improvement Proposal 0030](https://github.com/bitcoin/bips/blob/master/bip-0030.mediawiki) was introduced. This means that any Bitcoin client that validates the block chain must build exceptions into its code for these two blocks. This situation reminds me of a [story by Joel Spolsky](http://www.joelonsoftware.com/articles/APIWar.html) of IF/OR statements being included into the Windows operating system to make sure SimCity could run properly. Soon the problem of catering for each exception became intractable.

### Solution

I can't think of one but there will almost certainly be more issues within the block chain like this in the future. No one has perfect foresight to prevent this type of problem. However for the majority of use cases with Bitcoin, full validation of the block chain is not needed.

## Protocol documentation is not complete

### Problem

The [documentation](https://en.bitcoin.it/wiki/Protocol_specification for the Bitcoin protocol is not complete. If you were to implement your own client you would come across nuances that are not documented on the official wiki and instead you must hunt across the internet or look at the official Bitcoin client C++ code for information. An example of the nuances are having to cater for the errors in the block chain mentioned above.

### Solution

Make the documentation easy to read and up-to-date. I tried when developing my own client but didn't have any bitcoins at the time to allow me to update the wiki with my edits.

## The official bitcoin client code is a mess

### Problem

The [official Bitcoin client code](https://github.com/bitcoin/bitcoin) is nasty. It looks like a bunch of academics made it with the coding philosophy of live now and forget about the future. The code is very difficult to read, not unit tested nearly enough and just nasty. I am quite surprised that it works as well as it does and not laden with bugs (maybe it is). Having said that, the architecture seems to be slowly improving with each new commit.
The reasons for clean, readable, consistent, well architected and well tested code are obvious. The readable part is especially important for Bitcoin seeing as the protocol documentation is not up to scratch. I believe for Bitcoin to be robust there must be a large variety of different clients interacting, not just the official one. Right now the barrier to creating a new client is very high.

### Solutions

* Continue to refactor the current code base to something that developers can take pride in and understand easily.
* Possibly a bit controversial, but it might be worth scrapping the entire code base all together. This is generally thought to be a big mistake in software development worlds. However [Conformal](https://github.com/conformal) have open sourced their own Bitcoin client. I rarely see a code base as beautiful as theirs and encourage anyone interested in understanding how software development should be done to review it. It is astonishing. What's more all the Bitcoin protocol nuances are contained within their wonderful code and its inlined documentation. I would love to see the Conformal client become the official one, i.e. the one that is promoted by [bitcoin.org](http://bitcoin.org/). I already have more trust in it despite it only being a alpha/beta product.

## Default minimum transaction costs too much

### Problem

The default minimum transaction costs remain fixed with the official Bitcoin client. This does not make sense. A year ago one bitcoin was worth 10 times less than it is now. The cost to include a transaction within a block by a miner is independent of this meaning default transaction costs may either be too high or too low for any one transaction depending on the intent of the participants.

### Solutions

* Change the default transaction fee to 0. This would mean that someone making a transaction would have to consciously think about how much they want to give - if anything at all.
* As transaction fees are generally related to speed of processing transactions, make the default transaction fee variable based on how fast someone wants their transaction to go through. This could be calculated by looking at previous blocks.

## Transaction confirmation time is not granular enough

### Problem

On average a block is created every 10 minutes. To reduce your chances of being defrauded by a double-spend attack you should wait for your transaction to be buried by at least two blocks. This will take on average 20 minutes. However if you are extra cautious then you might want to wait for 6 blocks. You can imagine two people meeting up to exchange bitcoins and having to wait 20 minutes before the recipient trusts he is not being defrauded.

### Solutions

* If the transaction sender and receiver really want an instant transaction then they would both have to agree on a third party cloud based wallet which they both trust. The cloud wallet would and own both parties private keys and not disclose them to their users. This would possibly lead to the tendency of very few cloud based wallet platforms emerging which goes against decentralisation.
* Make the average block creation time shorter. Note that this would not actually speed anything up in terms of security but would give a more granular view on how secure your transaction was. This would be at the expense of increased network bandwidth and the likelihood of more sidechains.

## Transaction throughput will be too small

### Problem

For the Bitcoin protocol to be useful it will need to be able to scale to at least 20,000 transactions per second like VISA and Mastercard combined when at their maximum. These figures may need to be even higher due to the ease of making a transaction and the ability to make micropayments compared to VISA and Mastercard. These figures are possible according to [this document](https://en.bitcoin.it/wiki/Scalability) but only for nodes with powerful CPUs and large network bandwidth. Although with Moore's Law and ever increasing bandwidth we might be fine, for me there are two problems. Poorer, less connected countries/regions will be excluded from participating in the Bitcoin network. The average user will be less incentivised to run their own full node in the background if it means several of their CPU cores and much of their bandwidth will be taken up. This will leave the network open to mining pool monopolies.

### Solutions

* Use the [Merkle Tree](http://en.wikipedia.org/wiki/Merkle_tree) idea as suggested in [Namkamoto's paper](http://bitcoin.org/bitcoin.pdf) where possible to save network bandwidth, CPU cycles and disk space.
* Batch recent blocks up and compress them before sending them over the network to allow nodes to participate intermittently i.e. once they catch up to save network bandwidth.
* This is just speculation but I think I will mention it anyway. Currently proof-of-work is broken down into non-divisible units called blocks. Maybe it would be possible to have to have units of work that were divisible and a block could only be validated if the divisions summed to one whole unit. This would allow less powerful nodes to play a part in the Bitcoin network without using up their network bandwidth or CPU. There would probably need to be the concept of super nodes that would be in charge of creating a decentralised mining pool and dishing out divisions of work.

*The following are slightly more esoteric issues I have with Bitcoin:*

## Little endian and big endian protocol mixups

### Problem

The bitcoin protocol uses little-endian for some things and big-endian format for others. This is just a pain more than anything else and it would just be better if one format was stuck to.

### Solution

At this point, there really isn't much point in changing anything.

## Odd elliptic curve algorithm

### Problem

The elliptic curve digital signature algorithm that is used by the Bitcoin protocol is odd (secp256k1). It is not recommended by NIST and not usually included in cryptographic software libraries. The reason secp256k1 was used is not given but there are [conspiracy theories](https://bitcointalk.org/index.php?topic=151120.0). From a software development point of view, this just makes it a pain to develop as you need to code your own parameters to for the algorithms. Have a look at this [link](https://github.com/conformal/btcec") to see the code required to make secp256k1 work with the Go language. More code equals more potential sources of errors. If a standard ECDSA was used, almost none of that code would be needed.

### Solution

It's too late now for Bitcoin but if anyone decides to make a new cryptocurrency,  if a non-standard crypto algorithm is used please give your reasoning and proofs that the standard ones are insufficient.

## Double SHA256 hashing

### Problem

Some of the cryptographic hashes are done twice, some aren't. No reason is specified but there is at least one [hypothesis](http://bitcoin.stackexchange.com/questions/6037/why-are-hashes-in-the-bitcoin-protocol-typically-computed-twice-double-computed). Until I can see a good reason I am going to assume it is done for paranoia. Paranoia is not a good reason. It just frustrates someone trying to make a client with inconsistencies between single hashing some things and double hashing others.

### Solution

There's no solution now - we are stuck with it. Maybe it was the right thing to do and I just have not see the evidence yet.