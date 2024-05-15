---
title: Phone system
description: >
  The POC has set up a platform for programmable telephony hacks, based on
  Jambonz
---

As part of the EMF Phone Operations Centre (POC) this year, we have a set of programmable
communications APIs that you can use to interact with the phone network and build your own
services.

This is based on the open source [jambonz](https://www.jambonz.org) platform, which has APIs for incoming
and outgoing calls similar to some of the other programmable communications platforms on the
market. The key thing is that this one can be called from the EMF site phone network.

You can find the details of the Jambonz API at [https://www.jambonz.org/docs/](https://www.jambonz.org/docs/)

There are a couple of points to note regarding our implementation:

- You can make a call TO a jambonz app but at present Apps can't call other site phones, we may get this fixed before EMF but no commitments, this applies to using the Dial verb or Originating a call.
- If you REALLY need to be able to send calls to phones then please come and talk to us nicely we might be able to help on a case by case basis.
- You won't see calls from phones in your jambonz portal call logs, this is due to the way we setup the system and too late to re-design for this year, if you need a trace talk to the Phone Team.
- SMS/SMPP/Messaging is not supported (might be at some point)
- We have provided AWS as a Speech to Text and Text to Speech provider for all accounts to use, but someone is paying for this out of their own pocket, so please don't start transcribing the whole talks feed!
- We reccomend not changing the password for your jambonz account in the jambonz console do this via https://phones.emfcamp.org instead under user profile, Create Jambonz Account. This page will overwrite any changes in jambonz.


To create a new application firstly login to https://phones.emfcamp.org

Goto the User profile and click on the Create Jambonz Account link.
This will setup a jambonz account with the same username but you can choose a different password here.

Once you have done that then you can safely create a new Number in the Add a Number link with a Type of Service of App.
The config param will auto fill your username, 

When you have created the number it will then be provisioned into Jambonz.

Login to https://jambonz.poc.emf.camp with the password you just created and in you should find the phone number listed in phone numbers, 

You can now create an application, with Calling & Call Status wehooks, then link your phone number to that application.

[Video walkthrough to come]
