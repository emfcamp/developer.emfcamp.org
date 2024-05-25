---
title: Bar data
linkTitle: Bar
description: >
  Opening times, live stock, prices and sales data for the Bar and Cybar.
---
* * *
Example data is from this year's database, although sale prices
are still subject to change.

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

* `type`: the string "session"
* `opening_time`: opening time
* `closing_time`: closing time

Example:

```json
{
  "type": "session",
  "opening_time": "2024-05-31T11:00:00",
  "closing_time": "2024-06-01T01:00:00"
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
    "id": 20,
    "description": "Craft Keg",
    "notes": null
  },
  "manufacturer": "Milton",
  "name": "Dynamo",
  "abv": "3.9",
  "fullname": "Milton Dynamo (3.9% ABV)",
  "price": "5.00",
  "logo": "/media/emf/product-logos/f6ecbcf8bac0d2ac95791a9aad246666fe2c3a48924966145f914d8b645cc4f7.png",
  "tasting_notes": null,
  "base_units_bought": "1056.0",
  "base_units_remaining": "1056.0",
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
  "key": "stocktype/28",
  "id": 28,
  "department": {
    "type": "department",
    "id": 70,
    "description": "Soft Drink Cartons",
    "notes": null
  },
  "manufacturer": "Sunpride",
  "name": "Apple Juice",
  "abv": null,
  "fullname": "Sunpride Apple Juice",
  "price": "2.50",
  "logo": null,
  "tasting_notes": null,
  "base_units_bought": "36000.0",
  "base_units_remaining": "36000.0",
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
* `description`: a description of the item, for example "Firkin" or "50l keg"
* `remaining`: the number of base units remaining of this stock item
* `size`: the number of base units originally in this stock item
* `remaining_pct`: a Decimal representing the percentage of this item that is remaining

Example:

```json
{
  "type": "stockitem",
  "key": "stockitem/3",
  "id": 3,
  "stocktype": {
    "type": "stocktype",
    "key": "stocktype/1",
    "id": 1,
    "department": {
      "type": "department",
      "id": 20,
      "description": "Craft Keg",
      "notes": null
    },
    "manufacturer": "Milton",
    "name": "Dynamo",
    "abv": "3.9",
    "fullname": "Milton Dynamo (3.9% ABV)",
    "price": "5.00",
    "logo": "/media/emf/product-logos/f6ecbcf8bac0d2ac95791a9aad246666fe2c3a48924966145f914d8b645cc4f7.png",
    "tasting_notes": null,
    "base_units_bought": "1056.0",
    "base_units_remaining": "1056.0",
    "base_unit_name": "pint",
    "sale_unit_name": "pint",
    "sale_unit_name_plural": "pints",
    "base_units_per_sale_unit": "1.0",
    "stock_unit_name": "pint",
    "stock_unit_name_plural": "pints",
    "base_units_per_stock_unit": "1.0"
  },
  "remaining": "88.0",
  "size": "88.0"
  "remaining_pct": "100.00"
}
```

### Stock line

Everything sold on the till is sold through a "stock line". In some
cases, this is a physical thing: a hand pump on the bar, a keg tap, or
a place on the bar where we keep boxes of cider. In other cases, it's
just an abstract thing, for example "the Orange Juice is currently
Princes Orange Juice cartons".

There are two variants of the StockLine object. The "full" varient has
type "stockline" and has all the properties listed below. The "brief"
varient has type "stockline-brief" and does not have the `stockitem`
or `stocktype` properties.

Properties:
* `type`: the string "stockline" or "stockline-brief"
* `key`: the object's websocket key
* `id`: an integer uniquely identifying the stock line
* `name`: a string describing this stock line
* `location`: a string describing the physical location of this stock line
* `note`: a string describing the state of this stock line or the product on sale on it; may be blank
* `linetype`: a string describing the type of line; this will be `regular` for lines where *particular* stock items are put on sale, or `continuous` for lines where a whole stock type is on sale
* `stockitem`: the StockItem object on sale on this stock line, if the line is of type `regular` and there is something on sale on it; otherwise null
* `stocktype`: the StockType object on sale on this stock line, if the line is of type `continuous`; otherwise null

Examples:
```json
{
  "type": "stockline",
  "key": "stockline/104",
  "id": 104,
  "name": "Tap 1",
  "location": "Bar",
  "note": "Example",
  "linetype": "regular",
  "stockitem": {
    "type": "stockitem",
    "key": "stockitem/3",
    "id": 3,
    "stocktype": {
      "type": "stocktype",
      "key": "stocktype/1",
      "id": 1,
      "department": {
        "type": "department",
        "id": 20,
        "description": "Craft Keg",
        "notes": null
      },
      "manufacturer": "Milton",
      "name": "Dynamo",
      "abv": "3.9",
      "fullname": "Milton Dynamo (3.9% ABV)",
      "price": "5.00",
      "logo": "/media/emf/product-logos/f6ecbcf8bac0d2ac95791a9aad246666fe2c3a48924966145f914d8b645cc4f7.png",
      "tasting_notes": null,
      "base_units_bought": "1056.0",
      "base_units_remaining": "1056.0",
      "base_unit_name": "pint",
      "sale_unit_name": "pint",
      "sale_unit_name_plural": "pints",
      "base_units_per_sale_unit": "1.0",
      "stock_unit_name": "pint",
      "stock_unit_name_plural": "pints",
      "base_units_per_stock_unit": "1.0"
    },
    "remaining": "88.0",
    "size": "88.0"
    "remaining_pct": "100.00"
  },
  "stocktype": null
}
```

```json
{
  "type": "stockline",
  "key": "stockline/130",
  "id": 130,
  "name": "Club Mate Regular",
  "location": "Fridge",
  "note": "",
  "linetype": "continuous",
  "stockitem": null,
  "stocktype": {
    "type": "stocktype",
    "key": "stocktype/8",
    "id": 8,
    "department": {
      "type": "department",
      "id": 75,
      "description": "Club Mate",
      "notes": null
    },
    "manufacturer": "Club Mate",
    "name": "Regular 500ml",
    "abv": null,
    "fullname": "Club Mate Regular 500ml",
    "price": "2.80",
    "logo": "/media/emf/product-logos/f38444690c6b28a8dbd6a238c1741d36dd6990aa09243dda4c30513292580a8e.png",
    "tasting_notes": null,
    "base_units_bought": "1700.0",
    "base_units_remaining": "1700.0",
    "base_unit_name": "bottle",
    "sale_unit_name": "bottle",
    "sale_unit_name_plural": "bottles",
    "base_units_per_sale_unit": "1.0",
    "stock_unit_name": "bottle",
    "stock_unit_name_plural": "bottles",
    "base_units_per_stock_unit": "1.0"
  }
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
      "type": "session",
      "opening_time": "2024-05-29T20:00:00",
      "closing_time": "2024-05-29T23:00:00"
    },
    {
      "type": "session",
      "opening_time": "2024-05-30T15:00:00",
      "closing_time": "2024-05-31T00:30:00"
    },
    {
      "type": "session",
      "opening_time": "2024-05-31T11:00:00",
      "closing_time": "2024-06-01T01:00:00"
    },
    {
      "type": "session",
      "opening_time": "2024-06-01T11:00:00",
      "closing_time": "2024-06-02T01:00:00"
    },
    {
      "type": "session",
      "opening_time": "2024-06-02T11:00:00",
      "closing_time": "2024-06-03T01:00:00"
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

Example:
```json
{
  "licensed_time_pct": "0",
  "expected_consumption_pct": "0",
  "actual_consumption_pct": "0"
}
```

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
      "id": 100,
      "description": "Cup re-use",
      "notes": null
    }
  ]
}
```

### Beers and ciders on tap

`/api/on-tap.json` returns lists of Stock items on sale on the bar at
the moment. The list is divided in to real ales (on handpumps), keg
beers and ciders (on taps) and real ciders (sold from boxes on the bar).

Properties:

* `ales`: list of real ale Stock items
* `kegs`: list of keg beer/cider Stock items
* `ciders`: list of real cider Stock items

`/api/cybar-on-tap.json` returns lists of Stock items on sale on the
bar in Null Sector. The list is divided in to keg beers and ciders (on
taps) and real ciders (sold from boxes on the bar).

Properties:

* `kegs`: list of keg beer/cider Stock items
* `ciders`: list of real cider Stock items

Example from `/api/on-tap.json`:

```json
{
  "ales": [],
  "kegs": [
    {
      "type": "stockitem",
      "key": "stockitem/3",
      "id": 3,
      "stocktype": {
        "type": "stocktype",
        "key": "stocktype/1",
        "id": 1,
        "department": {
          "type": "department",
          "id": 20,
          "description": "Craft Keg",
          "notes": null
        },
        "manufacturer": "Milton",
        "name": "Dynamo",
        "abv": "3.9",
        "fullname": "Milton Dynamo (3.9% ABV)",
        "price": "5.00",
        "logo": "/media/emf/product-logos/f6ecbcf8bac0d2ac95791a9aad246666fe2c3a48924966145f914d8b645cc4f7.png",
        "tasting_notes": null,
        "base_units_bought": "1056.0",
        "base_units_remaining": "1056.0",
        "base_unit_name": "pint",
        "sale_unit_name": "pint",
        "sale_unit_name_plural": "pints",
        "base_units_per_sale_unit": "1.0",
        "stock_unit_name": "pint",
        "stock_unit_name_plural": "pints",
        "base_units_per_stock_unit": "1.0"
      },
      "remaining": "88.0",
      "size": "88.0",
      "remaining_pct": "100.00"
    }
  ],
  "ciders": []
}
```

### All stock types

`/api/stocktypes.json` lists all the types of stock remaining at the
bar and cybar.

⚠️This is a fairly expensive API call, please do not poll it! If you
want to receive updates, save the object keys and subscribe to the
objects over the websocket.

Example:

```json
{
  "stocktypes": [
    {
      "type": "stocktype",
      "key": "stocktype/16",
      "id": 16,
      "department": {
        "type": "department",
        "id": 10,
        "description": "Real Ale",
        "notes": null
      },
      "manufacturer": "Ledbury",
      "name": "Admiral Parker Pale Ale",
      "abv": "4.0",
      "fullname": "Ledbury Admiral Parker Pale Ale (4.0% ABV)",
      "price": "5.00",
      "logo": "/media/emf/product-logos/40bf1d101e329f015fb74b597a372e4fe3bce75eb11504b95b1984c3708b518c.png",
      "tasting_notes": "<p>This 4% pale only uses hops grown by Simon Parker at Instone Court in Bishops Frome. We have Simon’s Admiral, Opus, Ernest and Emperor hops. It is amazing that you can get the complex aromas of Orange, Lemon, Mint, Peach, Tea, Lime, Apricots and Pepper from not just locally grown hops but from hops grown on a single farm in Herefordshire.</p>",
      "base_units_bought": "144.0",
      "base_units_remaining": "144.0",
      "base_unit_name": "pint",
      "sale_unit_name": "pint",
      "sale_unit_name_plural": "pints",
      "base_units_per_sale_unit": "1.0",
      "stock_unit_name": "pint",
      "stock_unit_name_plural": "pints",
      "base_units_per_stock_unit": "1.0"
    },
    /* many, many stocktypes omitted! */
    {
      "type": "stocktype",
      "key": "stocktype/32",
      "id": 32,
      "department": {
        "type": "department",
        "id": 90,
        "description": "Wine Bottles",
        "notes": "Available in 125ml, 175ml and 250ml"
      },
      "manufacturer": "Inkosi",
      "name": "Sauvignon Blanc",
      "abv": "13.5",
      "fullname": "Inkosi Sauvignon Blanc (13.5% ABV)",
      "price": "20.00",
      "logo": null,
      "tasting_notes": null,
      "base_units_bought": "27000.0",
      "base_units_remaining": "27000.0",
      "base_unit_name": "ml",
      "sale_unit_name": "bottle",
      "sale_unit_name_plural": "bottles",
      "base_units_per_sale_unit": "750.0",
      "stock_unit_name": "bottle",
      "stock_unit_name_plural": "bottles",
      "base_units_per_stock_unit": "750.0"
    }
  ]
}
```

### All stock types in a department

You can retrieve all stock types in a department using
`/api/department/<int:dept_id>.json`.  If you know the department ID,
this is more efficient than retrieving everything from
`/api/stocktypes.json` and filtering locally.

⚠️This is a fairly expensive API call, please do not poll it! If you
want to receive updates, save the object keys and subscribe to the
objects over the websocket.

For example, to retrieve the Club Mate stock levels you can fetch from
`/api/department/75.json`:

```json
{
  "stocktypes": [
    {
      "type": "stocktype",
      "key": "stocktype/9",
      "id": 9,
      "department": {
        "type": "department",
        "id": 75,
        "description": "Club Mate",
        "notes": null
      },
      "manufacturer": "Club Mate",
      "name": "Granat 500ml",
      "abv": null,
      "fullname": "Club Mate Granat 500ml",
      "price": "2.80",
      "logo": "/media/emf/product-logos/e4852de71fb174907169b8b011a751e5bcb34efa60ce5683a167d8d9e1b75f36.png",
      "tasting_notes": null,
      "base_units_bought": "700.0",
      "base_units_remaining": "700.0",
      "base_unit_name": "bottle",
      "sale_unit_name": "bottle",
      "sale_unit_name_plural": "bottles",
      "base_units_per_sale_unit": "1.0",
      "stock_unit_name": "bottle",
      "stock_unit_name_plural": "bottles",
      "base_units_per_stock_unit": "1.0"
    },
    {
      "type": "stocktype",
      "key": "stocktype/8",
      "id": 8,
      "department": {
        "type": "department",
        "id": 75,
        "description": "Club Mate",
        "notes": null
      },
      "manufacturer": "Club Mate",
      "name": "Regular 500ml",
      "abv": null,
      "fullname": "Club Mate Regular 500ml",
      "price": "2.80",
      "logo": "/media/emf/product-logos/f38444690c6b28a8dbd6a238c1741d36dd6990aa09243dda4c30513292580a8e.png",
      "tasting_notes": null,
      "base_units_bought": "1700.0",
      "base_units_remaining": "1700.0",
      "base_unit_name": "bottle",
      "sale_unit_name": "bottle",
      "sale_unit_name_plural": "bottles",
      "base_units_per_sale_unit": "1.0",
      "stock_unit_name": "bottle",
      "stock_unit_name_plural": "bottles",
      "base_units_per_stock_unit": "1.0"
    }
  ]
}
```

### Retrieve a specific stock type

If you know the stock type ID you can fetch the Stock type object for
that directly from `/api/stocktype/<int:stocktype_id>.json`.

For example, to fetch the record for Stowford Press, you can fetch
from `/api/stocktype/10.json`:

```json
{
  "type": "stocktype",
  "key": "stocktype/10",
  "id": 10,
  "department": {
    "type": "department",
    "id": 35,
    "description": "Keg Cider",
    "notes": null
  },
  "manufacturer": "Westons",
  "name": "Stowford Press",
  "abv": "4.5",
  "fullname": "Westons Stowford Press (4.5% ABV)",
  "price": "4.50",
  "logo": "/media/emf/product-logos/f584a1ee8a3dafd443aaa6d991c8e18c7e3febf483ff1edad730691dec881ff1.png",
  "tasting_notes": "<p>A refreshing medium-dry sparkling cider that is bursting with the delicious flavour of crisp cider apples.</p>",
  "base_units_bought": "704.0",
  "base_units_remaining": "704.0",
  "base_unit_name": "pint",
  "sale_unit_name": "pint",
  "sale_unit_name_plural": "pints",
  "base_units_per_sale_unit": "1.0",
  "stock_unit_name": "pint",
  "stock_unit_name_plural": "pints",
  "base_units_per_stock_unit": "1.0"
}
```

### Locations

`/api/locations.json` lists all of the stock line locations at the bar
and cybar. These can be used to filter stocklines.

Example:

```json
{
  "locations": [
    "Back bar",
    "Bar",
    "Fridge",
    "Null Sector",
    "Optics (main bar)",
    "Optics (Null Sector)"
  ]
}
```

### Stock lines

`/api/stocklines.json` lists all of the stock lines at the bar and
cybar.

By default it returns the "brief" variant of the StockLine object. You
can request full StockLine objects by setting the "output" query
parameter to "full"; please be aware that this can be an expensive
query and you should not use it to poll! Subscribe to updates using
the websocket interface instead.

You can restrict the output to stock lines of a particular type using
the "type" query parameter, and to a particular location using the
"location" query parameter. For example:

* `/api/stocklines.json?type=regular&location=Bar`
* `/api/stocklines.json?type=regular&location=Null%20Sector`

Example output for location=Bar:
```json
{
  "stocklines": [
    {
      "type": "stockline-brief",
      "key": "stockline/119",
      "id": 119,
      "name": "Cider 1",
      "location": "Bar",
      "note": "",
      "linetype": "regular"
    },
    {
      "type": "stockline-brief",
      "key": "stockline/120",
      "id": 120,
      "name": "Cider 2",
      "location": "Bar",
      "note": "",
      "linetype": "regular"
    },
    {
      "type": "stockline-brief",
      "key": "stockline/121",
      "id": 121,
      "name": "Cider 3",
      "location": "Bar",
      "note": "",
      "linetype": "regular"
    },
    {
      "type": "stockline-brief",
      "key": "stockline/100",
      "id": 100,
      "name": "Pump 1",
      "location": "Bar",
      "note": "",
      "linetype": "regular"
    },
    {
      "type": "stockline-brief",
      "key": "stockline/101",
      "id": 101,
      "name": "Pump 2",
      "location": "Bar",
      "note": "",
      "linetype": "regular"
    },
    {
      "type": "stockline-brief",
      "key": "stockline/102",
      "id": 102,
      "name": "Pump 3",
      "location": "Bar",
      "note": "",
      "linetype": "regular"
    },
    {
      "type": "stockline-brief",
      "key": "stockline/103",
      "id": 103,
      "name": "Pump 4",
      "location": "Bar",
      "note": "",
      "linetype": "regular"
    },
    {
      "type": "stockline-brief",
      "key": "stockline/104",
      "id": 104,
      "name": "Tap 1",
      "location": "Bar",
      "note": "Example",
      "linetype": "regular"
    },
    {
      "type": "stockline-brief",
      "key": "stockline/105",
      "id": 105,
      "name": "Tap 2",
      "location": "Bar",
      "note": "",
      "linetype": "regular"
    },
    {
      "type": "stockline-brief",
      "key": "stockline/106",
      "id": 106,
      "name": "Tap 3",
      "location": "Bar",
      "note": "",
      "linetype": "regular"
    },
    {
      "type": "stockline-brief",
      "key": "stockline/107",
      "id": 107,
      "name": "Tap 4",
      "location": "Bar",
      "note": "",
      "linetype": "regular"
    },
    {
      "type": "stockline-brief",
      "key": "stockline/108",
      "id": 108,
      "name": "Tap 5",
      "location": "Bar",
      "note": "",
      "linetype": "regular"
    },
    {
      "type": "stockline-brief",
      "key": "stockline/109",
      "id": 109,
      "name": "Tap 6",
      "location": "Bar",
      "note": "",
      "linetype": "regular"
    }
  ]
}
```

### Null Sector bar / "Cybar"

The Null Sector bar sells the same stock as the main bar, with a
couple of exceptions:

* The beers and ciders on tap are different: fetch the current selection using `/api/cybar-on-tap.json` instead of `/api/on-tap.json`, or filter for "location=Null Sector" using `/api/stocklines.json`
* Although the selection of spirits is the same, the individual bottles are necessarily different! Use `/api/stocklines.json` filtering for "location=Optics (Null Sector)" to retrieve the details specific to Null Sector

All other stock (wines, cans, bottles, soft drinks) is not tracked
separately between the bars.

## Web socket

You can connect to the web socket at `wss://bar.emf.camp/websocket/`

Once connected, you can subscribe to a key by sending the string
`SUBSCRIBE key`, for example `SUBSCRIBE stockline/107`.

You can unsubscribe from a key by sending the string `UNSUBSCRIBE
key`. You will not receive a response. If an update to that key was
already on the way to you, you may still receive it even after
unsubscribing.

Shortly after you subscribe to a key, you will be sent the
corresponding object. If the system does not know about that object,
you will be sent an object with type "not present". The subscription
will remain in place, and if the object is subsequently created you
will receive it. (This is most likely to happen if the till server
reboots: the service that responds to websocket requests may finish
starting up before the service that forwards changes in state from the
till database to the object store.)

Whenever the object changes state, you will receive an updated copy.

## Web socket objects

All the objects mentioned above that have a `key` property can be
subscribed to over the web socket.

Additionally, the following objects are only available over the web
socket.

### Totals by Unit

Key: `totals/by-unit`

This object gives the amounts sold broken down by Unit, where Unit is
one of the following:

* `25ml measure`
* `Bottle (not wine)`
* `Can`
* `Pint (draught)`
* `Pint (from carton)`
* `Wine bottle`

The amount sold is given as a Decimal. If the amount sold is zero, the
Unit is omitted.

Example:

```json
{
  "type": "totals by unit",
  "key": "totals/by-unit",
  "units": {
    "Can": "1.0"
  }
}
```

### Cups re-used

Key: `totals/cups-re-used`

This object gives the number of cups that have been brought back to
the bar to be re-filled. (When someone brings a cup to the bar to be
filled with their drink, rather than take a fresh cup from our supply,
we offer a small discount. This event we are paying a small amount to
hire re-usable plastic cups, but a larger amount for each one actually
used and requiring cleaning.)

This object is not updated in real-time. Instead, an update is
published every minute.

Example:

```json
{
  "type": "cups re-used",
  "key": "totals/cups-re-used",
  "count": 1
}
```

## MQTT

All the objects that can be subscribed to over the websocket will also
be published over MQTT.

Check out the [MQTT broker](/mqtt/) page for details on how to connect.
