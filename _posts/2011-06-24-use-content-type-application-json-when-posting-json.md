---
layout: post
title: Use Content-Type "application/json" when posting JSON to Google App Engine
date: '2011-06-24T15:58:00+01:00'
tags:
- objective-c
- json
- google app engine
---
If you are sending a [JSON](http://www.json.org/) POST body to App Engine, make sure you set the *Content-Type* request header to *application/json* otherwise the post body will not be decoded correctly on the Google App Engine side.

Using [ASIHTTHttpRequest](http://allseeing-i.com/ASIHTTPRequest/How-to-use):

{% highlight objective-c %}
NSURL *url = [NSURL URLWithString:@"http://example.com/"];
ASIHTTPRequest *request = [ASIHTTPRequest requestWithURL:url];
[request addRequestHeader:@"Content-Type" value:@"application/json"];
[request appendPostData:[@"{\"count\" : 49}" dataUsingEncoding:NSUTF8StringEncoding]];
{% endhighlight %}