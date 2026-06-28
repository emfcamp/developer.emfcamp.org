---
title: Talks Schedule APIs
linkTitle: Talks schedule
description: >
  Endpoints for the full talk schedule, upcoming films at Stage C, and
  current & next talks on each stage.
---

## Full schedule

### Endpoints

#### JSON
[https://www.emfcamp.org/schedule/2026.json](https://www.emfcamp.org/schedule/2026.json)

### Filters

You can use any of the standard schedule filters:

* By stage e.g. `?venue=Stage+A` or `?venue=Stage+A&venue=Stage+B&venue=Stage+C`
* By 'favourited' status, if logged in e.g. `?is_favourite=True`
* By content type e.g. `?type=performance` or `?type=talk&type=workshop`

##### Example

```json
[
  {
    "id": 70,
    "type": "talk",
    "names": "Kliment",
    "pronouns": "he/him",
    "title": "Design decisions behind the Tildagon - a technical walkthrough",
    "description": "A few years back we came up with the concept of a reusable event badge, and built the Tildagon. I'd like to tell you how it came to be, and walk you through the electronic, mechanical, and visual design for 2024 and 2026 and the reasoning behind the various decisions we had to make along the way. We'll go through the schematic, layout, and part selection, and hopefully some of the weirder decisions will make sense.\r\n\r\nWe'll also talk about naming things, off-by-one errors, DHL being awful, and ketchup.",
    "short_description": "We'll go through the Tildagon's electronic, mechanical, and visual design and explain why it ended up as it is",
    "video_privacy": "public",
    "is_fave": false,
    "official_content": true,
    "slug": "design-decisions-behind-the-tildagon-a-technical-walkthrough",
    "link": "http://www.emfcamp.org/schedule/2026/70-design-decisions-behind-the-tildagon-a-technical-walkthrough",
    "occurrences": [
      {
        "occurrence_num": 1,
        "start_date": "2026-07-17 10:00:00",
        "start_time": "10:00",
        "end_date": "2026-07-17 11:00:00",
        "end_time": "11:00",
        "venue": "Stage X",
        "latlon": [
          52.043008,
          -2.377768
        ],
        "map_link": "https://map.emfcamp.org/#18.5/52.043008/-2.377768/m=52.043008,-2.377768",
        "uses_lottery": false,
        "video_privacy": "public",
        "recording_lost": false
      }
    ],
    "family_friendly": false,
    "content_note": ""
  },
  ...
]
```

#### iCal

There's also an [iCalendar version](https://en.wikipedia.org/wiki/ICalendar) of the main feed.

[https://www.emfcamp.org/schedule/2026.ical](https://www.emfcamp.org/schedule/2026.ical)

Note that this won't be active until the schedule is published.

##### Example

```
BEGIN:VCALENDAR
VERSION:2.0
SUMMARY:EMF 2026
X-WR-CALDESC:EMF 2026
X-WR-CALNAME:EMF 2026
BEGIN:VEVENT
SUMMARY:Design decisions behind the Tildagon - a technical walkthrough
DTSTART;TZID=Europe/London:20260717T100000
DTEND;TZID=Europe/London:20260717T110000
UID:2026-content-70-1
DESCRIPTION:A few years back we came up with the concept of a reusable eve
 nt badge\, and built the Tildagon. I'd like to tell you how it came to be\
 , and walk you through the electronic\, mechanical\, and visual design for
  2024 and 2026 and the reasoning behind the various decisions we had to ma
 ke along the way. We'll go through the schematic\, layout\, and part selec
 tion\, and hopefully some of the weirder decisions will make sense.\n\nWe'
 ll also talk about naming things\, off-by-one errors\, DHL being awful\, a
 nd ketchup.\n\nLink: https://www.emfcamp.org/schedule/2026/70-design-decis
 ions-behind-the-tildagon-a-technical-walkthrough\nVenue: Stage X (https://
 map.emfcamp.org/#18.5/52.043008/-2.377768/m=52.043008\,-2.377768)
LOCATION:Stage X
END:VEVENT
BEGIN:VEVENT
... (more events here)
END:VEVENT
END:VCALENDAR

```


## Now and Next

To find out what talks are happening by stage now or soon, a now/next endpoint is available, which saves on parsing the entire schedule.

### Endpoint

[https://emfcamp.org/schedule/now-and-next.json](https://emfcamp.org/schedule/now-and-next.json)

### Filters

You can use any of the standard schedule filters:

* By stage e.g. `?venue=Stage+A` or `?venue=Stage+A&venue=Stage+B&venue=Stage+C`
* By 'favourited' status, if logged in e.g. `?is_favourite=True`
* By content type e.g. `?type=performance` or `?type=talk&type=workshop`

### Example

```json
{
  "stage-a": [
    {
      "id": 24,
      "type": "talk",
      "names": "Howard Anderson",
      "pronouns": "he/him",
      "title": "Fugiat doloribus repellat exercitationem qui asperiores",
      "description": "Sequi dolore id vero. Sint quam accusamus similique eligendi repellendus. Nulla aliquid quasi dolore.\nExercitationem facere nulla aspernatur dolorum occaecati.\nDicta voluptate fugit nulla. Aut eaque temporibus placeat dignissimos.\nDolore odit aperiam illo enim illum. Nulla perspiciatis eius voluptatibus. Ex et molestias praesentium. Fuga ex iusto repellendus.\nRecusandae voluptatibus facere occaecati aut. Dicta et earum ratione eligendi.",
      "short_description": "Consequatur dignissimos sunt vitae provident amet a harum ipsum porro itaque tempore reiciendis nesciunt nesciunt reiciendis esse accusantium quod.",
      "video_privacy": "public",
      "is_fave": false,
      "official_content": true,
      "slug": "fugiat-doloribus-repellat-exercitationem-qui-asperiores",
      "link": "https://www.emfcamp.org/schedule/2026/24-fugiat-doloribus-repellat-exercitationem-qui-asperiores",
      "occurrences": [
        {
          "occurrence_num": 2,
          "start_date": "2026-07-17 11:00:00",
          "end_date": "2026-07-17 11:50:00",
          "venue": "Stage A",
          "latlon": [
            52.03961,
            -2.37787
          ],
          "map_link": "https://map.emfcamp.org/#18.5/52.03961/-2.37787/m=52.03961,-2.37787",
          "uses_lottery": false,
          "video_privacy": "public",
          "recording_lost": false,
          "start_time": "11:00",
          "end_time": "11:50"
        }
      ],
      "family_friendly": true,
      "content_note": "Doloremque occaecati ducimus vero voluptatem a consequatur."
    },
    {
      "id": 12,
      "type": "talk",
      "names": "Beth Fisher-McDonald",
      "pronouns": "she/her",
      "title": "Aliquid impedit nemo aperiam",
      "description": "Debitis laborum aliquam. Autem dolorem iusto porro sit. Quasi corrupti vel. Quia debitis atque dignissimos error.\nNatus iste adipisci ipsam consequatur ex. Molestias aperiam aut vitae.\nMinima accusantium eveniet commodi. Perspiciatis praesentium reprehenderit optio dolores error voluptate. Autem voluptatem aperiam omnis sequi.\nUllam impedit maiores. Voluptatem consequuntur culpa. Explicabo quasi corporis blanditiis cupiditate.",
      "short_description": "Aspernatur tenetur sequi corrupti voluptatum quam distinctio quod sequi sequi voluptatum molestiae placeat labore laudantium illo ratione vitae iure possimus.",
      "video_privacy": "public",
      "is_fave": false,
      "official_content": true,
      "slug": "aliquid-impedit-nemo-aperiam",
      "link": "https://www.emfcamp.org/schedule/2026/12-aliquid-impedit-nemo-aperiam",
      "occurrences": [
        {
          "occurrence_num": 0,
          "start_date": "2026-07-17 12:00:00",
          "end_date": "2026-07-17 12:50:00",
          "venue": "Stage A",
          "latlon": [
            52.03961,
            -2.37787
          ],
          "map_link": "https://map.emfcamp.org/#18.5/52.03961/-2.37787/m=52.03961,-2.37787",
          "uses_lottery": false,
          "video_privacy": "public",
          "recording_lost": false,
          "start_time": "12:00",
          "end_time": "12:50"
        }
      ],
      "family_friendly": false,
      "content_note": "Aspernatur alias modi neque accusamus excepturi perferendis eligendi hic."
    }
  ],
  "stage-b": [...
  ],
  "stage-c": [...
  ]
}
```

## Films

Details of films showing in Stage C every night.

### Endpoint

[https://emffilms.org/schedule.json](https://emffilms.org/schedule.json)

### Example

```json
{
    "schemaVersion": 1,
    "films": [
        {
            "display": true,
            "slug": "quam",
            "title": "Quam",
            "director": "Nettle Writtle",
            "year": 1997,
            "certificate": "18",
            "poster": "https://dummyimage.com/620x864.png/5fa2dd/ffffff",
            "image": "https://dummyimage.com/620x864.png/5fa2dd/ffffff",
            "starring": [
                "Aileen Mattusevich",
                "Deane Borrie",
                "Susan Algie",
                "Tamqrah Willans",
                "Annabell Sambidge"
            ],
            "precis": {
                "full": "In sagittis dui vel nisl. Duis ac nibh. Fusce lacus purus, aliquet at, feugiat non, pretium quis, lectus.",
                "oneLine": "Nam tristique tortor eu pede.",
                "special": "If this node exists, this has a special event associated with it"
            },
            "tagline": "engage B2C initiatives",
            "showing": {
                "timestamp": 1654189200000,
                "text": "02 June 18:00",
                "day": "Thursday"
            },
            "imdb": "https://weather.com/rhoncus/aliquam/lacus/morbi/quis.aspx",
            "trailer": "http://scribd.com/mattis/pulvinar/nulla/pede/ullamcorper/augue/a.xml",
            "runtime": {
                "minutes": 102,
                "text": "1h 23"
            }
        },
        ...
    ]
}
```
