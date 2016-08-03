---
layout: post
title: "One cool trick to inspect live objects in your Django app with a standard Python function"
date: 2016-08-02 23:12:00 -0500
tags: python django
byline:
    name: "Cody Winchester"
    twitter: "cody_winchester"
---

{% include byline.html %}

Scenario: I'm writing unit tests for an API via the [Django Rest Framework](http://www.django-rest-framework.org/). (Which, wooooo tests.)

For ~reasons~, one of the test functions needs to loop over a list of the app's model classes -- but only the ones that get imported into `views.py` for inclusion in a [viewset](http://www.django-rest-framework.org/api-guide/viewsets/).

Why not just hard-code the list into the test case, I hear you asking. Sure, I could do that, I've done worse things in my life.* But then I found the Python standard library's [`inspect`](https://docs.python.org/3.4/library/inspect.html) function.

Some just _furious_ Googling and a bit of shell-tinkering later, I had a two-step process outlined:
<ol>
    <li>Using <code><a href="https://docs.python.org/3.4/library/inspect.html#inspect.getmembers" target="_blank">inspect.getmembers()</a></code>, fetch a list of objects in <i>my_cool_app/views.py</i>, passing <code><a href="https://docs.python.org/3.4/library/inspect.html#inspect.isclass" target="_blank">inspect.isclass</a></code> as the predicate argument to return only classes.</li>
    <li>Filter out anything that isn't an instance of the <code>django.db.models.base.ModelBase</code> class. (I used a list comprehension.)</li>
</ol>

Off to the races:

```python
# from tests.py
import my_cool_app.views as mcp_views
import inspect
from django.db.models.base import ModelBase as django_mb

# inspect.getmembers() returns a tuple --> we want [1]
models_to_test = [x[1] for x in inspect.getmembers(mcp_views, inspect.isclass) if isinstance(x[1], django_mb)]

# bang bang, y'all
for my_cool_model in models_to_test:
    qs = my_cool_model.objects.all()
    # test test test
    # test test test
    # test? test!
    # (spoiler, tests all passed)
```

Further reading:

- [https://docs.python.org/3.4/library/inspect.html](https://docs.python.org/3.4/library/inspect.html)
- [https://pymotw.com/2/inspect/](https://docs.python.org/3.4/library/inspect.html)

<p class="footnote">*You have <i>no idea</i>.</p>