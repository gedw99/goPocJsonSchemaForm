# Techncial

This is the Techncial descrioptin notes.


## Design Diagrams

TODO: Add a Diagram to show the Pyramid Ustream / Downstream pattern. This is the Logical Topology for how it works.

TODO. Add a Diagram to show the Hub and Spoke Patter. This is the Physical topology of it working when in Multi Org mode.



## Service Worker and Client Side Rendering

WIP... 

https://github.com/tractordev/wanix gives a full UX...



## DB following

So when your DB structure changes, the GUI changes with it automatically in real time.

The JSON Schema and GUI JSON SCHEMA being generated at runtime based on sensible reflection logic.

### DB Schema CDC

Data CDC = Change Data Capture of the DB Data.

Schema CDC = Change Data Capture of the DB Schema.

We need to know that the DB schema changed and what changed ( if possible ). 

We can then can regenerate the DB JSON Schema and the UI JSON Schema to account for the difference.

Any in flight screens where a user is looking at a screen that is affected by the DB Schema change needs to also be updated in real time.  No Screen is left behind :) 

We  can  diff the old json schema’s with the new and then then calculate what partial areas of the html is affected and then real time update all screens with the new html. No screen is left behind so that we don’t get http posts with wrong data coming back in to the server . If I do ( which is the nature of eventually consistent systems ), I will catch the difference back on the server and remap the data  using the json schema diff.

From that wish list, it’s evident that we need to have a DB Schema Sequence ID or UUID -  Lets call this a DSID, so be consistent and save my meat sticks.

1. Store the DSID as a CRDT counter that’s in a protected table in the “system tables” of the db itself .

2. This DSID is embedded into all data going through the servers up to the clients and back to the servers.

3. We track what DSID the data maps to and then use the underlying db schema and json schema to re map it on the fly.

The data on screen is 2D, 3D and 4D ( video streams that are composed of 3d and 2d ). The 4d is the element of time.

### What DB ?

Corrosion DB is SQLITE based and has the right mix to enable this NAKED CDC.

https://github.com/superfly/corrosion

https://github.com/superfly/corrosion/blob/main/doc/quick-start.md

- Multi master, so can run offline and then sync when the network is up, thanks to CRDT.
- Multi version, so that DB Migrations can happen, and not break Multi Master. 
- SQL Queries that are subscriptions, so we do not need a complex CQRS code base.
- DB Data CDC
- DB Schema CDC

POC: https://github.com/jeromegn


## Data Schema Providers

Hook up using the Provider pattern for other storage Archetypes.

Candidates are:

AnyQuery ( https://github.com/julien040/anyquery )

Conduit ( https://github.com/ConduitIO ) 

Peerdb ( https://github.com/PeerDB-io/peerdb )

## Data Sync across forms

Real time, matching of the underlying DB to the GUI is a work in progress.

Then we can built providers for different Storage systems and reflect off them at runtime; Schema CDC essentially.

Data CDC will be added using SSE and NATS, so that all users screens update in real time as one users changes a field.

A Real Time Collaborative example is the Todo demo at https://data-star.dev, but without all the code, https://github.com/delaneyj/datastar/blob/main/backends/go/site/routes_home.go, because Naked HTMX is reflective and so will not need all that code.


## Generation or Refection at Design Time or Runtime ?

Take your pick..

This is 100% reflection based, and so can be changed at runtime.

Generate the JSONSchema at Runtime or Design time ( aka code gen ) as the DB structure changes too, which is nice.

You can also move the "compute to the data" at runtime with WASM, using Compiler as a Service patterns.

This will run just fine as WASM on the Server or Clients, allowing HTMX to be used in either place.
Feed your DB schema changes as JSON to the WASM GUI, or some other thing you want.

Kind of almost a Low code, no code platform then. 

## CQRS / Event Sourced

Because the GUI is a direct representation of the Data, you may want to use this in a CQRS style Architecture, where the Read and Write side have different data structures.

In which case, the **NAKED HTMX** GUI is using the READ side DB, with changes send to the Write side as an Event.

This allows Event sourced Architectures with the Mutations ( the Write side of CQRS ) being described by JSON Schema, and the View ( the read side of the CQRS ) being also described by JSON Schema. 

You could, for example, use this with Pocketbase. Pocketbase is a Read side CQRS (NewSQL de-normalized structure).
It itself fire events, that could be directed into any Event Transform system.

Events feeding into Benthos is a good way to do this at runtime, as is NATS Jetstream. I use both in combination.

https://docs.redpanda.com/redpanda-connect/components/processors/json_schema/

https://github.com/nats-io/jsm.go/blob/main/schema_source/definitions.json

This gives you an OLTP and OLAP in one system also. You can do pretty much any sort of Project with it.


```json
{
    "$id": "https://example.com/person.schema.json",
    "$schema": "http://json-schema.org/draft-07/schema#",
    "title": "Person",
    "type": "object",
    "properties": {
      "firstName": {
        "type": "string",
        "description": "The person's first name."
      },
      "lastName": {
        "type": "string",
        "description": "The person's last name."
      },
      "age": {
        "description": "Age in years which must be equal to or greater than zero.",
        "type": "integer",
        "minimum": 0
      }
    }
}
```

## Deno, Bun, Nodejs

Will wrap with **deno2** for JS developers since you will not need golang to run and change it, since the system is self reflective. 

## NATS Evolution

Enable to reflect off NATS and configure NATS at runtime based on your Event and DB structure, allowing real time stream evolution over SQL migrations.

## Config

A system like this really wants a reactive parametric configuration system.

**koanf** has the underlying design to do this with NATS.

https://github.com/knadh/koanf/issues/310#issuecomment-2382158165 





