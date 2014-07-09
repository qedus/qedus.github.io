---
layout: post
title: NHS as a PaaS
date: '2012-06-24T15:37:00+01:00'
tags:
- technology
- surgery
- NHS
- medicine
---

![Cloud Stack](/images/cloud-stack.gif)

The NHS is a massive organisation with disparate patient services and IT infrastructure. As a result of this disparity islands of patient data build up so a clinician on one island never gets to see a patient's data generated on another. This is often detrimental to the patient and wastes NHS money and resources in duplication of effort when an investigation on one island is duplicated on another.

What is the solution then? Every island that stores patient data must be connected. A massive connected system between all primary, secondary and tertiary institutions? That was tried and failed miserably. The NHS 'Spine' is the result of this which currently is mainly used to store demographic data on patients. In practice it serves very little purpose other than attempting to find out a patient's unique NHS ID. What's more, to access the service you need a smart card and from what I understand the software required to authenticate the smart card only works on windows and the printers for the specific NHS smart cards are no longer made; the current ones are nearing the end of their life.

Why doesn't the NHS IT create a [PaaS](http://en.wikipedia.org/wiki/Platform_as_a_service)? The platform's most important feature would be acting as a repository for patient data. Data like blood tests, imaging, demographics, medications, appointments, diagnoses etc could all be stored in a repository with well defined APIs used to access them. This is much more manageable. It also would lower the barrier for companies to build on top of it and produce domain specific interfaces and analytics for each island. Products built on top of this platform would be contractually required to save the fundamental data on the NHS platform's datastore for others to use.

The system would be analogous to Network Rail maintaining our railway lines with many other companies providing a service on top of it. If First Great Western goes bust, who cares, another company can swoop in and take over - the infrastructure still remains.

New NHS policy seems to be going this way, let's hope something good happens soon.