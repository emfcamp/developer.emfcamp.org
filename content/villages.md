---
title: Villages API
description: >
  A straightforward API publishing simple information about EMF's villages.
---

They are available from [https://www.emfcamp.org/api](https://www.emfcamp.org/api).

## Endpoints

### Villages (get all)

Returns a list of all villages at EMF 2022 as a json object.

#### Endpoint
[https://www.emfcamp.org/api/villages](https://www.emfcamp.org/api/villages)


#### Example
```json
[
  {
    "id": 1,
    "name": "ECS",
    "url": null,
    "description": "A village for the lovely people from the University of Southampton.\r\nMost of us are students/staff/alumni from ECS (Electronics and Computer Science), but all other faculties and alumni are welcome :) ",
    "location": null
  },
  {
    "id": 2,
    "name": "Termitown",
    "url": null,
    "description": "Villiage for the Plymouth University computing society alumni Slack.",
    "location": null
  }...
]
```

### Village (get one)
Returns a single village's information as json if passed a valid id.

#### Endpoint
[https://www.emfcamp.org/api/villages/[id]](https://www.emfcamp.org/api/villages[id])

#### Example
```json
[
  {
    "id": 1,
    "name": "ECS",
    "url": null,
    "description": "A village for the lovely people from the University of Southampton.\r\nMost of us are students/staff/alumni from ECS (Electronics and Computer Science), but all other faculties and alumni are welcome :) ",
    "location": null
  }
]
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
    "name": "ECS",
    "url": null,
    "description": "A village for the lovely people from the University of Southampton.\r\nMost of us are students/staff/alumni from ECS (Electronics and Computer Science), but all other faculties and alumni are welcome :) ",
    "location": null
  }
]
```
