---
layout: post
title:  "Csvkit data pipelines with Bash in Jupyter Notebooks"
date:   2016-07-17 17:00:00 -0500
categories: data-analysis
tags: notebooks
byline:
    - name: "Christian McDonald"
      twitter: "crit"
---

I've been enamored with **[Jupyter Notebooks](http://jupyter.org/)** since I learned about them at the [NICAR conference](http://www.ire.org/conferences/nicar2016/) this year. The idea of transparent, repeatable data analysis for journalism is important to me, and Notebook makes this possible.

Learning about them has also turned me into a fan of **conda** virtual environments, and the [Anaconda](https://www.continuum.io/downloads) and [Miniconda](http://conda.pydata.org/miniconda.html) package solutions.

But I was bummed when I first tried to use [csvkit](https://csvkit.readthedocs.org/)-based data pipeline in a notebook, but I couldn't figure out how to run those csvkit commands, because they are usually used from a Bash command-line. Until I found [jeroen janssens's "iBash Notebook" post](http://jeroenjanssens.com/2015/02/19/ibash-notebook.html), where he talks about [Thomas Kluyver's bash kernel](https://github.com/takluyver/bash_kernel).

With a few quick commands, you can add a Bash option to your Jupyter Notebook. Once you are in your virtual environment, do:

``` bash
$ pip install bash_kernel
$ python -m bash_kernel.install
```

(Of note: I've found this doesn't work well on PC's at all. The Bash Kernel keeps crashing. That said, you still can run Bash in a cell in the Python kernal as noted below.)

Now when you start you Notebook, you should have a Bash option.

![start-notebook]({{ site.url }}/assets/img/notebook-start.png)

I've yet to use Notebooks on a published story yet, but I am for the first time using them [to teach csvkit](https://github.com/utdata/cli-tools/blob/master/lectures/UsingNotebooks.md) to my UT Journalism students.

## Run bash in a Python notebook

When I was working through the [agate](http://agate.readthedocs.io/en/1.4.0/tutorial.html) tutorial, I was frustrated that I could not see the data I was working with. It could be that I just don't know how to do the equivalent of `head` in agate, but I did find another way.

To run **Bash** in a single cell, start it with `%%bash`.

```
%%bash
head exonerations-20150828.csv
```

When I did the agate tutorial, I didn't want to do it in the Python interpreter, I wanted to run it in a notebook so I could take notes and reproduce it. So even though it was a Python notebook, I started it with a `%% bash` command to `curl` the data into my directory. I then ran `head` on it so I could see the first couple of rows, since I wasn't familiar with the data.

I'm sure there is a better way to do this in agate, but I'm sure there are other instances where running `%%bash` will come in handy.
