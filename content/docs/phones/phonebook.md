---
title: Phonebook JSON
description: Fetch the public phonebook over HTTP
---

You can fetch a JSON representation of the [public phonebook][phonebook]
from this URL:  
<https://phones.emf.camp/phonebook.json>

Here is some example output, formatted prettily:

```js
[
  {
    "number":118,
    "mnemonic":null,
    "length":3,
    "name":"Directory Enquiries",
    "service":"GROUP",
    "type":"SERVICE"
  },
  {
    "number":1234,
    "mnemonic":null,
    "length":4,
    "name":"Conduct",
    "service":"SIP",
    "type":"ORGA"
  },
  {
    "number":2002,
    "mnemonic":null,
    "length":4,
    "name":"Bob Good (DECT)",
    "service":"DECT",
    "type":"PERSON"
  }
]
```

Possible values for `service` are:

- `DECT`
- `GROUP`
- `PEER`
- `GSM`
- `POTS`
- `SIP`
- `SNOM` (orga desk phone)

[phonebook]: https://phones.emfcamp.org/
