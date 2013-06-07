---
layout: post
title: "Creating a Github Blog Using Octopress"
date: 2013-06-07 09:33
comments: true
categories:
---

A typical exercise in infrustration
---------------------
---------------------
There are many online guides to setting up an Octopress blog using Github pages.
Here are some key differences in my setup. Hopefully this will help find a solution faster and  avoid some of the frustration I went through.

### Some helpful tutorials
The following sites where my main source of information. Although there are many others, these are the ones I used after limiting my Google search
to results posted in the past year.

<ul>
    <li><a href="http://octopress.org/docs/setup/" title="Octopress">Octopress</a></li>
    <li><a href="http://paulsturgess.co.uk/blog/2013/04/24/hello-octopress-and-github-pages/" title="Paul Sturgess">Paul Sturgess</a></li>
    <li><a href="http://www.tomordonez.com/blog/2012/06/04/creating-a-github-blog-using-octopress/" title="Tom Ordonez">Tom Ordonez</a></li>
    <li><a href="http://schuyler.info/blog/how-to-setup-a-new-octopress-blog-on-github-pages.html" title="Schuyler Erle">Schuyler Erle</a></li>
    <li><a href="http://daringfireball.net/projects/markdown/basics" title="John Gruber">John Gruber: Markdown Basics</a></li>
    <li><a href="http://tmpvar.com/markdown.html" title="Elijah Insua">Elijah Insua: Markdown Previewer</a></li>
</ul>

Initial Setup
---------------------
Paul Sturgess' tutorial was the closest match to the exact procedure I used. Thank you Paul.

Create a new Github repo named yourgithubusername.github.io

The name is important here as Github Pages will automatically serve up the content at http://yourgithubusername.github.io

### Grab Octopress and change directory:
Note: When you clone Octopress, it will create a directory named yourgithubusername.github.io

$ git clone git://github.com/imathis/octopress.git yourgithubusername.github.io <br />
$ cd yourgithubusername.github.io <br />

$ gem install bundler <br />
$ bundle install

### Install the default theme:
Note: I received an error if I ran rake install
If someone can explain why I would appreciate it. I searched and found the solution: bundle exec all rake commands

$ bundle exec rake install

Octopress has a configuration rake task that automatically sets the repo up for easy deployment to Github Pages: <br />
Note: This will ask you for your Github Pages repository url.
When prompted in the next command, make sure you enter: git@github https://github.com/username/username.github.io.git <br />
If you omit the suffix 'git' the following commands will not work with your repository and Octopress will only render locally with Pow

$ bundle exec rake setup_github_pages

This task does quite a few things. The most important is that it creates a new _deploy directory that is another git repository. This is where Octopress generates the flat website for deployment to the master branch of your repo on Github.

All the Octopress code used to generate the website into the _deploy directory now lives in new branch called source. Note in the source branch the .gitignore includes _deploy so it doesn’t get committed in two places!

This sounds more complicated than it is, Octopress has rake tasks to make this really easy to manage. It’s worth pushing up at this point to check everything works before tinkering:

$ bundle exec rake generate <br />
$ bundle exec rake deploy

This copies the generated files into _deploy, adds them to git, commits and pushes them up to the master branch.

Load up http://yourgithubusername.github.io

Note: the first time(s) I tried this it didn't work. WTF. I deleted my repository and recreated the repository. Still did not work.
The error page said that Github would send me an email in about 10min. 20min. 30min. I still haven't received an email.

At this point only the website has been committed, the source needs to be comitted separately via:
Note: I tried this next but received an fatal error on the push. This was due to lack of 'git' suffix as noted above.

$ git add . <br />
$ git commit -m 'Initial Octopress source commit' <br />
$ git push origin source <br />
$ bundle exec rake deploy <br />

### Running Octopress locally
Octopress works really well with POW server. <br />
Note: This was a clue. Octopress rendered locally but not remotely through Github.

$ bundle exec rake preview

Load up http://localhost:4000

### Create a new posting
$ rake new_post["Creating a Github Blog Using Octopress"]

Go to the app folder source/_posts to find the new posting
Edit the posting and then follow these steps

$ bundle exec rake  generate<br />
$ git add . <br />
$ git commit -m 'Initial blog post' <br />
$ git push origin source <br />
$ bundle exec rake deploy <br />

Load up http://yourgithubusername.github.io
