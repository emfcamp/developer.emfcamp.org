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

# Films

Notes Currently mocked data. Supports `?filter=today` or `?filter=future`

## Endpoint
[https://emffilms.org/api/2022/schedule](https://emffilms.org/api/2022/schedule) 

## Example
```json
{
    "schemaVersion": 1,
    "films": [
        {
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
                "oneLine": "Nam tristique tortor eu pede."
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