REST API
========

fltspc's own Javascript-based client interacts with the server via a simple REST API. This means that anything the normal fltspc interface can do, you can do programmatically.

Here we just describe the basics: logging in, getting a list of surfaces, editing a surface, creating/deleting/updating widgets.

# Authentication

## /whoami (GET)

Returns data on the logged in user, with keys such as email, name, id, and lastSurface.

## /api/1/acc/logout (GET)

Logs out of a session. Returns `{ message: "OK" }`

## /api/1/acc/login (POST)

Logs in. Client should supply `email` and `password` data. Response will be data on logged in user.

# Surfaces

## /surfaces (GET)

Returns an array of a user's surfaces, but only each surface's metadata. Each surface is described with attributes such as: owner, shortId, bgColour, height, width, title, id and more.

## /api/1/surface/*SURFACE_ID*/ (GET)

Returns the metadata for surface with id *SURFACE_ID*.

## /api/1/surface/*SURFACE_ID*/widgets (GET)

Returns an array of widgets residing on surface with id *SURFACE_ID*. Each widget is described with attributes such as: owner, surfaceId, flipped, bgColour, size (with height and width sub-attributes), pos (with top and left sub-attributes), content, type, contentBack, title and more.

## /api/1/surface/*SURFACE_ID*/ (PUT)

Updates a surface with supplied data (JSON). Response will be the full data for the widget.

# Widgets

## /api/1/widget/*WIDGET_ID*/ (PUT)

Updates a widget with supplied data (JSON). Response will be the full data for the widget.

Use HTTP method DELETE to delete a widget.

## /api/1/widget/ (POST)

Creates a new widget with supplied data (JSON). Response will be the full data for the new widget, with a server-created `_id` attribute.


