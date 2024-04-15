---
title: Bar data
linkTitle: Bar
description: >
  Opening times, live stock, prices and sales data for the Bar and Cybar.
---
* * *
This is a DRAFT and the interface may still change before the
event. Object IDs and other sample data in this document are from 2022
and WILL change before 2024.

Prior to the event, this interface is available for testing at
https://emftill.assorted.org.uk/ although we make no guarantees about
the accuracy of the data returned!
* * *

# 2024 Bars

There is a simple read-only interface to the till system running the
main bar and the Null Sector bar.  It's available via HTTP GET from
`bar.emf.camp` and returns JSON, except when you request an object
that does not exist when it will return HTTP 404.

Some types of object can be subscribed to over a websocket
connection. These objects have a property "key" that you can use to
identify them. You will receive a fresh copy of the object over the
websocket connection immediately after subscription, and then again
whenever the object is updated. The websocket is at
`wss://bar.emf.camp/websocket/`

[The web service that implements this API is here.](https://github.com/emfcamp/quicktill-tillweb)

## Data types

All these data types are represented as JSON objects. Integers are
represented as JSON numbers, Decimals are represented as JSON
strings, times are local times in ISO 8601 format.

### Error

Only received over a websocket connection. Sent when the websocket
server can't parse your request.

Properties:

* `type`: the string "error"
* `message`: a description of the problem

Example:

```json
{
  "type": "error",
  "message": "Invalid request"
}
```

### Not present

Only received over a websocket connection. Sent when the key you
subscribed to does not exist, or ceases to exist. The subscription
remains valid and if the key is created in the future you will receive
it.

Properties:

* `type`: the string "not present"
* `key`: the key that is not present

Example:

```json
{
  "type": "not present",
  "key": "stocktype/12345"
}
```

### Session

A time period that the bar is planned to be open.

Properties:

* `opening_time`: opening time
* `closing_time`: closing time

Example:

```json
{
  "opening_time": "2022-06-02T11:00:00",
  "closing_time": "2022-06-03T01:00:00"
}
```

### Department

Everything sold through the till has a "department", which is a
general classification by type (real ale, keg, wine, etc.)

Properties:

* `type`: the string "department"
* `id`: an integer uniquely identifying the department
* `description`: a string describing the department
* `notes`: an optional string giving more information

Example:

```json
{
  "type": "department",
  "id": 90,
  "description": "Wine (bottles)",
  "notes": "Available in 125ml, 175ml and 250ml"
}
```

### Stock type

Properties:

* `type`: the string "stocktype"
* `key`: the object's websocket key
* `id`: an integer uniquely identifying the stock type
* `department`: the Department object associated with the stock type
* `manufacturer`: a string naming the manufacturer
* `name`: a string describing this type of stock; unique per manufacturer
* `abv`: an optional Decimal giving the alcohol by volume
* `fullname`: manufacturer, name and abv combined into a single string
* `price`: the sale price as a Decimal, including VAT where appropriate, for a `sale_unit_name` of this type of stock
* `logo`: the path to a PNG logo, or null if not present
* `tasting_notes`: tasting notes as HTML, or null if not present
* `base_units_bought`: the amount of stock bought in base units as a Decimal
* `base_units_remaining`: the amount of stock remaining in base units as a Decimal
* `base_unit_name`: a string giving the unit in which we count how much of this stock we have bought and sold, eg. "pint" or "ml"
* `sale_unit_name`: a string giving the unit in which we sell this type of stock, eg. "bottle" or "pint"
* `sale_unit_name_plural`: plural form of `sale_unit_name`
* `base_units_per_sale_unit`: the number of base units per sale unit as a Decimal
* `stock_unit_name`: a string giving the unit in which we count this type of stock, eg. "pint" or "1l carton"
* `stock_unit_name_plural`: plural form of `stock_unit_name`
* `base_units_per_stock_unit`: the number of base units per stock unit as a Decimal

Examples:

```json
{
  "type": "stocktype",
  "key": "stocktype/1",
  "id": 1,
  "department": {
    "type": "department",
    "id": 35,
    "description": "Keg cider",
    "notes": ""
  },
  "manufacturer": "Westons",
  "name": "Stowford Press",
  "abv": "4.5",
  "fullname": "Westons Stowford Press (4.5% ABV)",
  "price": "4.00",
  "logo": "/media/emf/product-logos/cea539af1f7ebb2929572458355bf6426f83c96dbf243a69e340a70b43935c54.png",
  "tasting_notes": null,
  "base_units_bought": "616.0",
  "base_units_remaining": "-5.0",
  "base_unit_name": "pint",
  "sale_unit_name": "pint",
  "sale_unit_name_plural": "pints",
  "base_units_per_sale_unit": "1.0",
  "stock_unit_name": "pint",
  "stock_unit_name_plural": "pints",
  "base_units_per_stock_unit": "1.0"
}
```

```json
{
  "type": "stocktype",
  "key": "stocktype/10",
  "id": 10,
  "department": {
    "type": "department",
    "id": 70,
    "description": "Soft Drinks",
    "notes": ""
  },
  "manufacturer": "Sunpride",
  "name": "Orange Juice",
  "abv": null,
  "fullname": "Sunpride Orange Juice",
  "price": "1.20",
  "logo": null,
  "tasting_notes": null,
  "base_units_bought": "120000.0",
  "base_units_remaining": "85892.0",
  "base_unit_name": "ml",
  "sale_unit_name": "pint",
  "sale_unit_name_plural": "pints",
  "base_units_per_sale_unit": "568.0",
  "stock_unit_name": "litre",
  "stock_unit_name_plural": "litres",
  "base_units_per_stock_unit": "1000.0"
}
```

### Stock item

Some types of stock are divided into "items", for example we may have
three casks of a particular type of beer. When they are put on sale on
the bar, the amount remaining is counted for each item as well as for
the type of stock as a whole.

Properties:

* `type`: the string "stockitem"
* `key`: the object's websocket key
* `id`: an integer uniquely identifying the stock item
* `stocktype`: the StockType object for the stock item
* `remaining`: the number of base units remaining of this stock item
* `size`: the number of base units originally in this stock item
* `remaining_pct`: a Decimal representing the percentage of this item that is remaining

Example:

```json
{
  "type": "stockitem",
  "key": "stockitem/234",
  "id": 234,
  "stocktype": {
    "type": "stocktype",
    "key": "stocktype/83",
    "id": 83,
    "department": {
      "type": "department",
      "id": 10,
      "description": "Real Ale",
      "notes": ""
    },
    "manufacturer": "Hop Kettle",
    "name": "North Wall",
    "abv": "4.3",
    "fullname": "Hop Kettle North Wall (4.3% ABV)",
    "price": "4.00",
    "logo": "/media/emf/product-logos/a68998c301b12244f18fd745ede57a817c09ef3d6e5cff0f4c47427509148840.png",
    "tasting_notes": null,
    "base_units_bought": "144.0",
    "base_units_remaining": "84.5",
    "base_unit_name": "pint",
    "sale_unit_name": "pint",
    "sale_unit_name_plural": "pints",
    "base_units_per_sale_unit": "1.0",
    "stock_unit_name": "pint",
    "stock_unit_name_plural": "pints",
    "base_units_per_stock_unit": "1.0"
  },
  "remaining": "84.5",
  "size": "144.0",
  "remaining_pct": "58.68055555555555555600"
}
```

### Stock line

Everything sold on the till is sold through a "stock line". In some
cases, this is a physical thing: a hand pump on the bar, a keg tap, or
a place on the bar where we keep boxes of cider. In other cases, it's
just an abstract thing, for example "the Orange Juice is currently
Princes Orange Juice cartons".

Properties:
* `type`: the string "stockline"
* `key`: the object's websocket key
* `id`: an integer uniquely identifying the stock line
* `name`: a string describing this stock line
* `location`: a string describing the physical location of this stock line
* `note`: a string describing the state of this stock line or the product on sale on it; may be blank
* `linetype`: a string describing the type of line; this will be `regular` for lines where *particular* stock items are put on sale, or `continuous` for lines where a whole stock type is on sale
* `stockitem`: the StockItem object on sale on this stock line, if the line is of type `regular` and there is something on sale on it; otherwise null
* `stocktype`: the StockType object on sale on this stock line, if the line is of type `continuous`; otherwise null

Example:
```json
{
  "type": "stockline",
  "key": "stockline/100",
  "id": 100,
  "name": "Pump 1",
  "location": "Bar",
  "note": "Freddie Starr ate my hamster",
  "linetype": "regular",
  "stockitem": {
    "type": "stockitem",
    "key": "stockitem/124",
    "id": 124,
    "stocktype": {
      "type": "stocktype",
      "key": "stocktype/23",
      "id": 23,
      "department": {
        "type": "department",
        "id": 10,
        "description": "Real Ale",
        "notes": ""
      },
      "manufacturer": "Milton",
      "name": "Justinian",
      "abv": "3.9",
      "fullname": "Milton Justinian (3.9% ABV)",
      "price": "4.50",
      "logo": "/media/emf/product-logos/cea539af1f7ebb03344d9458355bf6426f83c96dbf243a69e340a70b43935c54.png",
      "tasting_notes": "<p>Crisp pale bronze-coloured bitter. Attractive bitter orange flavours persist into a satisfying lasting finish.</p>",
      "base_units_bought": "432.0",
      "base_units_remaining": "44.0",
      "base_unit_name": "pint",
      "sale_unit_name": "pint",
      "sale_unit_name_plural": "pints",
      "base_units_per_sale_unit": "1.0",
      "stock_unit_name": "pint",
      "stock_unit_name_plural": "pints",
      "base_units_per_stock_unit": "1.0"
    },
    "remaining": "44.0",
    "size": "144.0"
  },
  "stocktype": null
}
```

## Endpoints

All endpoints return JSON objects; where an endpoint needs to return a
list, it returns an object with the list as the value of a key.

### Sessions

`/api/sessions.json` returns a list of current and future bar
Sessions.

Example (N.B. the times in this example are *not* final and may change
during the event based on available volunteers, etc.):

```json
{
  "sessions": [
    {
      "opening_time": "2022-06-02T11:00:00",
      "closing_time": "2022-06-03T01:00:00"
    },
    {
      "opening_time": "2022-06-03T11:00:00",
      "closing_time": "2022-06-04T01:00:00"
    },
    {
      "opening_time": "2022-06-04T11:00:00",
      "closing_time": "2022-06-05T01:00:00"
    },
    {
      "opening_time": "2022-06-05T11:00:00",
      "closing_time": "2022-06-06T00:30:00"
    }
  ]
}
```

### Event progress

`/api/progress.json` returns general information about bar progress.

Although this drives a couple of graphs on the [bar web site](https://bar.emf.camp/)
it is *not* intended to be scientifically accurate!

Properties (all Decimal percentages):

* `licensed_time_pct`: how far through the sessions are we at the moment?
* `expected_consumption_pct`: how far through the sessions are we at
the moment, with a weighting applied per session based on experience
at previous events?
* `actual_consumption_pct`: how much of the booze have we used?
"Booze" is any stock type with a non-null abv, including those with
an abv of "0.0".

### Departments

`/api/departments.json` returns the complete list of Departments.

Example:

```json
{
  "departments": [
    {
      "type": "department",
      "id": 10,
      "description": "Real Ale",
      "notes": null
    },
    /* many departments omitted */
    {
      "type": "department",
      "id": 310,
      "description": "Red Cross donations",
      "notes": null
    }
  ]
}
```

### Beers and ciders on tap

`/api/on-tap.json` returns lists of Stock items on sale on the bar at
the moment. The list is divided in to real ales (on handpumps), keg
beers (on taps) and real ciders (sold from boxes on the bar).

Properties:

* `ales`: list of real ale Stock items
* `kegs`: list of keg beer/cider Stock items
* `ciders`: list of real cider Stock items

Example:

```json
{
  "ales": [
    {
      "type": "stockitem",
      "key": "stockitem/234",
      "id": 234,
      "stocktype": {
        "type": "stocktype",
        "key": "stocktype/83",
        "id": 83,
        "department": {
          "type": "department",
          "id": 10,
          "description": "Real Ale",
          "notes": ""
        },
        "manufacturer": "Hop Kettle",
        "name": "North Wall",
        "abv": "4.3",
        "fullname": "Hop Kettle North Wall (4.3% ABV)",
        "price": "4.00",
        "logo": "/media/emf/product-logos/a68998c301b12244f18fd745ede57a817c09ef3d6e5cff0f4c47427509148840.png",
        "tasting_notes": null,
        "base_units_bought": "144.0",
        "base_units_remaining": "84.5",
        "base_unit_name": "pint",
        "sale_unit_name": "pint",
        "sale_unit_name_plural": "pints",
        "base_units_per_sale_unit": "1.0",
        "stock_unit_name": "pint",
        "stock_unit_name_plural": "pints",
        "base_units_per_stock_unit": "1.0"
      },
      "remaining": "84.5",
      "size": "144.0",
      "remaining_pct": "58.68055555555555555600"
    }
  ],
  "kegs": [],
  "ciders": []
}
```

### Null Sector bar / "Cybar"

`/api/cybar.json` lists stock types on sale at the bar in Null
Sector.

Example:

```json
{
  "cybar": [
    {
      "id": 59,
      "department": {
        "id": 60,
        "description": "Cans >0.5%",
        "notes": null
      },
      "manufacturer": "Arbor",
      "name": "Citrus Maxima 568ml",
      "abv": "4.0",
      "fullname": "Arbor Citrus Maxima 568ml (4.0% ABV)",
      "price": "5.70",
      "base_units_bought": "48.0",
      "base_units_remaining": "48.0",
      "base_unit": "can",
      "sale_unit": "can",
      "base_units_per_sale_unit": "1.0"
    },
    /* many stock types omitted */
    {
      "id": 30,
      "department": {
        "id": 95,
        "description": "Wine (cans)",
        "notes": null
      },
      "manufacturer": "Copper Crew",
      "name": "Sauvignon Blanc 250ml",
      "abv": "13.0",
      "fullname": "Copper Crew Sauvignon Blanc 250ml (13.0% ABV)",
      "price": "5.00",
      "base_units_bought": "48.0",
      "base_units_remaining": "48.0",
      "base_unit": "can",
      "sale_unit": "can",
      "base_units_per_sale_unit": "1.0"
    }
  ]
}
```

### All stock types

`/api/stocktypes.json` lists all the types of stock remaining at the
bar and cybar.

Example:

```json
{
  "stocktypes": [
    {
      "id": 23,
      "department": {
        "id": 10,
        "description": "Real Ale",
        "notes": null
      },
      "manufacturer": "Milton",
      "name": "Justinian",
      "abv": "3.9",
      "fullname": "Milton Justinian (3.9% ABV)",
      "price": "3.80",
      "base_units_bought": "432.0",
      "base_units_remaining": "432.0",
      "base_unit": "pint",
      "sale_unit": "pint",
      "base_units_per_sale_unit": "1.0"
    },
    /* many, many stock types omitted! */
    {
      "id": 30,
      "department": {
        "id": 95,
        "description": "Wine (cans)",
        "notes": null
      },
      "manufacturer": "Copper Crew",
      "name": "Sauvignon Blanc 250ml",
      "abv": "13.0",
      "fullname": "Copper Crew Sauvignon Blanc 250ml (13.0% ABV)",
      "price": "5.00",
      "base_units_bought": "48.0",
      "base_units_remaining": "48.0",
      "base_unit": "can",
      "sale_unit": "can",
      "base_units_per_sale_unit": "1.0"
    }
  ]
}
```

### All stock types in a department

You can retrieve all stock types in a department using
`/api/department/<int:dept_id>.json`.  If you know the department ID,
this is more efficient than retrieving everything from
`/api/stocktypes.json` and filtering locally.

For example, to retrieve the Club Mate stock levels you can fetch from
`/api/department/75.json`:

```json
{
  "stocktypes": [
    {
      "id": 32,
      "department": {
        "id": 75,
        "description": "Club Mate",
        "notes": null
      },
      "manufacturer": "Club Mate",
      "name": "Granat 500ml",
      "abv": null,
      "fullname": "Club Mate Granat 500ml",
      "price": "2.50",
      "base_units_bought": "800.0",
      "base_units_remaining": "800.0",
      "base_unit": "bottle",
      "sale_unit": "bottle",
      "base_units_per_sale_unit": "1.0"
    },
    {
      "id": 31,
      "department": {
        "id": 75,
        "description": "Club Mate",
        "notes": null
      },
      "manufacturer": "Club Mate",
      "name": "Regular 500ml",
      "abv": null,
      "fullname": "Club Mate Regular 500ml",
      "price": "2.50",
      "base_units_bought": "1600.0",
      "base_units_remaining": "1600.0",
      "base_unit": "bottle",
      "sale_unit": "bottle",
      "base_units_per_sale_unit": "1.0"
    }
  ]
}
```

### Retrieve a specific stock type

If you know the stock type ID you can fetch the Stock type object for
that directly from `/api/stocktype/<int:stocktype_id>.json`.

For example, to fetch the record for [ChariTea Mate
Tea](https://charitea.com/en/product/charitea-mate/) you can fetch
from `/api/stocktype/48.json`:

```json
{
  "id": 48,
  "department": {
    "id": 70,
    "description": "Soft Drinks",
    "notes": null
  },
  "manufacturer": "ChariTea",
  "name": "Mate 330ml",
  "abv": null,
  "fullname": "ChariTea Mate 330ml",
  "price": "3.40",
  "base_units_bought": "120.0",
  "base_units_remaining": "120.0",
  "base_unit": "bottle",
  "sale_unit": "bottle",
  "base_units_per_sale_unit": "1.0"
}
```

### Stock lines

**XXX This is an expensive API call if made with no parameters. We may
  want to restrict it or cache the output, or provide a cheaper call
  just for getting object keys**

`/api/stocklines.json` lists all of the stock lines at the bar and
cybar.

You can restrict the output to stock lines of a particular type using
the "type" query parameter, and to a particular location using the
"location" query parameter. For example:
`/api/stocklines.json?type=regular&location=Bar`

Example output for that query:
```json
{
  "stocklines": [
    {
      "type": "stockline",
      "key": "stockline/125",
      "id": 125,
      "name": "Cider 1",
      "location": "Bar",
      "note": "",
      "linetype": "regular",
      "stockitem": {
        "type": "stockitem",
        "key": "stockitem/254",
        "id": 254,
        "stocktype": {
          "type": "stocktype",
          "key": "stocktype/102",
          "id": 102,
          "department": {
            "type": "department",
            "id": 30,
            "description": "Real Cider",
            "notes": ""
          },
          "manufacturer": "Troggi",
          "name": "Tregagle (Dry Perry)",
          "abv": "6.0",
          "fullname": "Troggi Tregagle (Dry Perry) (6.0% ABV)",
          "price": "4.40",
          "logo": null,
          "tasting_notes": null,
          "base_units_bought": "35.2",
          "base_units_remaining": "-0.3",
          "base_unit_name": "pint",
          "sale_unit_name": "pint",
          "sale_unit_name_plural": "pints",
          "base_units_per_sale_unit": "1.0",
          "stock_unit_name": "pint",
          "stock_unit_name_plural": "pints",
          "base_units_per_stock_unit": "1.0"
        },
        "remaining": "-0.3",
        "size": "35.2"
      },
      "stocktype": null
    },
    {
      "type": "stockline",
      "key": "stockline/126",
      "id": 126,
      "name": "Cider 2",
      "location": "Bar",
      "note": "",
      "linetype": "regular",
      "stockitem": null,
      "stocktype": null
    },
    {
      "type": "stockline",
      "key": "stockline/127",
      "id": 127,
      "name": "Cider 3",
      "location": "Bar",
      "note": "",
      "linetype": "regular",
      "stockitem": {
        "type": "stockitem",
        "key": "stockitem/248",
        "id": 248,
        "stocktype": {
          "type": "stocktype",
          "key": "stocktype/96",
          "id": 96,
          "department": {
            "type": "department",
            "id": 30,
            "description": "Real Cider",
            "notes": ""
          },
          "manufacturer": "Gwatkin",
          "name": "Thorn Perry (Dry)",
          "abv": "6.0",
          "fullname": "Gwatkin Thorn Perry (Dry) (6.0% ABV)",
          "price": "4.30",
          "logo": null,
          "tasting_notes": null,
          "base_units_bought": "35.2",
          "base_units_remaining": "-2.8",
          "base_unit_name": "pint",
          "sale_unit_name": "pint",
          "sale_unit_name_plural": "pints",
          "base_units_per_sale_unit": "1.0",
          "stock_unit_name": "pint",
          "stock_unit_name_plural": "pints",
          "base_units_per_stock_unit": "1.0"
        },
        "remaining": "-2.8",
        "size": "35.2"
      },
      "stocktype": null
    },
    {
      "type": "stockline",
      "key": "stockline/104",
      "id": 104,
      "name": "Lager",
      "location": "Bar",
      "note": "",
      "linetype": "regular",
      "stockitem": {
        "type": "stockitem",
        "key": "stockitem/14",
        "id": 14,
        "stocktype": {
          "type": "stocktype",
          "key": "stocktype/2",
          "id": 2,
          "department": {
            "type": "department",
            "id": 25,
            "description": "Lager",
            "notes": ""
          },
          "manufacturer": "Dortmunder",
          "name": "Vier",
          "abv": "4.0",
          "fullname": "Dortmunder Vier (4.0% ABV)",
          "price": "4.00",
          "logo": null,
          "tasting_notes": null,
          "base_units_bought": "880.0",
          "base_units_remaining": "317.0",
          "base_unit_name": "pint",
          "sale_unit_name": "pint",
          "sale_unit_name_plural": "pints",
          "base_units_per_sale_unit": "1.0",
          "stock_unit_name": "pint",
          "stock_unit_name_plural": "pints",
          "base_units_per_stock_unit": "1.0"
        },
        "remaining": "53.0",
        "size": "88.0"
      },
      "stocktype": null
    },
    {
      "type": "stockline",
      "key": "stockline/100",
      "id": 100,
      "name": "Pump 1",
      "location": "Bar",
      "note": "Will need a clean after this cask",
      "linetype": "regular",
      "stockitem": {
        "type": "stockitem",
        "key": "stockitem/124",
        "id": 124,
        "stocktype": {
          "type": "stocktype",
          "key": "stocktype/23",
          "id": 23,
          "department": {
            "type": "department",
            "id": 10,
            "description": "Real Ale",
            "notes": ""
          },
          "manufacturer": "Milton",
          "name": "Justinian",
          "abv": "3.9",
          "fullname": "Milton Justinian (3.9% ABV)",
          "price": "4.50",
          "logo": "/media/emf/product-logos/cea539af1f7ebb03344d9458355bf6426f83c96dbf243a69e340a70b43935c54.png",
          "tasting_notes": "\u003Cp\u003ECrisp pale bronze-coloured bitter. Attractive bitter orange flavours persist into a satisfying lasting finish.\u003C/p\u003E",
          "base_units_bought": "432.0",
          "base_units_remaining": "44.0",
          "base_unit_name": "pint",
          "sale_unit_name": "pint",
          "sale_unit_name_plural": "pints",
          "base_units_per_sale_unit": "1.0",
          "stock_unit_name": "pint",
          "stock_unit_name_plural": "pints",
          "base_units_per_stock_unit": "1.0"
        },
        "remaining": "44.0",
        "size": "144.0"
      },
      "stocktype": null
    },
    {
      "type": "stockline",
      "key": "stockline/101",
      "id": 101,
      "name": "Pump 2",
      "location": "Bar",
      "note": "",
      "linetype": "regular",
      "stockitem": {
        "type": "stockitem",
        "key": "stockitem/211",
        "id": 211,
        "stocktype": {
          "type": "stocktype",
          "key": "stocktype/71",
          "id": 71,
          "department": {
            "type": "department",
            "id": 10,
            "description": "Real Ale",
            "notes": ""
          },
          "manufacturer": "Ledbury",
          "name": "Pale Ale",
          "abv": "4.0",
          "fullname": "Ledbury Pale Ale (4.0% ABV)",
          "price": "4.00",
          "logo": null,
          "tasting_notes": null,
          "base_units_bought": "288.0",
          "base_units_remaining": "117.0",
          "base_unit_name": "pint",
          "sale_unit_name": "pint",
          "sale_unit_name_plural": "pints",
          "base_units_per_sale_unit": "1.0",
          "stock_unit_name": "pint",
          "stock_unit_name_plural": "pints",
          "base_units_per_stock_unit": "1.0"
        },
        "remaining": "45.0",
        "size": "72.0"
      },
      "stocktype": null
    },
    {
      "type": "stockline",
      "key": "stockline/102",
      "id": 102,
      "name": "Pump 3",
      "location": "Bar",
      "note": "",
      "linetype": "regular",
      "stockitem": {
        "type": "stockitem",
        "key": "stockitem/216",
        "id": 216,
        "stocktype": {
          "type": "stocktype",
          "key": "stocktype/72",
          "id": 72,
          "department": {
            "type": "department",
            "id": 10,
            "description": "Real Ale",
            "notes": ""
          },
          "manufacturer": "Ledbury",
          "name": "Dark",
          "abv": "3.9",
          "fullname": "Ledbury Dark (3.9% ABV)",
          "price": "4.00",
          "logo": null,
          "tasting_notes": null,
          "base_units_bought": "288.0",
          "base_units_remaining": "31.5",
          "base_unit_name": "pint",
          "sale_unit_name": "pint",
          "sale_unit_name_plural": "pints",
          "base_units_per_sale_unit": "1.0",
          "stock_unit_name": "pint",
          "stock_unit_name_plural": "pints",
          "base_units_per_stock_unit": "1.0"
        },
        "remaining": "31.5",
        "size": "72.0"
      },
      "stocktype": null
    },
    {
      "type": "stockline",
      "key": "stockline/103",
      "id": 103,
      "name": "Pump 4",
      "location": "Bar",
      "note": "",
      "linetype": "regular",
      "stockitem": {
        "type": "stockitem",
        "key": "stockitem/234",
        "id": 234,
        "stocktype": {
          "type": "stocktype",
          "key": "stocktype/83",
          "id": 83,
          "department": {
            "type": "department",
            "id": 10,
            "description": "Real Ale",
            "notes": ""
          },
          "manufacturer": "Hop Kettle",
          "name": "North Wall",
          "abv": "4.3",
          "fullname": "Hop Kettle North Wall (4.3% ABV)",
          "price": "4.00",
          "logo": "/media/emf/product-logos/a68998c301b12244f18fd745ede57a817c09ef3d6e5cff0f4c47427509148840.png",
          "tasting_notes": null,
          "base_units_bought": "144.0",
          "base_units_remaining": "84.5",
          "base_unit_name": "pint",
          "sale_unit_name": "pint",
          "sale_unit_name_plural": "pints",
          "base_units_per_sale_unit": "1.0",
          "stock_unit_name": "pint",
          "stock_unit_name_plural": "pints",
          "base_units_per_stock_unit": "1.0"
        },
        "remaining": "84.5",
        "size": "144.0"
      },
      "stocktype": null
    },
    {
      "type": "stockline",
      "key": "stockline/105",
      "id": 105,
      "name": "Stowford Press",
      "location": "Bar",
      "note": "",
      "linetype": "regular",
      "stockitem": {
        "type": "stockitem",
        "key": "stockitem/7",
        "id": 7,
        "stocktype": {
          "type": "stocktype",
          "key": "stocktype/1",
          "id": 1,
          "department": {
            "type": "department",
            "id": 35,
            "description": "Keg cider",
            "notes": ""
          },
          "manufacturer": "Westons",
          "name": "Stowford Press",
          "abv": "4.5",
          "fullname": "Westons Stowford Press (4.5% ABV)",
          "price": "4.00",
          "logo": null,
          "tasting_notes": null,
          "base_units_bought": "616.0",
          "base_units_remaining": "-5.0",
          "base_unit_name": "pint",
          "sale_unit_name": "pint",
          "sale_unit_name_plural": "pints",
          "base_units_per_sale_unit": "1.0",
          "stock_unit_name": "pint",
          "stock_unit_name_plural": "pints",
          "base_units_per_stock_unit": "1.0"
        },
        "remaining": "-5.0",
        "size": "88.0"
      },
      "stocktype": null
    },
    {
      "type": "stockline",
      "key": "stockline/109",
      "id": 109,
      "name": "Tap 10",
      "location": "Bar",
      "note": "",
      "linetype": "regular",
      "stockitem": {
        "type": "stockitem",
        "key": "stockitem/173",
        "id": 173,
        "stocktype": {
          "type": "stocktype",
          "key": "stocktype/40",
          "id": 40,
          "department": {
            "type": "department",
            "id": 20,
            "description": "Craft Keg",
            "notes": ""
          },
          "manufacturer": "Thornbridge",
          "name": "AM:PM (GF)",
          "abv": "4.5",
          "fullname": "Thornbridge AM:PM (GF) (4.5% ABV)",
          "price": "5.20",
          "logo": null,
          "tasting_notes": null,
          "base_units_bought": "105.6",
          "base_units_remaining": "73.6",
          "base_unit_name": "pint",
          "sale_unit_name": "pint",
          "sale_unit_name_plural": "pints",
          "base_units_per_sale_unit": "1.0",
          "stock_unit_name": "pint",
          "stock_unit_name_plural": "pints",
          "base_units_per_stock_unit": "1.0"
        },
        "remaining": "20.8",
        "size": "52.8"
      },
      "stocktype": null
    },
    {
      "type": "stockline",
      "key": "stockline/106",
      "id": 106,
      "name": "Tap 7",
      "location": "Bar",
      "note": "",
      "linetype": "regular",
      "stockitem": {
        "type": "stockitem",
        "key": "stockitem/162",
        "id": 162,
        "stocktype": {
          "type": "stocktype",
          "key": "stocktype/35",
          "id": 35,
          "department": {
            "type": "department",
            "id": 20,
            "description": "Craft Keg",
            "notes": ""
          },
          "manufacturer": "Five Points",
          "name": "JUPA",
          "abv": "5.5",
          "fullname": "Five Points JUPA (5.5% ABV)",
          "price": "4.90",
          "logo": null,
          "tasting_notes": null,
          "base_units_bought": "176.0",
          "base_units_remaining": "86.5",
          "base_unit_name": "pint",
          "sale_unit_name": "pint",
          "sale_unit_name_plural": "pints",
          "base_units_per_sale_unit": "1.0",
          "stock_unit_name": "pint",
          "stock_unit_name_plural": "pints",
          "base_units_per_stock_unit": "1.0"
        },
        "remaining": "-1.5",
        "size": "88.0"
      },
      "stocktype": null
    },
    {
      "type": "stockline",
      "key": "stockline/107",
      "id": 107,
      "name": "Tap 8",
      "location": "Bar",
      "note": "",
      "linetype": "regular",
      "stockitem": null,
      "stocktype": null
    },
    {
      "type": "stockline",
      "key": "stockline/108",
      "id": 108,
      "name": "Tap 9",
      "location": "Bar",
      "note": "",
      "linetype": "regular",
      "stockitem": null,
      "stocktype": null
    }
  ]
}
```

## Web socket

You can connect to the web socket at `wss://bar.emf.camp/websocket/`

Once connected, you can subscribe to a key by sending the string
`SUBSCRIBE key`, for example `SUBSCRIBE stockline/107`.

You can unsubscribe from a key by sending the string `UNSUBSCRIBE
key`.

Shortly after you subscribe to a key, you will be sent the
corresponding object. If the system does not know about that object,
you will be sent an object with type "not present". The subscription
will remain in place, and if the object is subsequently created you
will receive it. (This is most likely to happen if the till server
reboots: the service that responds to websocket requests may finish
starting up before the service that forwards changes in state from the
till database to the object store.)

Whenever the object changes state, you will receive an updated copy.
