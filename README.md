# Naked HTMX

This is a golang, HTMX based implementation of Naked Objects.

https://en.wikipedia.org/wiki/Naked_objects exposes the objects, like struts or DB rows directly to the GUI.

https://htmx.org is a simple way to build interactive web sites and marries well with the Naked Objects simplicity.

We are using latest htmx: https://unpkg.com/htmx.org@2.0.2

Its a fork of https://github.com/warlockxins/goPocJsonSchemaForm, as it seems to be abandoned. 


![image basic](./doc/basic.png)

![image control1](./doc/control1.png)

![image control2copy](./doc/control2copy.png)

## Status

It works now, but not DB following..

```sh 

# http://localhost:3000
go run cmd/main.go

here is deep obj map[age: birthDate: name: nationality: occupation: personalData:map[height:]]
here is deep obj map[birthDate: height: name:ss nationality: occupation: personalData:map[age:]]
here is deep obj map[birthDate:2024-09-11 height:1 name: ss nationality:DE occupation: personalData:map[age:12]]
here is deep obj map[age:12 birthDate:2024-09-11 name: ss nationality:DE occupation:ar personalData:map[height:1]]
here is deep obj map[age:12 birthDate:2024-09-11 name: ss nationality:DE occupation:ar personalData:map[height:12]]
here is deep obj map[boolean: date: dateTime: enum: integer: number: string: time:

```

## How ?

Using HTMX, this package reads the JSON Schema to describe your data structure and validation rules, and another JSON Schema to describe the GUI widgets, and then renders the HTML with full data validation using HTMX.

There are 3 examples in the **screens** folder. Here is what **control1** looks like.

The DATA description:

```json

{
  "type": "object",
  "properties": {
    "string": {
      "type": "string"
    },
    "boolean": {
      "type": "boolean",
      "description": "Boolean description as a tooltip"
    },
    "number": {
      "type": "number"
    },
    "integer": {
      "type": "integer"
    },
    "date": {
      "type": "string",
      "format": "date"
    },
    "time": {
      "type": "string",
      "format": "time"
    },
    "dateTime": {
      "type": "string",
      "format": "date-time"
    },
    "enum": {
      "type": "string",
      "enum": [
        "One",
        "Two",
        "Three"
      ]
    }
  }
}

```

The GUI Description:

```json
{
  "type": "VerticalLayout",
  "elements": [
    {
      "type": "Control",
      "scope": "#/properties/string"
    },
    {
      "type": "Control",
      "scope": "#/properties/boolean"
    },
    {
      "type": "Control",
      "scope": "#/properties/number"
    },
    {
      "type": "Control",
      "scope": "#/properties/integer"
    },
    {
      "type": "Control",
      "scope": "#/properties/date"
    },
    {
      "type": "Control",
      "scope": "#/properties/time"
    },
    {
      "type": "Control",
      "scope": "#/properties/dateTime"
    },
    {
      "type": "Control",
      "scope": "#/properties/enum"
    }
  ]
}

```


## Look and Feel

The UI Look and feel is easily changeable.

Layout using the standard golang HTMX structure as seen in this repo, where you have the layout and page archetypes.

Look using any GUI toolkit, such as **UIKIT**, works fine with 2 javascript head imports. https://getuikit.com/docs/installation#download

## Screen types

The classic Master / Detail Pattern is everywhere. https://en.wikipedia.org/wiki/Master–detail_interface

You all know it intuitively...Its the boring stuff...

1. Master is a list of rows in the DB, typically with a search form above it, to paginate through it.
2. Detail is a single row, where you can edit the data.

Master Page is to be added.

## Workflows

Cross Form Validation, for when you filling out a form where one forms data is dependent on another forms data is easy since the GUI is directly driven by the DB, and your saving to the DB when you you submit and move on to the next form.

However, for very complex use cases, the user changes some data on Form 2, that is related to Form 1, we need to navigate them back to Form 1.

This wil be added using Event Choreography pattern for loose decoupling, so that any complex form dependencies can be achieved.

