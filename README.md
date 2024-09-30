# Naked Objects

This is a golang, HTMX based imlemenation of Naked Objects - https://en.wikipedia.org/wiki/Naked_objects

Using HTMX, this package uses JSON Schema to describe your data structure and validation rules, and JSON Schema to describe the GUI, and then render the WebGUI and do data validation using HTMX.

This means that you can reflect off your underlying database ( or any store ) to, at runtime, have a Web GUI. The JSON Schema and GUI JSON SCHEMA being generated at runtime of sensible reflection.

So when your DB structure changes, the GUI changes with it automatically.

## CQRS / Event Sourced

Because the GUI is a direct representation of the Data, you may want to use this in a CQRS style Architecture, where the Read and Write side have different data structures.  

This allows Event sourced Architectures with the Mutations ( the Write side of CQRS ) being described by JSON Schema, and the View ( the read side of the CQRS ) being also described by JSON Schema. 

Benthos is a good way to do this, as is NATS Jetstream. 

https://docs.redpanda.com/redpanda-connect/components/processors/json_schema/

https://github.com/nats-io/jsm.go/blob/main/schema_source/definitions.json



