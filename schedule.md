# Talks

## Endpoint

[https://www.emfcamp.org/schedule/2022.json](https://www.emfcamp.org/schedule/2022.json)

## Example

``` json
[
  {
    "id": 165,
    "slug": "hacking-train-tickets-for-fun-but-not-for-profit",
    "start_date": "2022-06-03 13:50:00",
    "end_date": "2022-06-03 14:20:00",
    "venue": "Stage A",
    "latlon": [
      52.03961,
      -2.37787
    ],
    "map_link": "https://map.emfcamp.org/#18.5/52.03961/-2.37787",
    "title": "Hacking train tickets for fun, but not for profit",
    "speaker": "Hugh Wells",
    "pronouns": "",
    "user_id": 25,
    "description": "We take a scenic tour through the origins of the UK train ticket, from the original BR specification in the 1980s through to modern replacements like mTickets, eTickets and ITSO. \r\n\r\nThis is just a detour though, and we'll focus on the 'orange ticket' (RSP 9399/9599) - which continues to be a stalwart of the UK rail network. Surely they can't be that secure? After all, anyone can encode a magstripe - right? \r\n\r\nWe'll take a look through the data encoded on these tickets, what interesting things you can do with them and maybe (assuming I've got it working by then) we'll be able to read and write our own! ",
    "type": "talk",
    "may_record": true,
    "is_fave": false,
    "is_family_friendly": false,
    "content_note": "",
    "source": "database",
    "link": "https://www.emfcamp.org/schedule/2022/165-hacking-train-tickets-for-fun-but-not-for-profit",
    "start_time": "13:50",
    "end_time": "14:20"
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

```json
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
