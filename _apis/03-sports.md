---
title: Sports
---
Query games played with the Rutgers Athletics API.

    https://rumobile.rutgers.edu/2/sports/{sport}.json

Example for baseball:

{% highlight json %}
{

    "description": "Schedule of Rutgers Baseball Games",
    "games": [
        {
            "description": "Rutgers at Miami (Fla.)",
            "isEvent": false,
            "home": {
                "name": "Miami (Fla.)",
                "code": "mifl",
                "score": 4
            },
            "away": {
                "name": "Rutgers",
                "code": "rutu",
                "score": 6
            },
            "start": {
                "date": "2017-02-17T05:00:00.000Z",
                "time": false,
                "timeString": "TBA"
            },
            "location": "Coral Gables, Fla."
        },
    ...
{% endhighlight %}
