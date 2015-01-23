---
title: Places
---
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
