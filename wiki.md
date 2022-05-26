# Wiki (semantic-mediawiki)
The EMF Wiki ([https://wiki.emfcamp.org/wiki/](https://wiki.emfcamp.org/wiki/)) runs MediaWiki (which has its [own API](https://www.mediawiki.org/wiki/API:Main_page)), with the [Semantic MediaWiki](https://www.semantic-mediawiki.org/wiki/Semantic_MediaWiki) extension.

Semantic MediaWiki's API allows for structured data queries, and is [well documented](https://www.semantic-mediawiki.org/wiki/Help:API).

## Endpoint
 - `https://wiki.emfcamp.org/w/api.php`

## Method
 - `POST`

## Example
 - `/w/api.php?action=browsebysubject&format=json&subject=User%3ATheresNoTime`

```json
{
    "query": {
        "subject": "TheresNoTime#2##",
        "data": [
            {
                "property": "AttendeePagename",
                "dataitem": [
                    {
                        "type": 9,
                        "item": "TheresNoTime#0##"
                    }
                ]
            },
            {
                "property": "Attendee_Name",
                "dataitem": [
                    {
                        "type": 9,
                        "item": "Sammy#0##"
                    }
                ]
            },
            {
                "property": "Attendee_twitter",
                "dataitem": [
                    {
                        "type": 2,
                        "item": "TheresNoTimeFor"
                    }
                ]
            },
            {
                "property": "IRC",
                "dataitem": []
            },
            {
                "property": "Login",
                "dataitem": [
                    {
                        "type": 9,
                        "item": "TheresNoTime#0##"
                    }
                ]
            },
            {
                "property": "Village",
                "dataitem": [
                    {
                        "type": 9,
                        "item": "Village:GFTTSTC#0##"
                    }
                ]
            },
            {
                "property": "Website",
                "dataitem": [
                    {
                        "type": 9,
                        "item": "Https://www.theresnotime.co.uk#0##"
                    }
                ]
            },
            {
                "property": "_INST",
                "dataitem": [
                    {
                        "type": 9,
                        "item": "Attendee#14##"
                    }
                ]
            },
            {
                "property": "_MDAT",
                "dataitem": [
                    {
                        "type": 6,
                        "item": "1/2022/5/21/10/1/11/0"
                    }
                ]
            },
            {
                "property": "_SKEY",
                "dataitem": [
                    {
                        "type": 2,
                        "item": "TheresNoTime"
                    }
                ]
            }
        ],
        "serializer": "SMW\\Serializers\\SemanticDataSerializer",
        "version": 2
    }
}
```
