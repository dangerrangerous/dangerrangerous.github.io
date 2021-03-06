---
layout: post
comments: false
title: Build a Blog with Jekyll
---

### The What:
<!--excerpt.start-->Build a Blog using [Jekyll](https://jekyllrb.com) - a simple, static, blog-aware
website builder that is hosted by [GithubPages](https://pages.github.com). We
can build our Jekyll site from the ground up or take the easy route and download
one of the many [themes](https://github.com/jekyll/jekyll/wiki/Themes) available
and customize it.

### The Why:
* Jekyll supports mobile friendly themes.
* Since we are tied into Github, source control is already baked in.
* Posts are composed of static files, jekyll does the rest of the work for us.
* I'm setting up for a play on words in a later blog post...

### The How:
* Make sure you have ruby and ruby-dev tools installed on your machine. You
may have to use 'sudo'. On a Unix-like terminal type:

> sudo apt-get install ruby

> sudo apt-get install ruby-dev


* Install jekyll on your machine
<div class="message">
sudo apt-get install jekyll
</div>

* Download your theme of choice. For this example we will use
[Poole](https://github.com/poole/poole)

* Change directory name to yourgithubusername.github.io and run

> yourusername.github.io jekyll build

> yourusername.github.io jekyll serve


The default port is set to 4000 so open your browser of choice and type in
localhost:4000

Hopefully things are working! If they aren't, don't panic. I encountered some
errors that I resolved by following directions listed in the issues section
of the github repository. Take a deep breath and do some googling.

Now you can open the folder labeled posts and create a new .md file. Use the
existing posts as examples of how to format yours. Check out the [Jekyll Docs](https://jekyllrb.com/docs/posts/) for more help on creating posts.

Once you've got a post you'll want to push the changes to Github. Follow the
directions for setting up a [Github page](https://pages.github.com). If you
don't have a Github account, git one.

<3
