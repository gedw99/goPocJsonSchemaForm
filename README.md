# HTMX and Naked Objects

This is a golang, HTMX based implementation of Naked Objects - https://en.wikipedia.org/wiki/Naked_objects

Using HTMX, this package uses JSON Schema to describe your data structure and validation rules, and JSON Schema to describe the GUI, and then render the WebGUI and do data validation using HTMX.

This means that you can reflect off your underlying database ( or any store ) to, at runtime, have a Web GUI. The JSON Schema and GUI JSON SCHEMA being generated at runtime of sensible reflection.

So when your DB structure changes, the GUI changes with it automatically.

## Generation or Refection at Design Time or Runtime ?

Take your pick..

This is 100% reflection based, and so can be changed at runtime.

Generate the JSONSchema at Runtime or Design time ( aka code gen ) as the DB structure changes too, which is nice.

You can also move the "compute to the data" at runtime with WASM, using Compiler as a Service patterns.

This will run just fine as WASM on the Server or Clients, allowing HTMX to be used in either place.
Feed your DB schema changes as JSON to the WASM GUI, or some other thing you want.

Kind of almost a Low code, no code platform then. 


## How ?

There are 3 examples in https://github.com/gedw99/goPocJsonSchemaForm/tree/main/screens

This is what the 2nd example, called "control1", looks like.

There two files only needed.

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

## CQRS / Event Sourced

Because the GUI is a direct representation of the Data, you may want to use this in a CQRS style Architecture, where the Read and Write side have different data structures.  This means your GUI is decoupled from your Events Store.

You could, for example, use this with Pocketbase. Pocketbase is a Read side CQRS (NewSQL denormalised structure).

This allows Event sourced Architectures with the Mutations ( the Write side of CQRS ) being described by JSON Schema, and the View ( the read side of the CQRS ) being also described by JSON Schema. 

Benthos is a good way to do this, as is NATS Jetstream. I use both in combination.

https://docs.redpanda.com/redpanda-connect/components/processors/json_schema/

https://github.com/nats-io/jsm.go/blob/main/schema_source/definitions.json

This gives you an OLTP and OLAP in one system also. You can do pretty much any sort of Project with it.



