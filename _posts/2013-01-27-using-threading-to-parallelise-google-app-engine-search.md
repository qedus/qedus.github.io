---
layout: post
title: Using threading to parallelise Google App Engine Search Service
date: '2013-01-27T20:21:00+00:00'
tags:
- python
- google app engine
---
I ran into a performance/cost bottleneck with some of my code that uses the experimental App Engine [Search Service](https://developers.google.com/appengine/docs/python/search/). It currently does not let you asynchronously put documents into its search indexes, therefore I was getting the following display in App Stats when trying to insert 20 documents at a time:

![Non Threaded Search](/images/google-appengine-no-threading.png)

The whole task took over 4 seconds to execute with each document insertion being done sequentially. Note the datastore_v3.Put commands also add 20 entities at a time but use ndb.put_multi_async() to ensure they only use one RPC. It is executed in milliseconds.

With the python 2.7 runtime I changed my code to use [threading.Thread](http://docs.python.org/2/library/threading.html) for each of the document insertions and saw a big performance improvement:

![Threaded Search](/images/google-appengine-threading.png)

The task now takes under 3 seconds which means I can run with fewer instances which in turn costs me less. Ideally there would be a search.Index(name='index-name').put_multi_async() in the Search Service API but this is the next best thing.