# Naked Objects

This is a golang, HTMX based imlemenation of Naked Objects - https://en.wikipedia.org/wiki/Naked_objects

Using HTMX, this package uses JSON Schema to describe your data structure and validation rules, and JSON Schema to describe the GUI, and then render the WebGUI and do data validation using HTMX.

This means that you can reflect off your underlying database ( or any store ) to, at runtime, have a Web GUI.

So when your DB structure chnages, the GUI changes with it automatically.

