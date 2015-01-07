# Extending fltspc with widgets

In essence, you host your Javascript, HTML and CSS in any accessible location, and put the URL in a widget on a surface. When the surface is loaded, and if the widget is not too small, the content will be loaded in an IFRAME.

Being loaded in an IFRAME means that there are security restrictions in how you are able to interact with the parent page and fltspc environment, but otherwise you are able to run and load whatever Javascript you like (eg jQuery, Processing etc).

A little isolated execution environment is made for each widget that loads your script. A single surface, for example, might load your script five times for the five widgets that are set to it's URL. There is no global context, except for the values of the surface and widget which are available from each environment.

If you need to do more with fltspc via embedded JS, please drop me a line.

## Demos
For the demos to work, you need to host them somewhere, and then load the url into a widget.

* [demos/widget-basic.html](Basic) Demonstrates get and edit commands
* [demos/widget-rt.html](Realtime) Demonstrates interaction with fltspc's realtime channel

## Debugging

You'll probably need to reload your widget content as you debug. It's best if you load the same URL in a browser tab so you can force-refresh it. Then in fltspc, select your widget, and select 'reload' from the properties panel. This way you can reload your content without having to reload the whole fltspc app.

## Hosted widget info

### Hello

Approximately 5 seconds after the widget has loaded its content, it will receive a 'hello' message posted from the fltspc app with its widget id. The data takes the form:

```
	{
		fltspc: "hello",
		widget: "WIDGET ID"
	}
```

The delay gives your code a chance to be downloaded and initialise.

This is demonstrated in `demos/widget-basic.html`.

### URL hackery
If your script is loaded with `#fltspc_id` or `#fltspc_surface_id` as part of the URL, these tokens are automatically replaced with the respective widget id and surface ids. Eg, let's say your script is hosted at http://my.com/widget/.

If a widget's contents is set to:

```
http://my.com/widget/?widget=#fltspc_id&surface=#fltspc_surface_id
```

The URL that is *actually* fetched in the browser will be something like:

```
http://my.com/widget/?widget=9asdf9&surface=asdf93f
```

You can do some additional trickery on *your* server side based on this. For example, to return different content depending on the widget and surface. You can also use this feature with plain 'ol static pages, by introspecting the URL your script is loaded on, via `window.location.hash`. Something like this would work with a static HTML file:

```
http://my.com/widget.html##fltspc_id-#fltspc_surface_id
```

Which is transformed into something like:

```
http://my.com/widget.html#9asdf9-asdf93f
```

(You'll obviously have to do some parsing of the URL fragment)


# Get Commands

Commands that return dumps of information. Note that requests are asynchronous, and you need to listen for the result sent back via postMessage. See the `widget-basic` demo for how to set this up.

## getSurface
Returns data on the currently loaded suface, eg its title, background colour etc.

## getSurfaceWidgets
Returns the set of widgets on the currently loaded surface

## getUser
Returns info on the currently logged-in user

## getWidget(id)
Returns info on a single widget

# Edit Commands

Commands that manipulate data. The result of the command (if any) is sent back via postMessage.

## createWidget(widgetAttributes)

Creates a widget based on a given set of attributes.

Returns: created widget's attributes

## deleteWidget(id)

Deletes a widget.

## updateWidget(widgetAttributes)

Updates a widget's attributes. `widgetAttributes` must have an `id` property of the widget to update.

Note: You cannot change the id, surface or owner properties.

## updateSurface(surfaceAttributes)

Update's the currently loaded surface's attributes.

Note: You cannot change the id, permissions or owner of the surface.

# Communication

You can piggy-back off fltspc's realtime communications channel to send and receive packets between devices and widgets. See the `widget-advanced.html` demo.


## sendPacket(data)

Sends a surface packet which is distributed to all (tuned-in) widgets on the same surface.

This happens via the server, so any clients viewing the surface on other devices also receive the packet.

## receivePackets

Sending this command 'tunes in' to packets sent by other widgets.

# Persisting Data

Arbitrary data can be associated with a widget or surface. To do so, use the `updateWidget` or `updateSurface` commands as described above, setting the `meta` attribute. This should be a stringified JSON string. See the `widget-advanced.html` for an example.

Keep in mind that the value might be quite volatile if different processes are setting to it different strings. Always read and write to it with care.

The server will truncate any data above 5KB.