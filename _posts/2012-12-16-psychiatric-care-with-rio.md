---
layout: post
title: Psychiatric care with RiO
date: '2012-12-16T07:22:00+00:00'
tags:
- NHS
- medicine
---
For the several months I have used an [Electronic Medical Record](http://en.wikipedia.org/wiki/Electronic_medical_record) (EMR) system called RiO at a Mental Health Trust I have been working at. It covers many sites over a large part of the city and is the largest roll out of an EMR I have used. The system has been running for approximately 5 years and has clinical encounter details of every single patient that has passed through the psychiatric unit since then.

From a technical point of view the system is a web app built using ASP. You authenticate with a Smart Card and PIN. The authentication requires a separate proprietary Java program to be run outside the browser. From what I understand, this authentication client means that the RiO is locked into using Windows. Looking at the HTML source I can see that the JavaScript and HTML is definitely not W3C compliant. From the frequent database connection timeouts and subsequent stack trace dumps onto the screen I can tell the front end is connected to a Microsoft SQL Server database.

There is no doubt that the system is of great benefit to the health care professionals and therefore patients. However I still feel there is a long way to go before the system becomes a pleasure to use. I thought I would document some of my findings as I have seen the same issues repeated on several different systems.

### Logging In Is Slow

From logging in to Windows to getting the main RiO screen to appear can take 5 minutes. We are all told to only use our own accounts to access computers and to logout as soon as we have finished. This means that I must login and out maybe 10 times a day. That's 50 minutes per day wasted waiting or trying to fill your time with something else useful.

### The App Is Slow

Navigation around the site is very slow. It takes several seconds for each page to load and frequent database connection timeouts occur. This can be infuriating if you have just typed a clinical entry and are submitting it. This is why a lot of the staff have taken to using Word to type their entries and then copy and paste it into RiO.

### Pointless Forms

The main bulk of patient data is displayed as a chronologically ordered list of free text entries very much like a blog. However in an attempt to normalise some of this data there are several forms that need to be completed for various aspect is of the patient care. For example physical examination findings, risk profile and care plan meetings all have a separate set of forms to be completed. The problem with these forms is that they become out of date very quickly as clinical requirements and policies change leavening a large set of redundant mandatory fields to be completed. We mostly just put a full stop in these fields to stop the JavaScript validation complaining.

### Timeline Overload

As I mentioned earlier, the main bulk of patient data is entered in the form of a free text area. The data is entered by doctors, nurses and allied health professionals such as occupational therapists. The data is displayed in chronological order. If an inpatient has been resident for several weeks the timeline can become swamped with clinically useless information that makes it very difficult to find clinically important data. If a nurse sees a patient five times a day, they are obliged to note this each time on the timeline although the patient's condition may not have changed at all. I want this timeline to only highlight change, not stasis which is mostly the case.

### No Site Search

There are several ways to capture data in RiO, the patient encounter timeline and the various partially redundant forms that I discussed. The crevices of where data can be viewed/stored are numerous and easily missed. However RiO does not allow you to search the entire site so crucial information can easily go unseen.

### Summary

RiO is definitely an improvement over paper medical records. However it suffers from lack of ongoing maintenance and refinement meaning that is is painfully inefficient. It is clear just from looking under the surface at the HTML source that the people that coded it were not passionate or take much pride in coding it. I just wish vendor lock in was not so great to allow healthy competition as it is ultimately patients that suffer from inefficient clinical tools.