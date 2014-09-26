The XContentReady Event
=======================

**`XContentReady`** A custom DOM Event to notify robotic page renderers that the relevant content of a page is completely materialized in the HTML code. A renderer may listen for this event to know when it can scrape a page.

Why?
----

The HTML [Single-Page Application](http://en.wikipedia.org/wiki/Single-page_application) architecture is getting more popular and not only replacements for native apps are built on this principle but also applications that serve the purpose of classical Web sites. For public Web sites it is of uttermost importance that Web crawlers from search engines (Yandex, Bing, Facebook, Amazon, Google etc.) are able to get a static HTML representation of each page containing all textual content. The problem is that such pages tend to be mashups of different resources which are loaded asynchronously by the application code. This is why the rendering engine on the server which is often a headless browser like [PhantomJS](http://phantomjs.org/) is not able to deterministically decide when the page has completely been loaded with this respect. The page loading speed is to be kept as low as possible as it influences the page ranking; so waiting some seconds is not a sensible option.
Besides simple timeout based solutions, there are adventuresome approaches like tracking the included scripts and deciding that the content is ready once they are loaded but non of them can be really deterministic.

Usage
-----

### Consume it

Just add an event on the server side rendering engine listener like so:

````
document.addEventListener('XContentReady', function() {
  console.log('ready')
});
````

### Emit it

Instantiate a custom DOM event and emit it in the application like so,
once the application knows that the corresponding state as described above is reached:

````
var e = new CustomEvent('XContentReady');
document.dispatchEvent(e);
````

Note on PhatomJS:
As of version 1.9.7 the method above is not supported. Use the old way of creating a custom event as long as `CustomEvent()` is not supported:

````
var e = document.createEvent('Event');
e.initEvent('XContentReady', true, false);
document.dispatchEvent(e);
````

Name
----

Its name has been derived from the standardized [DOMContentLoaded](https://developer.mozilla.org/en-US/docs/Web/Reference/Events/DOMContentLoaded) Event which is based on the included resources like scripts of a page. The `X` should clearly mark it as a custom event.

Status
------

This event is not standard nor a W3C recommendation, it is just a proposed best practice.
But who knows, proved ideas with broad adoption tend to become standards in the IT.

Compatibility
-------------

- Ember JS
- Angular JS
- etc.

Contribute
----------

Emit and consume the event as you need and list yourself as adopter here:

- [Invend](www.invend.eu)
- [Ember Prerender](https://github.com/zipfworks/ember-prerender)

License
-------

This stuff is [Public Domain](http://wiki.creativecommons.org/CC0)

