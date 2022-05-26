# Talks

No data in this yet, it will be formatted like [https://www.emfcamp.org/schedule/2018.json](https://www.emfcamp.org/schedule/2018.json)

## Endpoint
[https://www.emfcamp.org/schedule/2022.json](https://www.emfcamp.org/schedule/2022.json)

## Example
``` json
[
  {
    "id": 309,
    "source": "database",
    "start_date": "2018-09-02 19:20:00",
    "start_time": "19:20",
    "slug": "braaaaaaains-archaeology-and-philosophy-of-zombies-in-games",
    "is_fave": false,
    "venue": "Stage A",
    "title": "BRAAAAAAAINS: Archaeology and Philosophy of Zombies in Games",
    "description": "These are not the zombies you've fragged! A bioarchaeologist and an applied philosopher will take you on a historical zombie adventure beyond Romero's shambling hordes and stories about Haitian Voodoo. Did you know that the D&D wondrous item: Hand of Glory was inspired by real artefacts? Or that miasmic \"zombification\" in some video games reflects medieval ideas about contagion? What do past ideas about zombies tell us about zombies now? And how do video game zombies reflect current ideas about ethics? Also, find out how zombies play a role in cutting-edge neuroscience and ICT research!",
    "type": "talk",
    "link": "http://www.emfcamp.org/line-up/2018/309",
    "user_id": 1285,
    "map_link": "https://map.emfcamp.org/#18.5/52.0396099/-2.377866",
    "may_record": true,
    "speaker": "Dr. Tyr Fothergill",
    "end_time": "20:00",
    "end_date": "2018-09-02 20:00:00",
    "latlon": [
      52.0396099,
      -2.377866
    ],
    "video": {
      "ccc": "https://media.ccc.de/v/emf2018-309-braaaaaaains-archaeology-and-philosophy-of-zombies-in-games",
      "youtube": "https://www.youtube.com/watch?v=N9_z6Zp5waY",
      "preview_image": "https://static.media.ccc.de/media/events/emf/2018/309-hd_preview.jpg"
    }
  },
  ...
]
```

# Now and Next

To find out what talks are happening by stage now or soon a now/next endpoint is available. Filters by stage are also supported e.g. `?venue=Stage+A`.

## Endpoint
[https://emfcamp.org/schedule/now-and-next.json](https://emfcamp.org/schedule/now-and-next.json)

## Filters


### Example

``` json
{
  "stage-a": [
    {
      "id": 143,
      "slug": "iure-dolore-aperiam-expedita-cum-magnam-quidem",
      "start_date": "2022-06-03 10:00:00",
      "end_date": "2022-06-03 10:30:00",
      "venue": "Stage A",
      "latlon": [
        52.03961,
        52.03961
      ],
      "map_link": "https://map.emfcamp.org/#18.5/52.03961/52.03961",
      "title": "Iure dolore aperiam expedita cum magnam quidem.",
      "speaker": "Graeme Adams",
      "pronouns": null,
      "user_id": 142,
      "description": "Adipisci mollitia aliquid qui. Nam quae vero blanditiis minima nulla temporibus rem.\nQuod adipisci modi in autem porro architecto. Est odio sapiente soluta ea dolores.\nUllam nisi perspiciatis praesentium tempora eum perspiciatis. Fuga ut non dolore eum odio.\nCupiditate recusandae dolores molestiae accusantium reiciendis voluptates. Incidunt beatae similique nemo non.\nVitae sequi in soluta fugiat. Tempora maxime neque nostrum. Ea ipsa qui nulla cupiditate. Fuga iste quis autem omnis.",
      "type": "talk",
      "may_record": null,
      "is_fave": false,
      "source": "database",
      "link": "http://localhost:2342/line-up?year=2022&slug=iure-dolore-aperiam-expedita-cum-magnam-quidem&proposal_id=143",
      "start_time": "10:00",
      "end_time": "10:30"
    },
    {
      "id": 112,
      "slug": "quos-recusandae-asperiores-ab",
      "start_date": "2022-06-03 12:00:00",
      "end_date": "2022-06-03 12:50:00",
      "venue": "Stage A",
      "latlon": [
        52.03961,
        52.03961
      ],
      "map_link": "https://map.emfcamp.org/#18.5/52.03961/52.03961",
      "title": "Quos recusandae asperiores ab.",
      "speaker": "William Davies",
      "pronouns": null,
      "user_id": 111,
      "description": "Accusamus facilis earum reiciendis. Laborum fugiat perferendis tempora dolore eius. Neque sunt mollitia in debitis ex deleniti.\nAsperiores quis repellat in fugit voluptatibus. Non id sequi laboriosam perferendis eligendi dicta. Laboriosam tempore temporibus tempora ab itaque.\nAnimi suscipit dolor maxime modi nihil praesentium. Voluptatem consequatur eius debitis nulla quisquam ad. Tempora suscipit id dolores consequatur eveniet autem.",
      "type": "talk",
      "may_record": null,
      "is_fave": false,
      "source": "database",
      "link": "http://localhost:2342/line-up?year=2022&slug=quos-recusandae-asperiores-ab&proposal_id=112",
      "start_time": "12:00",
      "end_time": "12:50"
    }
  ],
  "stage-b": [...
  ],
  "stage-c": [...
  ]
}
```

# Films

Details of films showing in Stage C every night. Supports `?filter=today` or `?filter=future` on the query string for specific results. Use `films[*].image` to get a relative link to a small-ish portrait jpg from the site.

## Endpoint
[https://emffilms.org/api/2022/schedule](https://emffilms.org/api/2022/schedule) 

## Example
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
