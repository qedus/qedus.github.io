---
layout: post
title: Accessing a security enabled App Engine dev server using
  Apache HttpClient
date: '2012-11-11T20:59:00+00:00'
tags:
- java
- google app engine
---
[Martin Krasser](http://krasserm.blogspot.co.uk/2010/01/accessing-security-enabled-google-app.html) has an article on how to programatically login your Java client on both production and development Google App Engine servers. It's a couple of years old now and the development server section advice no longer works. Here's how to make it work again with Apache HttpClient:

{% highlight java %}
HttpPost httpPost = new HttpPost("http://localhost:8080/_ah/login");
httpPost.setHeader("Content-Type", "application/x-www-form-urlencoded");
 
List <NameValuePair> parameters = new ArrayList <NameValuePair>();
parameters.add(new BasicNameValuePair("email", "test@example.com"));
parameters.add(new BasicNameValuePair("continue", "http://localhost:8080/"));
parameters.add(new BasicNameValuePair("admin", "False"));
parameters.add(new BasicNameValuePair("action", "Login"));
 
UrlEncodedFormEntity entity = new UrlEncodedFormEntity(parameters, HTTP.UTF_8);
httpPost.setEntity(entity);
HttpClient httpClient = new DefaultHttpClient();
httpClient.getParams().setBooleanParameter(ClientPNames.HANDLE_REDIRECTS, false);
HttpResponse resp = httpClient.execute(httpPost);
{% endhighlight %}

Now can use that httpClient to access any protected resources on your development server. Change the *admin* value from *False* to *True* if you need admin access. Make sure to change the port (8080) to whatever port your development server is running on. Note that I turn *HANDLE_REDIRECTS* to *false* as in my case I do not want to be redirected to http://localhost:8080/ when I submit the form; you may want to.
