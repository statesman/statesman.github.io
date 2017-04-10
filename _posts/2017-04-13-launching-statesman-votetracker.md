---
layout: post
title: "Launching the Statesman VoteTracker, a history lesson"
date: 2017-04-10 10:00:00 -0500
categories: votetracker
byline:
    - name: "Christian McDonald"
      twitter: "crit"
---

![First draft VoteTracker on mobile](/assets/img/2017-04-13-votetracker-mock.png)
<small>First draft of mobile front end for VoteTracker from May 2016. (Christian McDonald)</small>

Last week, after a 3-year journey, the Statesman news apps team launched [VoteTracker](http://apps.statesman.com/votetracker/entities/austin-city-council/), a look into the voting history of Austin City Council members on some of the more important issues that cross the dias.

Then city hall reporter [Lilly Rockwell](https://twitter.com/LillyRockwell) first approached the news apps team some time in 2014 as the first 10-1 council was up for election. She suggested it could be useful for both reporters and the public to browse voting records, as there was (and still isn't) a way to see voting history beyond going through the approved minutes of each meeting.

It wasn't an easy thing to build at the time because there was no "data". There are dozens of agenda items at each council meeting, no vote board or anything to keep track of during the meeting, and the minutes are not in data format. While the city now posts [agenda items on their data portal](https://data.austintexas.gov/Government/Austin-City-Council-Agenda-Items/es7e-878h/data), the descriptions are still obtuse and there isn't any follow-up data. 

### The first admin

We really wanted to get this going by the time the first 10-1 council was seated, so [Andrew Chavez]() worked on a "minimum viable product" as an Express/Node app deployed on Heroku. The first commit was Nov. 21st 2014. It was just an admin to record the votes, because we didn't need to show anything to the public until we had some history.

As Lilly and Andra Lim entered votes they found tweaks and features that needed to be added. At the same time, Andrew and I had been discussing using [Django](https://www.djangoproject.com/) for such projects so we wouldn't have to reinvent an admin for each new thing. If we switched to Django we could develop features faster and take advantage of the APIs as we looked forward to a public-facing app.

So, 101 commits and 10 issues closed, the last commit was Feb. 17, 2015.

### Data warehouse admin

![First draft VoteTracker on mobile](/assets/img/2017-04-13-votetracker-django-first-commit.png)

We were pretty excited to set up Django for a number of reasons, and VoteTracker was one of the first things Andrew started to work on in April 2015 in what has come to be our "Data Warehouse."

But with the council now months into existence, we were starting over on the back end and there was no public-facing site. We got sidetracked by news events and reporters grew weary of keeping up with the votes with no work to show for it.

With everyone losing interest in VoteTracker, we rocked along on Data Warehouse, using it as a data reporting admin to track [bikers involved a shootout in Waco](http://www.statesman.com/news/local/waco-biker-gang-shootout-involved-bandidos-and-cossacks-sheriff-says/RZM3mj877eOg24z6rUf10L/) and for some election results. That was enough to get us looking at the L.A. Times' [django-bakery](https://github.com/datadesk/django-bakery), which Andrew used on our first big project using baked pages, our restaurant guide [Austin360Eats](http://apps.statesman.com/austin360/eats/).

The death knell for VoteTracker was when Andrew left in November 2015 to [join the Dallas Morning News](https://twitter.com/adchavez/status/667128135595388929). Lilly left the city hall team to join [512Tech](http://www.512tech.com/) in January 2016 and Andra followed soon after.

### Languishment and rebirth

We tried to inject life into VoteTracker every now and again, submitting it as a project for the Knight News Challenge in late 2015. We didn't win. In early 2016, assistant city editor [Bridget Grumet](https://twitter.com/bgrumet) pitched the project as part of a company contest that would've brought in resources. It didn't win.

New council elections in 2016 brought renewed interest, and with the addition of [Cody Winchester](https://twitter.com/cody_winchester/status/688049261666160640) in February to the news apps team, we set it as a priority in second half of 2016.

We also hired new city hall reporters ([Elizabeth Findell](https://twitter.com/efindell) and [Nolan Hicks](https://twitter.com/ndhapple)) who had to be brought up to speed, and frankly, to buy into the project.

Cody worked and reworked the admin for quite a while because the models were not simple and the admin became quite nested and complex. He would take his laptop to a council meeting and follow along and record votes, file issues and then iterate.

He would deploy changes and have Elizabeth and Nolan try them out. They had lots of great feedback. We would be nowhere without them.

At the same time we were still trying to figure out the front end. In May 2016, I had done mockups of [mobile](/assets/img/2017-04-13-Votetracker-mobile-01.pdf) and [desktop](/assets/img/2017-04-13-votetracker-mock.png) views to start with, but we knew we had to get this in the hands of folks.

In June, Cody started to work on an API layer to the django backend so we could use Backbone or some other Javascript framework to display the data.

But with more and more success using django-bakery on other projects like the [Homicides](http://apps.statesman.com/homicides/) database, we decided to go that route. By this time, we realized we would not be tracking ALL issues, only those that our reporters felt were newsworthy and important, so we didn't have an official data store of all records. Cody was much more familiar with working in Django than any Javascript framework. We went with what we knew: django-bakery.

### Front-end user testing

As the end of the year approached and the election passed, we started to feel real pressure to launch, but we had been developing the user-facing part of VoteTracker in a near vacuum.

So [Dan Hill](http://twitter.com/danhillreports) worked on a user testing script that we used with different folks in the newsroom to get feedback on front end. It helped us simplify and arrange some things on the front end, though we still hope to iterate as we get more feedback from our readers.

### Nothing beats actual use

Even though we tested the admin, we got the best feedback once the current session took office and started meeting. That's when Elizabeth and Nolan began using the admin regularly, which brought on more feature requests and tweaks.

We wanted to wait through several council sessions to collect data before releasing to the public, but we had it running through February and March where we could see the results and tweak them. It wasn't until after SXSW in March that Cody really started the django-bakery parts. 

You see, Cody had to do by March 31st, because that was his last day with us :(. He took a job as a training directory with [Investigative Reporters and Editors](http://ire.org) (which we support and are fans of, so we were both happy and sad.)

We launched to the public on [April 4](http://cityhall.blog.statesman.com/2017/04/04/introducing-votetracker-our-database-of-key-austin-city-council-votes/) and we've gotten some good feedback so far from readers. We have 11 open issues of varying degree of import and challenge.

### Learnings from the process

What did we learn from this? It's been so long, I barely remember what we learned in this project vs others, but some things we used or did:

- VoteTracker was the first time we used Balsamiq to sketch out a user interface and get feedback. It was helpful, but I put too much detail in them to begin with.
- If this wasn't the first project we used user-testing scripts on, it was close to it. The feedback we got even from our own newsroom was invaluable. We need to get that testing to more folks in development, me thinks.
- We take django-bakery pretty far with related models, we are thinking. Cody developed a way to use pass some save trigger keywords back and forth between models to keep us from getting in an endless loop of re-baking. This isn't the first project we've used on it, but it definitely came in handy.
- The Atlantic's [django-nested-admin](https://github.com/theatlantic/django-nested-admin) was another module we found invaluable in this project as way to handle our model relationships. (A council member is member of an entity during a time a vote is made on an agenda item which is part of a meeting. Got that?).
- Nothing beats an editor who won't quit and won't take "No" for an answer. Thank you. Bridget.

