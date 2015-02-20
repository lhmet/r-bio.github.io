---
layout: page
title: An introduction to Git and how to use it with RStudio
author: François Michonneau
---


## What is version control?

![A better backup](http://www.phdcomics.com/comics/archive/phd101212s.gif) (from phdcomics.com)

You know that great paper you just wrote, you were done with it, so you called
it "final". And then, you sent it to your advisor for her last comments. So you
renamed it "final\_thisone". A few days passed, and you realized there was a typo
in your figure, and you renamed it "final\_thisone\_really". A couple of weeks
pass, you get comments back from the reviewer, you start changing it and you
renamed your file "final\_submitted". And then... your advisor
think you should rewrite the conclusion and the file become
"final\_submitted\_really".

A year pass, you are mentoring a new undergrad in the lab, and she asks you for
your final document so she can look at your methods... You open your folder, and
mesmerized by the number of final labelled final, you forget which one if the
correct one. Sounds familiar?

Version control is a category of software that keep track of changes to your
files for you. It allows you to:

- keep the entire history of a file and inspect a file throughout its life time
- tag particular version so you can go back to them easily
- facilitates collaborations and makes contributions transparent
- experiment with code and feature without breaking the main project

Version control is designed to manage source code. It works best when tracking
files stored as plain text and not encoded as binary (e.g., Word documents). It
is the "lab notebook" of the digital world. Because of its powerful and useful
features, it is being used for many different project and can be used to track
data sets (small), text (if you haven't written too much of your dissertation,
you should use version control to keep track of it), images, and much more.

## What is Git?

Git is a particular implementation of version control. It's really powerful and
has many features but we will only scratch the surface.


## What is GitHub?

When you start a Git project on your computer, you are going to store the entire
history of the project locally. The storage of your project and its history is
called a **repository**. It is fine. However, the great advantage of using
version control such as Git is to be able to collaborate with other people (and
also to store your repository somewhere else).

GitHub is a commercial website that lets you store your repository publicly for
free (you need to pay if you want to keep them private, you can also get an
educative account with an `.edu` email address that will give you some free
private accounts). There are other website that offer similar services including
BitBucket. Storing your repositories on these website has many advantages. It
offers a friendly interface to many common operations so that you don't have to
remember how to do them at the command line. They also provide other useful
features including an "Issue tracker" and wikis.

## The Git workflow

We have already "committed", "pulled" and "pushed" but we haven't been through
the details of what it means how these pieces fit together.

![The different Git areas]({{site.baseurl}}img/git_areas.png)

The typical workflow goes like this:
- you create/edit/modify a file inside your repository
- you **stage** the changes to the staging area
- you **commit** these changes which creates a permanent snapshot of the file in
  the Git directory along with a message that indicates what you did to the
  file.

When you start a new project, the files in your working directory are
**untracked**, you will first need to **add** them to your repository before Git
can keep track of them and their history.

At this stage, everything is still on your hard drive. To upload your
modifications (i.e., your commits) to GitHub you need to **push** to it.

If you are working with other people you are also committing your shared
repository on GitHub, you will need to **pull** to bring their modifications
into your local copy of the repository.

Commits are cheap. Commit often and provide useful messages so you can keep
track of what you are doing. Don't do this:

![](http://imgs.xkcd.com/comics/git_commit.png) (From xkcd.com)

## Branching

Branching is a great feature of version control. It allows you to duplicate your
existing repository, develop or experiment with a new feature independently, and
if you like what you are doing you can **merge** these modifications back into
your project.

Branching is particularly important with Git as it is the mechanism that is used
 when you are collaborating to external projects (projects you are not directly
 involved with).

RStudio can't create branches directly, so you need to either:

- create them in GitHub and pull the changes in your repository;
- create them from the Shell (Tools > Shell) and type `git checkout -b
  new-branch`

## Pull requests

With a **pull request** you are asking someone who maintains a repository to
pull your changes into their repository. I'll demonstrate the process in class.

## Exploring further

There are many resources on the web to learn about Git and GitHub. An excellent
(but maybe too comprehensive for beginners) is the
[Pro Git Book](http://git-scm.com/book/en/v2).
