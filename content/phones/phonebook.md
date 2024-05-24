---
title: Phonebook JSON
description: Fetch the public phonebook over HTTP
---

You can fetch a JSON representation of the [public phonebook][phonebook]
from this URL:  
<https://phones.emfcamp.org/api/phonebook/EMF2024>

Here is some example output, formatted prettily:

```js
[
  {
    "value": "1020",
    "label": "NOC",
    "typeofservice_id": "SNOM"
  },
  {
    "value": "9000",
    "label": "Bananaphone",
    "typeofservice_id": "SIP"
  },
  {
    "value": "3800",
    "label": "Joe Bloggs",
    "typeofservice_id": "DECT"
  }
]
```

Possible values for `typeofservice_id` are:

- `App` (routed to Jambonz)
- `DECT`
- `Group`
- `GSM`
- `POTS`
- `SIP`
- `SNOM` (orga desk phone)

[phonebook]: https://phones.emfcamp.org/
