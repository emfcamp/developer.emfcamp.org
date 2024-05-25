---
title: MQTT broker
description: An on-site MQTT broker, for all of your real-time data
---

[MQTT](https://en.wikipedia.org/wiki/MQTT) is a pub-sub protocol for real-time
data, popular in IoT and embedded scenarios because of how lightweight it is.

In MQTT, a "broker" is basically a server that you connect to, in order to read (subscribe) or write (publish) data on named _topics_.  A topic has a name (such as `emf/ducks`)and a set of clients which are subscribed to it at any point in time.

After running a slightly undersold MQTT broker in 2022, the Networks team (NOC) are making more of an effort in 2024.  We would love to see what you can share with the field over MQTT.

MQTT doesn't place any restrictions on what you send in payloads to a given
topic.  It could be UTF-8 text, JSON, or a binary message!  Make sure you
publish your data in a format that's relatively easy to consume, though :)

## Server connection details

Hostname: `mqtt.emf.camp`

Ports available:
  - 1883, no TLS (plaintext)
  - 8883, with TLS (encrypted)

You don't need to authenticate.

## Topics and permissions

Unauthenticated clients can publish (write) and subscribe (read) on any topic
name, _except_ topics beginning with `emf/`, which are reserved for specific
event purposes.

We use the topics under `emf/` to help run the event!  That said, we also want
to share all the data we can.  Unauthenticated clients can subscribe to any of
the `emf/` topics, but we just have to stop the world from being able to inject
dodgy data in.

### Known topic namespaces

Check out topics with these prefixes, which might have something fun to look at:

- `emf/bar/` -- live stats from the Robot Arms and Null Sector's Cybar.  See the [Bar page](/bar/) for the full rundown of what's published here.  
  TL;DR: anything available over the websocket API is also going to be on MQTT.
- `emf/films/` -- EMF Films will be publishing "an unnecessary level of info" under here.

### Tips on topic naming, and wildcard subscriptions

Topics are best named hierarchically, because MQTT supports subscribing to entire groups of topics at once using _wildcards_.

For example, you could subscribe to all the EMF Films data with one
subscription to `emf/films/#`. The `#` wildcard works much like how an asterisk
-- \* -- works outside of MQTT, like in typical filename wildcards, but it can
only appear at the end of a subscription.

There is also a `+` wildcard, which works at a single topic 'level', that is, a
bit of a topic name between slashes.  For example, subscribing to
`emf/fauna/+/count` would subscribe to both `emf/fauna/ducks/count` and
`emf/fauna/spiders/count` -- if those topics existed ;)

Here is some further reading on MQTT topic names and wildcard subscriptions: <https://www.hivemq.com/blog/mqtt-essentials-part-5-mqtt-topics-best-practices/>
