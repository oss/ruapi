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

{% for api in site.apis %}
#{{ api.title }}

{{ api.content }}

{% endfor %}

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
