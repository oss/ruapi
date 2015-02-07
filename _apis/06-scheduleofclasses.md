---
title: Schedule of Classes
---
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
