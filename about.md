---
layout: page
title: About
permalink: /about/
---

{% for human in site.data.peeps %}
### {{ human.name }}
{{ human.title }}

{% include icon-twitter.html username=human.twitter %}

{% include icon-github.html username=human.github %}

----
{% endfor %}
