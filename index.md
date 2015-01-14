---
layout: default
navs:
  - Nextbus
  - Events
  - Dining
  - Sports
  - Recreation
  - Places
  - Schedule of Classes
---

#Nextbus

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

#Events

The full guide for using the RuEvents feeds can be found
[here](http://ruevents.rutgers.edu/events/docs/RSS-Service-Documentation.pdf).
Basically, you make queries to

    http://ruevents.rutgers.edu/events/getEventsRss.xml

and you will receive RSS.  You can pass options on the query string, like so:

    http://ruevents.rutgers.edu/events/getEventsRss.xml?numberOfDays=7

to retreive events within 7 days.  All query parameters and return values are
fully documented in the pdf file above.


#Dining

The dining data can be found here:

    https://rumobile.rutgers.edu/1/rutgers-dining.txt

This is the same file which is loaded by the Rutgers Mobile Application.  It is
updated every couple of hours.  Dining Services has a public API which provides
the foods for one meal for one location; we simply pull all of the meals for all
of the locations and cat them into one big file.

We unfortunately do not have the ability to query different days or query for
nutritional data at this time.

#Sports

The scores feed shows scores for all games:

    http://scarletknights.com//rss/mobile/feed-scores.asp

These feeds can be queried based on sport ID:

Rosters:

    http://scarletknights.com/rss/mobile/feed-roster.asp?sportid=$ID

News:

    http://www.scarletknights.com/rss/rss.asp?sportid=$ID

Sport IDs:

<table class="table">
  <thead>
      <tr>
        <th>Name</th>
        <th>ID</th>
      </tr>
  </thead>
  <tbody>
      <tr>
        <td>Baseball</td>
        <td>19</td>
      </tr>
      <tr>
        <td>Basketball - Men</td>
        <td>11</td>
      </tr>
      <tr>
        <td>Basketball - Women</td>
        <td>12</td>
      </tr>
      <tr>
        <td>Crew - Women</td>
        <td>24</td>
      </tr>
      <tr>
        <td>Cross Country</td>
        <td>6</td>
      </tr>
      <tr>
        <td>Field Hockey</td>
        <td>4</td>
      </tr>
      <tr>
        <td>Football</td>
        <td>1</td>
      </tr>
      <tr>
        <td>Golf - Men</td>
        <td>7</td>
      </tr>
      <tr>
        <td>Golf - Women</td>
        <td>8</td>
      </tr>
      <tr>
        <td>Gymnastics</td>
        <td>14</td>
      </tr>
      <tr>
        <td>Lacrosse - Men</td>
        <td>21</td>
      </tr>
      <tr>
        <td>Lacrosse - Women</td>
        <td>22</td>
      </tr>
      <tr>
        <td>Soccer - Men</td>
        <td>2</td>
      </tr>
      <tr>
        <td>Soccer - Women</td>
        <td>3</td>
      </tr>
      <tr>
        <td>Softball</td>
        <td>20</td>
      </tr>
      <tr>
        <td>Swimming & Diving</td>
        <td>15</td>
      </tr>
      <tr>
        <td>Tennis - Women</td>
        <td>10</td>
      </tr>
      <tr>
        <td>Track & Field - Men</td>
        <td>16</td>
      </tr>
      <tr>
        <td>Track & Field - Women</td>
        <td>17</td>
      </tr>
      <tr>
        <td>Volleyball - Women</td>
        <td>5</td>
      </tr>
      <tr>
        <td>Wrestling</td>
        <td>18</td>
      </tr>
  </tbody>
</table>

#Recreation

We have hours of operation and some general information about the recreation
facilities about Rutgers. It's available in json format at the following url:

    https://rumobile.rutgers.edu/1/gyms.txt

The data will look something like this:

{% highlight json %}
{
"Busch Campus": {
    "Sonny Werblin Recreation Center": {
        "FacilityAddress": "656 Bartholomew Road, Piscataway, NJ 08854",
        "FacilityBusiness": "732-445-0462",
        "FacilityInformation": "732-445-0460",
        "FacilityBody": "The Sonny Werblin Recreation Center is a state-of-the-art recreation facility. Located on the Busch Campus, the Center is a 150,000 square foot facility dedicated to the recreational sports and fitness needs of the student body. Imagine a full ... to Route #287 or Route #1. Follow the respective directions for those roads.  \n\n",
        "FacilityBrief": "3 Basketball / Volleyball/ Badminton Courts\n6 Racquet Ball Courts\n2 Squash Courts\n50 Meter Olympic Pool\n2 Lap pools\nFitness Center\n3 Outdoor Sand Volleyball courts",
        "meetingareas": {
            "Fitness Center": {
                "2/27/2013": "7:00AM - 11:00PM",
                "2/28/2013": "6:00AM - 11:00PM",

                ...

                "4/5/2013": "7:00AM - 8:30PM",
                "4/6/2013": "10:30AM - 6:00PM"
            },
            "Multisports Bay 1 (Badminton)": {
                "2/27/2013": "8:30AM - 11:30PM",
                "2/28/2013": "8:30AM - 8:30PM",

                ...
            },

            ...

{% endhighlight %}


#Places

This API can give you a list of all of the buildings we have data at Rutgers.
You can also query for buildings by buildingno.

The whole database, along with a lunr index (javascript full text search) and
kd-tree (for doing nearby queries) can be found here:

    https://vanguard.rutgers.edu/~rfranknj/mobile/1/places.txt

It's a JSON object with a `lunr` property, a `kdree` property and an `all`
property. The indexer and the code used to utilize the database from within the
app [can be found here](https://github.com/oss/placesindex). The data looks
something like this:

{% highlight json %}
[
{
title: "Douglass Campus Center",
...
campus_name: "Douglass",
location: {
  name: "Douglass Campus Center",
  street: "100 GEORGE STREET",
  city: "New Brunswick",
  state: "New Jersey",
  state_abbr: "NJ",
  postal_code: "08901-1412",
  country: "United States",
  country_abbr: "US",
  latitude: "40.484707",
  longitude: "-74.436640"
},
offices: ["SA-Student Affairs, VP", "SA-Student Life"]
},
...
{% endhighlight %}

You can also pull the latest data for a particular buildingno from:

    http://rumaps.rutgers.edu/bldgnum/$BUILDING_NO/json

#Schedule of Classes

The SOC API allows you to grab a list of courses for a subject / campus /
semester / level.  You can request a key for this API
[here](http://sauron.rutgers.edu/~rfranknj/soc/key.php). After requesting a key,
you can hit the API like this:

    http://sauron.rutgers.edu/~rfranknj/soc/api.php?key=YOURKEY&semester=92013&subj=830&campus=NB&level=U

Semesters are specified as two numbers concatenated together; the first is the
month during which that semester starts (9 = September = fall semester) and the
second is the year. Level can be U or G. Campus can be one of

- New Brunswick: NB
- Newark: NK
- Camden: CM
- Online Courses: ONLINE
- Freehold WMHEC / RU-BCC: WM
- Mays Landing / RU-ACCC: AC
- Denville / RU-Morris: MC
- McGuire-Dix-Lakehurst / RU-JBMDL: J
- North Branch / RU-RVCC: RV
- Camden County College: CC
- Cumberland County College: CU


Example output:

{% highlight json %}
[
{
title: "INTRO COMPUTER SCI",
synopsisUrl: "http://www.cs.rutgers.edu/undergraduate/courses/",
courseNotes: null,
campusCode: "NB",
courseNumber: "111",
sections: [
  {
    index: "37452",
    number: "61",
    campusCode: "NB",
    openStatus: false,
    meetingTimes: [
      {
        campusAbbrev: "BUS",
        startTime: "0500",
        meetingDay: "T",
        roomNumber: "117",
        meetingModeDesc: "LEC",
        campusName: "BUSCH",
        ...
        endTime: "0620"
      },
      {
        campusAbbrev: "BUS",
        startTime: "0500",
        meetingDay: "TH",
        roomNumber: "117",
...
{% endhighlight %}

##Autocompletion Code

The code that runs the autocomplete for SOC in the mobile app is open source.
[here](https://github.com/oss/socindex). It does intelligent autocompletion;
recognizing abbreviations and course numbers, as well as doing naive fulltext
search.

If you need help with any of the above APIs:

<a href="https://twitter.com/intent/tweet?screen_name=russjf" class="twitter-mention-button" data-size="large" data-related="russjf">Tweet to @russjf</a>
<script>!function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0];if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src="//platform.twitter.com/widgets.js";fjs.parentNode.insertBefore(js,fjs);}}(document,"script","twitter-wjs");</script>
