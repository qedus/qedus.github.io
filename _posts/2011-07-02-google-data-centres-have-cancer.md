---
layout: post
title: Google data centres have cancer
date: '2011-07-02T18:58:00+01:00'
tags:
- google
- distributed systems
- cancer
- amazon
---
I noticed in Mike Handler's talk [Google I/O 2011: Life in App Engine Production](http://www.google.com/events/io/2011/sessions/life-in-app-engine-production.html) and the [Paxos Made Live - An Engineering Perspective](http://static.googleusercontent.com/external_content/untrusted_dlcp/labs.google.com/en//papers/paxos_made_live.pdf) paper that engineers of complex distributed systems face a difficult problem.

<iframe width="560" height="315" src="//www.youtube.com/embed/rgQm1KEIIuc" frameborder="0" allowfullscreen></iframe>

The problem is they can't rely on anything. When dealing with massive scale, the improbable or seemingly impossible becomes probable. A flipped bit due to electromagnetic interference at the wrong time can make 1 + 1 not equal 2 somewhere in the system.

The human body, being a massively scaled chemical engine, has to deal with this also. What happens if you are unlucky and an ionised particle flips a bit of your DNA and 2 + 2 = 3 instead of 4? Generally not much. Your gene checksum mechanisms kick in making the ribosomes fail to read the faulty DNA. Or maybe your white blood cells release [Tumour Necrosis Factors](http://en.wikipedia.org/wiki/Tumor_necrosis_factors) to terminate the bad cells. If even one of these multitude of mechanisms stop working, you may be in trouble - see [Li-Fraumeni Syndrome](http://ghr.nlm.nih.gov/condition/li-fraumeni-syndrome) for an example.

However, sometimes you get cancer, a cell state that is immune to the damage limitation systems in your body. The cancer grows and consumes an ever increasing amount of your resources until your organs stop and you die.

Due to the increasing probability of the once improbable, Google, Amazon and any other owner of massively scaled distributed systems can and should expect their systems to get electronic unstoppable, metastatic cancer once in a while.

Hopefully no one dies as a result.