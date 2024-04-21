---
title: Bar and Shop data
linkTitle: Bar & Shop
description: >
  Opening times, live stock, prices and sales data for the Bar, Cybar and Shop.
---

There is a simple read-only interface to the till system running the
bar, Null Sector bar, and shop.  It's available via HTTP GET from
`bar.emf.camp` and returns JSON, except when you request an object
that does not exist when it will return HTTP 404.

The web service that implements this API is here: <https://github.com/emfcamp/quicktill-tillweb>

## Data types

All these data types are represented as JSON objects. Integers are
represented as JSON numbers, Decimals are represented as JSON
strings, times are local times in ISO 8601 format.

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

* `id`: an integer uniquely identifying the department
* `description`: a string describing the department
* `notes`: an optional string giving more information

Example:

```json
{
  "id": 90,
  "description": "Wine (bottles)",
  "notes": "Available in 125ml, 175ml and 250ml"
}
```

### Stock type

Properties:

* `id`: an integer uniquely identifying the stock type
* `department`: the Department object associated with the stock type
* `manufacturer`: a string naming the manufacturer
* `name`: a string describing this type of stock; unique per manufacturer
* `abv`: an optional Decimal giving the alcohol by volume
* `fullname`: manufacturer, name and abv combined into a single string
* `sale_unit`: a string giving the unit in which we sell this type of stock, eg. "bottle" or "pint"
* `price`: the sale price as a Decimal, including VAT where appropriate, for a `sale_unit` of this type of stock
* `base_unit`: a string giving the unit in which we count how much of this stock we have bought and sold, eg. "pint" or "ml"
* `base_units_bought`: the amount of stock bought in base units as a Decimal
* `base_units_remaining`: the amount of stock remaining in base units as a Decimal
* `base_units_per_sale_unit`: the number of base units per sale unit as a Decimal

Examples:

```json
{
  "id": 1,
  "department": {
    "id": 35,
    "description": "Keg cider",
    "notes": null
  },
  "manufacturer": "Westons",
  "name": "Stowford Press",
  "abv": "4.5",
  "fullname": "Westons Stowford Press (4.5% ABV)",
  "price": "4.00",
  "base_units_bought": "616.0",
  "base_units_remaining": "616.0",
  "base_unit": "pint",
  "sale_unit": "pint",
  "base_units_per_sale_unit": "568.0"
}
```

```json
{
  "id": 10,
  "department": {
    "id": 70,
    "description": "Soft Drinks",
    "notes": null
  },
  "manufacturer": "Sunpride",
  "name": "Orange Juice",
  "abv": null,
  "fullname": "Sunpride Orange Juice",
  "price": "1.20",
  "base_units_bought": "120000.0",
  "base_units_remaining": "120000.0",
  "base_unit": "ml",
  "sale_unit": "pint",
  "base_units_per_sale_unit": "568.0"
}
```

### Stock item

Some types of stock are divided into "items", for example we may have
three casks of a particular type of beer. When they are put on sale on
the bar, the amount remaining is counted for each item as well as for
the type of stock as a whole.

Properties:

* `id`: an integer uniquely identifying the stock item
* `stocktype_id`: an integer identifying the type of stock
* `manufacturer`: a string naming the manufacturer
* `name`: a string describing this type of stock; unique per manufacturer
* `abv`: an optional Decimal giving the alcohol by volume
* `fullname`: manufacturer, name and abv combined into a single string
* `price`: the sale price as a Decimal, including VAT where appropriate, for a unit of this type of stock
* `remaining_pct`: a Decimal representing the percentage of this item that is remaining

Example:

```json
{
  "id": 128,
  "stocktype_id": 25,
  "manufacturer": "Milton",
  "name": "Nero",
  "abv": "5.0",
  "fullname": "Milton Nero (5.0% ABV)",
  "price": "4.40",
  "remaining_pct": "100.00000000000000000000"
}
```

### Price lookup

We are not counting items of stock at the shop; the till system only
knows how much we are selling each item for.

Properties:

* `id`: an integer uniquely identifying the price lookup
* `department`: the Department object associated with the stock type
* `description`: a string describing the item
* `note`: an optional string describing the item further
* `department`: the Department object associated with the price lookup
* `price`: the amount we sell one of these items for as a Decimal

Example:

```json
{
  "id": 1,
  "description": "USB A-C",
  "note": "",
  "department": {
    "id": 210,
    "description": "Shop (20% VAT)",
    "notes": null
  },
  "price": "2.20"
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
      "id": 10,
      "description": "Real Ale",
      "notes": null
    },
    /* many departments omitted */
    {
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
      "id": 128,
      "stocktype_id": 25,
      "manufacturer": "Milton",
      "name": "Nero",
      "abv": "5.0",
      "fullname": "Milton Nero (5.0% ABV)",
      "price": "4.40",
      "remaining_pct": "100.00000000000000000000"
    }
  ],
  "kegs": [],
  "ciders": []
}
```

### Null Sector bar / "Cybar"

`/api/cybar.json` lists stock types on sale at the bar in Null
Sector. (Null Sector sells pre-packaged drinks out of fridges.)

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

### The shop

We are not counting items of stock at the shop; the till system only
knows how much we are selling each item for.

`/api/shop.json` returns a list of all price lookups defined at the
shop. N.B. this is not a guarantee that any of these items remain in
stock!

Example:

```json
{
  "shop": [
    {
      "id": 1,
      "description": "USB A-C",
      "note": "",
      "department": {
        "id": 210,
        "description": "Shop (20% VAT)",
        "notes": null
      },
      "price": "2.20"
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
