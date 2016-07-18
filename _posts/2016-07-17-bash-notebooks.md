---
layout: post
title:  "Csvkit data pipelines with Bash in Jupyter Notebooks"
date:   2016-07-17 17:00:00 -0500
categories: data analysis
tags: notebooks
---

By Christian McDonald

I've been enamored with [Jupyter Notebooks](http://jupyter.org/) since I learned about them at this years [NICAR conference](http://www.ire.org/conferences/nicar2016/). The idea of transparent, repeatable data analysis for journalism is important to me, and Notebook makes this pretty possible.

Learning about them has also turned me into a fan of **conda** virtual environments, and the [Anaconda](https://www.continuum.io/downloads) and [Miniconda](http://conda.pydata.org/miniconda.html) package solutions.

But I was bummed when I first tried to use csvkit-based data pipeline in a notebook, but I couldn't figure out how to run those csvkit commands, because they are usually used from a Bash command-line. Until I found [jeroen janssens's "iBash Notebook" post](http://jeroenjanssens.com/2015/02/19/ibash-notebook.html). He had found [Thomas Kluyver's bash kernel](https://github.com/takluyver/bash_kernel)

With a few quick commands, you can add a Bash option to your Jupyter Notebook. Once you are in your virtual environment, do:

``` bash
$ pip install bash_kernel
$ python -m bash_kernel.install
```
Now when you start you Notebook, you should have a Bash option.

![start-notebook]({{ site.url }}/assets/img/notebook-start.png)

I'm using this for the first time this fall with my UT Journalism students [to learn csvkit](https://github.com/utdata/cli-tools/blob/master/lectures/UsingNotebooks.md).
