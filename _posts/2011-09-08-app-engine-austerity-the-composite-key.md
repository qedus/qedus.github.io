---
layout: post
title: App Engine austerity - The composite key
date: '2011-09-08T10:58:00+01:00'
tags:
- python
- google app engine
- austerity
---
As anyone using App Engine knows, the age of austerity is almost upon us. Terrifying for a lot of people that have built their business on it. See [Brandon Wirtz](https://groups.google.com/forum/#!topic/google-appengine/dpCFQ3V8BQc) and [many others](https://groups.google.com/d/topic/google-appengine/obfGjbIkOTI/overview) reaction to the massive price increases. I seem to remember [Ikai Lan](https://plus.google.com/107011265359512082824), App Engine team member, predicting a year ago that if anything the price would go down in the future; a pretty safe prediction without hindsight.

![Great Depression](http://2.bp.blogspot.com/-2bca6DfjA3o/TmBVPmiV3BI/AAAAAAAAAZc/0LNKu2YD2Ho/s1600/great_depression%25202.jpg)

Google has provided us with some [tips](http://code.google.com/appengine/articles/managing-resources.html) to help us optimise our use of App Engine. I thought I would provide a technique here of using a composite key to reduce the cost of datastore operations and latency by only using an entity's key name to store key-value pairs.

## Problem

I have a sequence of events that I want to store in my GAE datastore. Each event consists of a unique timestamp and a small payload of *fixed size*. My app selects 50 - 60 of these events at a time from the datastore in chronological order.

What are my options for least latency and cost using GAE?

### Option 1 - Bad:

For each event create an entity in the datastore with a timestamp and payload:

{% highlight python %}
# Entity model
class Event(db.Model):    
  timestamp = db.DateTimeProperty()    
  payload = db.StringProperty(indexed=False)
  
# Query example where I want all entities with timestamps > 100000
query = Event.all().order('timestamp').filter('timestamp >', datetime.now())
{% endhighlight %}

This is probably the most intuitive way. However according to datastore best practices, it is best to perform your gets/queries via entity key if possible.

### Option 2 - Good:

Use the timestamp as the entity key. This should increase the query speed as GAE is optimised for searching by keys. To make this work I will need to change the timestamp from the datetime property type to a str or long. In doing so I will need to select my timestamp precision and millisecond precision is more than good enough for my use case to avoid duplicate keys:

{% highlight python %}
# Entity model
class Event(db.Model):    
  payload = db.StringProperty(indexed=False)
  
# Query example where I want all entities with timestamps > 100000
query_key = db.Key.from_path(Event.kind(), '100000')
query = Event.all() \
  .order('timestamp').filter('__key__ >', query_key)
{% endhighlight %}

The advantages of this method are that I directly query the key and the only index I need is the default key index.

### Option 3 - Great:

As we can make the payload fixed width, why not store the entire thing in a key? All we would then need to do is fetch the entities key and decode to get all the information we need:

{% highlight python %}
# Entity model - no longer requires a body
class Event(db.Model):
  pass
    
# assume our payload is always 50 characters long
def encode_key_name(timestamp, payload):
  timestamp = str(timestamp).zfill(10)    
  payload = str(payload).zfill(50)        
  return '%s|%s' % (timestamp, payload)
  
# Query example where I want all entities with timestamps > 100000
query_key = db.Key.from_path(Event.kind(), \                             
  encode_key_name(1000000, ''.zfill(50))
query = Event.all(keys_only=True) \                
  .order('timestamp').filter('__key__ >=', query_key)
{% endhighlight %}

Now we only need to do a keys_only query which is much cheaper and faster than pulling the whole entity from the datastore.

Caveats:

* It's your responsibility to make sure your data is always a fixed size.
* It might be tricky to perform >= queries instead of > queries depending on the ASCII/UNICODE character precedence.
* Scalability - I group sequences of events under different parents therefore allowing me to know each users sequence of events will not collide with other users.

When I get around to it I'll create some stats to get objective evidence that this really is a useful technique. However, subjectively this works well.