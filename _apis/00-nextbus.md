---
title: Nextbus
---

###Nextbus Public API

Nextbus has an API which allows you to query for stops, routes, and
vehicle locations.  The base URL is:

    http://webservices.nextbus.com/service/publicXMLFeed?a=rutgers&command=$COMMAND

The return format is XML. Possible commands are:

<table class="table">
  <thead>
      <tr>
        <th>Command</th>
        <th>Possible parameters</th>
        <th>Description</th>
      </tr>
  </thead>
  <tbody>
      <tr>
        <td>routeConfig</td>
        <td></td>
        <td>Returns the list of all routes and stops and the lat/lon of
        each stop.</td>
      </tr>
      <tr>
        <td>predictions</td>
        <td>stopId or r (route) or s (stop), optional d (direction)</td>
        <td>Returns predictions for a particular route or stop, optionally
        returning results only for a particular direction.</td>
      </tr>
      <tr>
        <td>predictionsForMultiStops</td>
        <td>multiple 'stops' parameters, these are: routeTag|dirTag|stopId</td>
        <td>Returns predictions for a set of route/stop combinations. Again
        direction is optional and can be null.</td>
      </tr>
      <tr>
        <td>vehicleLocations</td>
        <td>t, time of the last vehicle locations request</td>
        <td>Returns a list of vehicles and their locations.  You can specify
        the t parameter, which will only show you vehicles which have moved
        since t.</td>
      </tr>
  </tbody>
</table>

More complete documentation is available
[here](http://www.sfmta.com/cms/asite/nextmunidata.htm)
Unfortunately, there are some problems with this API; for example, if one
would like all buses which stop at the Hill Center, one would either have
to run one query for each of Hill's three stop IDs or build a
predictionsForMultiStops query.

The code in the Rutgers Mobile App for handling this case is open source:
[http://github.com/rf/nextbusjs](http://github.com/rf/nextbusjs),
however, for those of you who are scared of javascript, an API based on
this library is available for the duration of the hackathon.

###Sugabus API

Base url:

    http://runextbus.heroku.com/

The return format is JSON. Queries:

<table class="table">
  <thead>
      <tr>
        <th>URL</th>
        <th>Parameters</th>
        <th>Description</th>
      </tr>
  </thead>
  <tr>
      <td>/route/$ROUTE</td>
      <td>$ROUTE is a route tag</td>
      <td>List of predictions for the requested route</td>
  </tr>
  <tr>
      <td>/stop/$STOP</td>
      <td>$STOP is a stop tag OR stop title</td>
      <td>List of predictions for the requested stop</td>
  </tr>
  <tr>
      <td>/active</td>
      <td></td>
      <td>List of active routes and stops (ie, buses are running)</td>
  </tr>
  <tr>
      <td>/nearby/$LAT/$LON</td>
      <td>$LAT and $LON are latitude and longitutde</td>
      <td>Nearby stops to the given lat lon</td>
  </tr>
  <tr>
      <td>/locations</td>
      <td></td>
      <td>Vehicle locations.  This always gets the vehicles which have moved in the last 15 minutes.</td>
  </tr>
  <tr>
      <td>/config</td>
      <td></td>
      <td>Retrieves the 'agency cache'.  This contains a list of all routes and stops, as well as a sorted
      array of routes and stops for display in the app.</td>
  </tr>
</table>

This is also [open source](http://github.com/russfrank/sugabus). It's a pretty
straightforward flatiron/node.js app written in CoffeeScript.

For a list of routes and stops, do a /config query; you'll get a big json object.  It will
have properties 'sortedStops' and 'sortedRoutes'.

The responses come straight out of nextbusjs, so see the
[nextbusjs wiki](https://github.com/russfrank/nextbusjs/wiki)
for more information.

###Nextbusjs

If you're working in Node.js or Appcelerator Titanium, you can use nextbusjs
directly. It will need a route configuration, which you can cache yourself,
or pull the one we use for mobile:

    https://rumobile.rutgers.edu/1/rutgersrouteconfig.txt

Here's an example in CoffeeScript:

{% highlight coffeescript %}
request = require 'request'
nextbus = require 'nextbus'
rutgers = nextbus.client()
request 'https://rumobile.rutgers.edu/1/rutgersrouteconfig.txt', (err, response, body) ->
  if err then return console.dir err
  rutgers.setAgencyCache (JSON.parse body), 'rutgers'
{% endhighlight %}

After doing the above, you can call the prediction methods on the rutgers
object. See the [nextbusjs wiki](https://github.com/russfrank/nextbusjs/wiki)
for more information.
