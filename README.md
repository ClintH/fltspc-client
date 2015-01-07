This repository shows you how to programmatically interact with [ftspc](http://fltspc.itu.dk).

There are two techniques: widget and REST.

# Widget
The first is to have your code run within the fltspc client application (a Javascript-based app, executed in user's browser), the *widget* approach. In this approach, the fltspc app takes care of a lot of the work for you, and interacts with the server. This approach is particularly useful if it makes sense for your app to "live" within a widget. The widget technique works by loading your code into a contained execution environment (an IFRAME) hosted within a visual widget. You can load in what ever Javascript libraries you like and make network calls to other services, and fltspc offers some possibility of interaction via a message passing system, necessary because of browser security restraints. This approach is still very powerful - a fully-editable Google Docs document can be loaded within a widget, for example, or a playable YouTube video.

[Read more about the widget approach](widgets.md). Please see the [basic demo](demos/widget-basic.html) and [advanced demo](widget-advanced.html) as well. 

# REST
The second is the *REST* approach, making HTTP REST-style calls directly to the server. The benefit of this approach is that you can interact with fltspc from practically any programming language or platform. The downside is that you have to take care some house keeping yourself, such as maintaining a user's session.

At present, the system is its own best documentation. If you open Chrome developer tools, watch the Network tab and filter for XHR traffic. It'll be plain to see what requests are sent and what responses are received. If you have any questions, please get in touch.

People have successfully used fltspc's REST APIs from Java Android apps, Tessel microprocessors and other Javascript environments. 



[Read more about the REST API](rest.md)