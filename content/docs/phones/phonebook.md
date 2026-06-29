---
title: Phonebook
description: Fetch the public phonebook over HTTP
---

See https://phones.emfcamp.org/api for more info.

You can fetch a JSON representation of the public phonebook from this URL: <https://phones.emf.camp/phonebook.json>

```js
[
  {
    "number": 118,
    "mnemonic": null,
    "length": 3,
    "name": "Directory Enquiries",
    "service": "GROUP",
    "type": "SERVICE"
  },
  {
    "number": 123,
    "mnemonic": null,
    "length": 3,
    "name": "Talking Clock",
    "service": "PEER",
    "type": null
  },
  {
    "number": 150,
    "mnemonic": null,
    "length": 3,
    "name": "Information",
    "service": "PEER",
    "type": null
  }
]
```

Service type (`service`) can be:
- `DECT`
- `SIP`
- `DESK`
- `GSM`
- `POTS`
- `APP`
- `GROUP`
- `PEER`

Extension type (`type`) can be:
- `PERSON`
- `PUBLIC`
- `VILLAGE`
- `SERVICE`
- `FAX`
- `MODEM`
- `ORGA`
- `IMPORTANT`

[phonebook]: https://phones.emfcamp.org/
