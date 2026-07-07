---
title: MQTT broker
description: An on-site MQTT broker, for all of your real-time data
---

[MQTT](https://en.wikipedia.org/wiki/MQTT) is a pub-sub protocol for real-time
data, popular in IoT and embedded scenarios because of how lightweight it is.

In MQTT, a "broker" is basically a server that you connect to, in order to read (subscribe) or write (publish) data on named _topics_.  A topic has a name (such as `emf/ducks`)and a set of clients which are subscribed to it at any point in time.

We would love to see what you can share with the field over MQTT.  Everything is readable by everyone, so prepare to embrace the openness but don't share anything private!

MQTT doesn't place any restrictions on what you send in payloads to a given
topic.  It could be UTF-8 text, JSON, or a binary message!  Try to publish your
data in a format that's relatively easy to decode, though :)

## Server connection details

Hostname: `mqtt.emf.camp`

Ports available:
  - 1883, no TLS (plaintext)
  - 8883, with TLS (encrypted)

You don't need to authenticate, see below.

## Topics and permissions

Anyone can **subscribe** to **any topic**.  You could subscribe to everything if you
wanted!

Without authenticating with a username/password, you can **publish** to topics
beginning with `open/`.  (This is a change from pre-2026 where anything except
`emf/...` was publishable anonymously.)

Some event infrastructure is publishing to MQTT too, using other topic prefixes
where publishing (but not subscribing) is protected with usernames & passwords.
We use these to help run the event!  There are also a number of installations
and games doing cool things on MQTT.

### Known topic namespaces

Check out topics under these prefixes, which might have something fun to look at:

- `bar/` -- live stats from the bars at the Robot Arms and NullSector.  See the [Bar page](/bar/) for the full rundown of what's published here.  
  TL;DR: anything published over the bar system's websocket API can also be found on MQTT.
<!-- - `films/` -- EMF Films will be publishing "an unnecessary level of info" under this prefix. -->
- `weather/hq` -- live weather data from HQ.  Full details on the [Weather](/weather/) page.

### Tips on topic naming, and wildcard subscriptions

Topics are best named hierarchically, because MQTT supports subscribing to entire groups of topics at once using _wildcards_.

For example, you could subscribe to all the bar data with one
subscription to `bar/#`. The `#` wildcard works much like how an asterisk
-- \* -- works outside of MQTT, like in typical filename wildcards, but it can
only appear at the end of a subscription.

There is also a `+` wildcard, which works at a single topic 'level', that is, a
bit of a topic name between slashes.  For example, subscribing to
`fauna/+/count` would subscribe to both `fauna/ducks/count` and
`fauna/spiders/count` -- if those topics existed ;)

Here is some further reading material on MQTT topic names and wildcard subscriptions: <https://www.hivemq.com/blog/mqtt-essentials-part-5-mqtt-topics-best-practices/>
