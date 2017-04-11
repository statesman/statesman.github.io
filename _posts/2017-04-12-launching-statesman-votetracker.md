---
layout: post
title: "Lessons learned working on Statesman VoteTracker"
date: 2017-04-12 10:00:00 -0500
categories: votetracker
byline:
    - name: "Christian McDonald"
      twitter: "crit"
---

![VoteTracker](/assets/img/2017-04-12-votetracker-top.png)

Last week, after a 3-year journey, the Statesman news apps team launched [VoteTracker](http://apps.statesman.com/votetracker/entities/austin-city-council/), a look into the voting history of Austin City Council members on some of the more important issues that cross the dais.

[Lilly Rockwell](https://twitter.com/LillyRockwell), then a city hall reporter, first approached the news apps team with the idea for VoteTracker some time in 2014 as the first 10-1 council was up for election. There were no "data" on agenda items or votes then, and it isn't much different today. The city now posts [agenda items on their data portal](https://data.austintexas.gov/Government/Austin-City-Council-Agenda-Items/es7e-878h/data), but the descriptions are still obtuse and there are no follow-up data on voting. The only vote record is posted in a PDF word doc in minutes approved at the following meeting.

In the three years since, we worked on VoteTracker more off than on. We created a "minimum viable product" admin and then abandoned it. We built and API back end with plans to develop a Javascript framework frontend, and then abandonded it. In the end, we settled on a tried-and-true method for us: A [django](https://www.djangoproject.com/) admin to manage data, and L.A. Times' [django-bakery](https://github.com/datadesk/django-bakery)-based views to publish static pages to Amazon S3. 


### Where's the data?

Without a data source of outcomes, building a tracker of all votes at City Hall is a dubious challenge. OK, buidling it is not the problem ... maintaining it is. Even with a data source of agenda items, we don't have the manpower (or interest, frankly) to enhance each item with every discussion, amendment and vote in a typical 100-item agenda.

We decided to concentrate only on the newsworthy agenda items we typically write about on the City Hall beat, making VoteTracker more of a browsing experience than a database of record.

Hopefully, as we use VoteTracker more, we can use it as an endpoint for our coverage, allowing reporters to spend more time on specific issues that matter to most people, while at the same time offering more coverage of things we haven’t been able to spend time, ink or pixels on in the past.

Perhaps some day the city will advance their record keeping and we could use something like [Councilmatic](https://www.councilmatic.org/) in the future.

### Using django and django-bakery for VoteTracker

#### Andrew's first commit on what is now VoteTracker.

![First commit VoteTracker](/assets/img/2017-04-12-votetracker-django-first-commit.png)

#### Cody's last commit on VoteTracker before heading for IRE

![Cody's last commit](/assets/img/2017-04-12-votetracker-last-cody.png)

#### Dan's last push before launch

![Launch by Dan](/assets/img/2017-04-12-votetracker-launch.png)


Back in 2015, we were pretty excited to set up Django for a number of reasons, and VoteTracker was one of the first things Andrew started to work on in what has come to be our "Data Warehouse." We had just spent four months an a VoteTracker admin that wasn't near ready, and we were interested in using the "free" admin that comes with Django to turn projects around quicker and better.

Unfortunely (or perhaps fortuitively) we got sidetracked from VoteTracker onto other things and built up Django experience with less complex projects. We used the admin to track bikers involved the Waco shootout, some election results and traffic fatalities. Andrew left, and [Cody Winchester](https://twitter.com/cody_winchester/status/688049261666160640) joined us in early 2016. Because we were working in Django and not a bespoke project, Cody was able to pick up VoteTracker where it was left off.

We also wanted to take advantage of the Django views, templates and ORM for our data projects, but I did not want the add the sysops responsiblity of an always-live database to a two-person team. That's what got us looking at [django-bakery](http://django-bakery.readthedocs.io/en/latest/), which Andrew used on our first big project using baked pages, our restaurant guide [Austin360Eats](http://apps.statesman.com/austin360/eats/).

This concept of building views in Django and then baking the results to S3 lets us sleep a little easier at night. Once those pages are pushed out to Amazon, they are done. They won't break. And if Amazon goes down, we are all screwed. [Wait, that did happen](https://www.recode.net/2017/3/2/14792636/amazon-aws-internet-outage-cause-human-error-incorrect-command).

### Front-end user testing

As the 2016 election passed and the end of the year approached, we started to feel real pressure to launch. But, we had been developing the user-facing part of VoteTracker in a near vacuum.

To get some valuable feedback, [Dan Hill](http://twitter.com/danhillreports) worked on a user testing script that we used with different folks in the newsroom to get an idea of what worked and what was confusing. It helped us simplify and arrange some things on the front end, though we still hope to iterate as we get more feedback from our readers.

### Nothing beats actual use

Even though we tested the admin, we got the best feedback once the current council members took office and started meeting in 2017. That's when [Elizabeth Findell](https://twitter.com/efindell) and [Nolan Hicks](https://twitter.com/ndhapple) began using the admin regularly, which brought on more tweaks and feature requests. Using our exising Django framework, Cody could usually turn those around fairly quickly, just as Andrew and I had dreamed back in 2015. He finished the baking features just before he [left for a new job at IRE](https://twitter.com/cody_winchester/status/844930510698745856) at the end of March.

### Other learnings

It's been such a long road, I forget what we learned on this vs other projects, but here are some things we used or did:

- VoteTracker was the first time we used Balsamiq to sketch out a user interface and get feedback. It was helpful, but I put too much detail into early sketches, and didn't actuall sit with people to use them.
- If this wasn't the first project we used user-testing scripts on, it was close to it. The feedback we got even from our own newsroom was invaluable. We need to get that testing to more folks in development, me thinks.
- We are thinking we take django-bakery pretty far with related models. Cody developed a way to use pass save trigger keywords back and forth between models to keep us from getting caught in an endless loop of re-baking. This isn't the first project we've used on it, but it definitely came in handy.
- The Atlantic's [django-nested-admin](https://github.com/theatlantic/django-nested-admin) was another module we found invaluable in this project as way to handle our model relationships. (A council member is member of an entity during a time a vote is made on an agenda item which is part of a meeting. Got that?).
- Nothing beats an editor who won't quit and won't take "No" for an answer. Thank you, [Bridget Grumet](https://twitter.com/bgrumet).

![First draft VoteTracker on mobile](/assets/img/2017-04-12-votetracker-mock.png)
<small>My first draft of mobile front end for VoteTracker from May 2016.</small>
