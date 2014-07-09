---
layout: post
title: App Engine NDB gotcha PickleProperty default value
date: '2012-04-29T14:07:00+01:00'
tags:
- python
- ndb
- google app engine
---
App Engine's python NDB code is amazing; created by a guy who really knows how to use Python well. It's been really nice to follow its progress to see how a master developer works. Thank's Guido & Google.   I though I would post here to let you all know of a trap I fell into that reduces the usefulness of PickleProperty default parameter.

{% highlight python %}
class TestPickle(ndb.Model):
    test_value = ndb.PickleProperty(default={'hello': 'there'})
 
test_pickle = TestPickle()
self.assertEqual(test_pickle.test_value['hello'], 'there')
test_pickle.test_value['what'] = 'up'
test_pickle.put()
 
test_pickle_two = TestPickle()
# The line below fails
self.assertEqual(test_pickle_two.test_value.get('what'), None)
{% endhighlight %}

The last line fails as is expected; test_pickle_two takes on the key-value pair 'what': 'up'. However this fact means that I can only ever instantiate one test_value per web request if I don't want to pollute my default value.

The thing is that ndb.PickleProperty is very likely to hold a mutable python object as that's where it's useful.

I wonder if there is a simple way I could get a fresh mutable default value for ndb.PickleProperty each time I instantiate the class TestPickle?