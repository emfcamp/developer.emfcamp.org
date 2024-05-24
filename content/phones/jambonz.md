---
title: Jambonz
description: A programmable system for telephony apps and hacks
---

As part of the EMF Phone Operations Centre (POC), aka 'Phones team', we have a
set of programmable communications APIs that you can use to interact with the
phone network and build your own services.

This is based on the open source [Jambonz](https://www.jambonz.org) platform.
Jambonz has APIs for incoming and outgoing calls similar to some of the other
programmable communications platforms on the market.  The key difference: this
one can be called from the EMF site phone network!

You can find the details of the Jambonz API in their public docs at [https://www.jambonz.org/docs/](https://www.jambonz.org/docs/).

### Key details

Jambonz is up and running at <https://jambonz.poc.emf.camp/>.

To get your Jambonz account, first create/log into your account on the EMF
Phonebook at <https://phones.emfcamp.org/> -- full step-by-step instructions
are available below.

#### EMF-specific quirks

There are a couple of points to note regarding our implementation of Jambonz:

- You can make a call _to_ a Jambonz app, but at present apps can't call out to
  other site phones.  We may get this fixed before EMF but no commitments.
  (This point applies to using the Dial verb or Originating a call from
  Jambonz.)

  If you _really_ need to be able to send calls to phones then please come and
  talk to the Phones team and we might be able to help on a case-by-case basis.

- You won't see calls from phones in your Jambonz portal call logs, because of
  the way we originally set up the system -- unfortunately it's too late to re-design for 2024.  If
  you need a call trace, talk to the Phones team.

- SMS/SMPP/Messaging is not supported (might be at some point during the event).

- We have set up [Amazon Polly](https://aws.amazon.com/polly/) as a
  Speech-to-Text and Text-to-Speech provider for all accounts to use, but
  someone is paying for this out of their own pocket, so please don't start
  transcribing the whole talks feed!

- We recommend not changing your Jambonz account password in the Jambonz
  console!  Instead, go to your user profile on the [EMF Phonebook][phonebook]
  and use the 'Create Jambonz Account' feature.  This page will overwrite any
  changes in Jambonz.


### Step-by-step guides

#### Set up your Jambonz account

1.  Log in to <https://phones.emfcamp.org>
2.  Go to "\<your username\>'s profile" and click on the "Create Jambonz Account" link.

This will set up a Jambonz account with the same username as in the Phonebook, but you can choose a different password here.

With credentials in hand, you can go straight into creating your first Jambonz app!

#### Create a new phone number and Jambonz app

Before you create an app, make sure you have a Calling webhook and Call Status
webhook ready to go.  These are two HTTP or WebSocket endpoints that Jambonz
will hit with requests/messages when, respectively, a call first comes in and
when there are updates to send about it.

1. Jambonz apps each need a phone number registered in the [EMF Phonebook][phonebook].
   Go and [register a new number](https://phones.emfcamp.org/number/create), setting the
   Type of Service to 'App'.  The config param will auto-fill your Jambonz username.

2. When you have created the number it will then be provisioned into
   [Jambonz][jambonz], where you should find the phone number listed in the
   "Phone Numbers" section of BYO Services.

3. You can now create an application in the Jambonz web UI, under
   "Applications".  This is where you add details of the Calling & Call Status
   webhooks.

4.  Once you've made your app, return to the Phone Numbers menu, and edit and
    save your number to assign it to your new Jambonz app.

[Video walkthrough to come]

[jambonz]: https://jambonz.poc.emf.camp/
[phonebook]: https://phones.emfcamp.org/
