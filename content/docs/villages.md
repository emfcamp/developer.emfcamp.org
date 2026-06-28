---
title: Villages API
description: >
  A straightforward API publishing simple information about EMF's villages.
---

They are available from [https://www.emfcamp.org/api](https://www.emfcamp.org/api).

## Endpoints

### Villages (get all)

Returns a list of all villages at EMF 2026 as a json object.

#### Endpoint
[https://www.emfcamp.org/api/villages](https://www.emfcamp.org/api/villages)


#### Example
```json
[
  {
    "id": 1,
    "name": "Cardiff Makerspace",
    "url": "https://www.emfcamp.org/villages/2026/1-cardiff-makerspace",
    "external_url": "https://cardiffmakerspace.com/",
    "description": "Cardiff Makerspace is a shared workspace in Cardiff for people who like to learn and make stuff. A bunch of us will be at EMF, so come along and say hi to our lovely group of people! Our members interests include electronics, robotics, woodwork, metalwork, photography, music, jewellery making, paper craft and textiles.",
    "location": null,
    "num_members": 2
  },
  {
    "id": 3,
    "name": "rLab - Reading Hackspace",
    "url": "https://www.emfcamp.org/villages/2026/3-rlab-reading-hackspace",
    "external_url": "https://wiki.rlab.org.uk",
    "description": "rLab is Reading's hackspace. Founded in 2010 and moving into our current building in 2014, we're one of the longest running spaces in the UK. \r\n\r\nWe'll be doing all manner of making and hacking during EMF and we'll be keen to show off what we've been working on and even run some demos! ",
    "location": {
      "type": "Point",
      "coordinates": [
        -2.3759886362119005,
        52.041527200927334
      ]
    },
    "num_members": 6
  }, 
  ...
]
```

### Village (get one)
Returns a single village's information as json if passed a valid id.

#### Endpoint
[https://www.emfcamp.org/api/villages/[id]](https://www.emfcamp.org/api/villages[id])

#### Example
```json
{
  "id": 1,
  "name": "Cardiff Makerspace",
  "url": "https://www.emfcamp.org/villages/2026/1-cardiff-makerspace",
  "external_url": "https://cardiffmakerspace.com/",
  "description": "Cardiff Makerspace is a shared workspace in Cardiff for people who like to learn and make stuff. A bunch of us will be at EMF, so come along and say hi to our lovely group of people! Our members interests include electronics, robotics, woodwork, metalwork, photography, music, jewellery making, paper craft and textiles.",
  "location": null,
  "num_members": 2
}
```

### My Village

If your EMF account is a member of a village you are logged in you can return your village's information as json.

#### Endpoint 
[https://www.emfcamp.org/api/villages/mine](https://www.emfcamp.org/api/villages/mine)

#### Example
``` json
[
  {
    "id": 1,
    "name": "Cardiff Makerspace",
    "url": "https://www.emfcamp.org/villages/2026/1-cardiff-makerspace",
    "external_url": "https://cardiffmakerspace.com/",
    "description": "Cardiff Makerspace is a shared workspace in Cardiff for people who like to learn and make stuff. A bunch of us will be at EMF, so come along and say hi to our lovely group of people! Our members interests include electronics, robotics, woodwork, metalwork, photography, music, jewellery making, paper craft and textiles.",
    "location": null,
    "num_members": 2
  }
]
```
