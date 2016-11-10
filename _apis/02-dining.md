---
title: Dining
---
The dining data can be found here:

    https://rumobile.rutgers.edu/2/rutgers-dining.txt

This is the same file which is loaded by the Rutgers Mobile Application.  It is
updated every couple of hours.  Dining Services has a public API which provides
the foods for one meal for one location; we simply pull all of the meals for all
of the locations and cat them into one big file.

We unfortunately do not have the ability to query different days or query for
nutritional data at this time.

{% highlight json %}
{
    "location_name": "Brower Commons",
    "date": 1478801653785,
    "meals": [
        {
            "meal_name": "Breakfast",
            "meal_avail": true,
            "genres": [
                {
                    "genre_name": "Breakfast Meats",
                    "items": [
                        "Grilled Turkey Sausage Links",
                        "Pork Roll",
                        "Vegan Breakfast Patties"
                    ]
                },
                ...
            ]
        },
        ...
{% endhighlight %}
