---
title: Events
---
The full guide for using the RuEvents feeds can be found
[here](http://ruevents.rutgers.edu/events/docs/RSS-Service-Documentation.pdf).
Basically, you make queries to

    http://ruevents.rutgers.edu/events/getEventsRss.xml

and you will receive RSS.  You can pass options on the query string, like so:

    http://ruevents.rutgers.edu/events/getEventsRss.xml?numberOfDays=7

to retreive events within 7 days.  All query parameters and return values are
fully documented in the pdf file above.
