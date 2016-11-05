---
title: Revisions in LaTeX using Git
tags: [git, latex]
categories: [blog]
---

Working with documents, I always try and keep track of the current version of it. Using Microsoft Word I usually just put a ``_draftX`` suffix to the name of the file, with X being the running number. Working with LaTeX documents this may not be feasible, so we can start using something more fancy. Using Git for version control and a LaTeX package called [gitinfo2](https://www.ctan.org/pkg/gitinfo2) makes live pretty easy.

<!--more-->

**Note:** This post was inspired by my need to write down on how to use ``gitinfo2`` and is more or less a small documentation for myself.
{: .notice}

Basically ``gitinfo2`` provides a simple interface for LaTeX to acquire metadata from your Git repository e.g., the current branch, hash and release tag. In the end you will find that information in the footer of your file on each page.

<figure>
    <a href="/images/posts/git-revision-large.png"><img src="/images/posts/git-revision.png"></a>
    <figcaption>Snapshot of the resulting output in one of my documents</figcaption>
</figure>

This is actually quite nifty and the [documentation](http://mirrors.ctan.org/macros/latex/contrib/gitinfo2/gitinfo2.pdf) provides a way to add this functionality to any local Git repository. Assuming you are using macOS and installed TeXLive 2016, this would go as follows.

We first change into the directory containing our TeX files and initialize a new Git repository.
{% highlight bash %}
cd thesis/
git init .
{% endhighlight %}

Afterwards we copy the sample hook provided by the package, to the hooks folder of the current Git repository.
{% highlight bash %}
cd .git/hooks
cp /usr/local/texlive/2016/texmf-dist/doc/latex/gitinfo2/post-xxx-sample.txt post-checkout
cp post-checkout post-merge
cp post-checkout post-commit
{% endhighlight %}

This is basically everything we need to do manually for now. The only thing left is to load the ``gitinfo2`` package inside our TeX file, using the ``[mark]`` option (this will automatically put the information below the footer, more information is found in the [documentation](http://mirrors.ctan.org/macros/latex/contrib/gitinfo2/gitinfo2.pdf)).

{% highlight latex %}
\usepackage[mark]{gitinfo2}
{% endhighlight %}

That is pretty cool, but I do not want to bother copying the files again and again when creating new repositories. Luckily Git can do the job for you! There is a special template folder, which Git uses to copy files from when creating a new repository.

Looking at my ``~/.gitconfig`` file, I have defined the templates folder to reside here:

{% highlight bash %}
[init]
    templatedir = /usr/local/share/git-core/templates/
{% endhighlight %}

Basically one just copies the ``post-xxx-sample.txt`` file to that location.

{% highlight bash %}
cd /usr/local/share/git-core/templates/hooks
cp /usr/local/texlive/2016/texmf-dist/doc/latex/gitinfo2/post-xxx-sample.txt post-checkout
cp post-checkout post-merge
cp post-checkout post-commit
{% endhighlight %}

Afterwards creating a new repository, you will end up with the three files inside the ``.git/hooks`` folder. Now you are ready to add revision information to your LaTeX documents! Just be aware that you need to either do ``git checkout``, ``git commit`` or ``git merge`` for the information to appear in the first place.
