---
layout: post
title: "Launching the Statesman VoteTracker, a history lesson"
date: 2017-04-13 10:00:00 -0500
categories: votetracker
byline:
    - name: "Christian McDonald"
      twitter: "crit"
---

![First draft VoteTracker on mobile](/assets/img/2017-04-13-votetracker-mock.png)
<small><a href="https://twitter.com/crit" target="_blank">Christian McDonald</a></small>

Last week, after a 3-year journey, the Statesman news apps team release [VoteTracker](http://apps.statesman.com/votetracker/entities/austin-city-council/), a look into the voting history of Austin City Council members on some of the more important issues that cross the dias.

Then city hall reporter [Lilly Rockwell](https://twitter.com/LillyRockwell) first approached the news apps team some time in 2014 as the first 10-1 council was up for election. She suggested it could be useful for both reporters and the public to browse voting records, as there was (and still isn't) a way to see voting history beyond going through the approved minutes of each meeting.

It wasn't an easy thing to build at then. There are dozens of agenda items at each council meeting, no vote board or anything to keep track of during the meeting, and the minutes are not in data format. While the city now posts [agenda items on their data portal](https://data.austintexas.gov/Government/Austin-City-Council-Agenda-Items/es7e-878h/data), the descriptions are still obtuse and there isn't any follow-up data. 

### The first admin

We really wanted to get this going by the time the first 10-1 council was seated, so [Andrew Chavez]() worked on a "minimum viable product" as an Express/Node app deployed on Heroku. The first commit was Nov. 21st 2014. It was just an admin to record the votes, because we didn't need to show anything to the public until we had some history.

As Lilly and Andra Lim entered votes they found tweaks and features that needed to be added. At the same time, Andrew and I had been discussing using [Django](https://www.djangoproject.com/) for such projects so we wouldn't have to reinvent an admin for each new project. If we switched to Django we could develop features faster and take advantage of the APIs as we looked forward to a public-facing app.

So, 101 commits and 10 issues closed, the last commit was Feb. 17, 2015.

### Data warehouse admin

![First draft VoteTracker on mobile](/assets/img/2017-04-13-votetracker-django-first-commit.png)

We were pretty excited to set up Django for a number of reasons, and Votetracker was one of the first things we started to work on in April 2015 in what has come to be "Data Warehouse".

But with the council now months into existence, we were starting over on the back end and there was no public-facing site. We got sidetracked by news events and reporters grew weary of keeping up with the votes with no work to show for it.

With everyone losing interest we concentrated on other projects. We rocked along on Data Warehouse, using it for a data admin to track [bikers involved a shootout in Waco](http://www.statesman.com/news/local/waco-biker-gang-shootout-involved-bandidos-and-cossacks-sheriff-says/RZM3mj877eOg24z6rUf10L/) (which for some reason never saw the light of day) and some election results, but it got us using the L.A. Times' [django-bakery](https://github.com/datadesk/django-bakery). We used it for some election stuff, and our first big project using baked pages Andrew built our restaurant guide [Austin360Eats](http://apps.statesman.com/austin360/eats/).

The death knell for VoteTracker was when Andrew left in November 2015 to join the Dallas Morning News. Lilly left the city hall team in January 2016 and Andra followed soon after.

### Languishment and rebirth

We tried to inject live now and again, submitting it as a project for the Knight News Challenge in late 2015. We didn't win. In early 2016. Editor Bridget Grumet pitched the project as part of a company contest that would've brought in resources. It didn't win.

But new council elections in 2016 brought along new interest, and with the addition of [Cody Winchester](https://twitter.com/cody_winchester) to the news apps team, we set it as a priority in second half of 2016.

Cody had some on in February and started in on Data Warehouse by rebuilding our [Homicides](http://apps.statesman.com/homicides/) database. So he took on the VoteTracker project.

We also hired new city hall reporters (Elizabeth Findell and Nolan Hicks) who had to be brought up to speed, and frankly, to buy into the project.

Cody worked on the admin for quite a while, because the models were not simple and the admin became quite nested and complex and nested. He would take his laptop up to a meeting and follow along. He would deploy changes and have Elizabeth and Nolan try them out.

They had lots of great feedback. We would be nowhwere without them.

At the same time we were still trying to figure out the front end. In May 2016, I had done mockups of [mobile](/assets/img/2017-04-13-Votetracker-mobile-01.pdf) and [desktop](/assets/img/2017-04-13-votetracker-mock.png) views.

In June, Cody started to work on an API layer to the django backend so we could use Backbone or some other Javascript framework to display the data.

But with more and more success using django-bakery on other projects, we decided to go that route. By this time, we realized we would not be tracking ALL issues, only those that our reporters felt were newsworthy and important, so we did't have an official data store of all records. Cody was much more familiar with working in Django than any Javascript framework. We went with what we knew: django-bakery.

### Front-end user testing

As the end of the year approached and the election passed, we started to feel real pressure to launch, but we had been developing the user-facing part of VoteTracker in a near vaccum.

So [Dan Hill]() worked on a user testing script and we showed several iteractions to folks in the newsroom. It helped us simplify and arrange some things on the front end, though we still consider the current deployment a first version.

### Cody's swansong

[More here about launch and the irony of Cody's leaving just before it launched.]


