---
title: Discovering scientific data with Python
tags: [python, data management]
categories: [blog]
---

During my studies all students always used Excel or OriginPro for analysis of experimental data. Starting my thesis in a theoretical laboratory finally introduced me to the every day use of Python.

Working with a bunch of data files can be quite tedious, especially when one has to keep track of their paths to do analysis on their contents. Gladly there is one Python package out there trying to solve this problem: [datreant](http://datreant.org/). This post is not supposed to give a full review of the package, but only show a neat way to get started. 
<!--more-->
Let's cut to the chase: grabbing paths to theoretical/experimental data files is never a great task to solve. Using Pythons ``os`` package one can [walk](https://docs.python.org/2/library/os.html#os.walk) through a path and grab all file names. But why bother to create own solutions when someone already has done so?

Let us have a look at a simple datreant workflow:

{% highlight python %}
# Import the datreant.core package and define a path where our data resides
In [1]: import datreant.core as dtr

In [2]: data_path = '<path-to-masters-thesis>/wetlab/data/c-laurdan/20161013'
{% endhighlight %}

So far we have not done anything special. We imported a package and set the path to the folder containing some experimental data.

{% highlight python %}
# Create a Treant object inside data_path
In [3]: treant = dtr.Treant(data_path)
{% endhighlight %}

``datreant`` actually comes with a handy way of visualizing everything inside a given ``Treant`` using the ``Treant.draw()`` method.

{% highlight python %}
In [4]: treant.draw()
20161013/
 +-- 1B.txt
 +-- 1.txt
 +-- 2B.txt
 +-- 4.txt
 +-- 20161013_CLaurdan.opj
 +-- 3.txt
 +-- 4B.txt
 +-- 2.txt
 +-- Treant.94847b1d-7ef8-490a-a381-c509fd0b1ac0.json
 +-- 3B.txt
{% endhighlight %}
i
This is neat, but we are only interested in the .txt files inside this folder. (``Treant.94847b1d-7ef8-490a-a381-c509fd0b1ac0.json`` is a state file created by ``datreant.core`` storing information about the current ``Treant`` object.)

To filter out everything else we just [glob](https://en.wikipedia.org/wiki/Glob_(programming)) for files ending with ``.txt``. Afterwards we use the ``abspaths`` method to retrieve the absolute file paths, instead of the ``Leave`` objects that we would get otherwise.

{% highlight python %}
In [5]: leaves = treant.glob('*.txt').abspaths

In [6]: leaves
Out[6]:
['<path-to-masters-thesis>/wetlab/data/c-laurdan/20161013/1.txt',
 '<path-to-masters-thesis>/wetlab/data/c-laurdan/20161013/1B.txt',
 '<path-to-masters-thesis>/wetlab/data/c-laurdan/20161013/2.txt',
 '<path-to-masters-thesis>/wetlab/data/c-laurdan/20161013/2B.txt',
 '<path-to-masters-thesis>/wetlab/data/c-laurdan/20161013/3.txt',
 '<path-to-masters-thesis>/wetlab/data/c-laurdan/20161013/3B.txt',
 '<path-to-masters-thesis>/wetlab/data/c-laurdan/20161013/4.txt',
 '<path-to-masters-thesis>/wetlab/data/c-laurdan/20161013/4B.txt']
{% endhighlight %}

Easy. Right? There is much more to datreant, but we will come back to that at some point later in time.
