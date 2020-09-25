---
layout: post
title: Git Collaboration
date: 2018-03-24 05:00:00
tags: git github collaboration pull-request
author: Izzy
published: true
---

### Story
Patrick, a software engineer on your team, has just submitted a pull request containing the newest feature to the product that your team owns. Patrick chooses to leave the office because he's done amazing work and deserves to spend some time with his dog. You later notice that he has misspelled the name of the product in the pull request and you start freaking out because it's going to deployed in the next hour. Frantically googling how to commit to a pull request has taken you here.

### Hub
I'm going to require that you install hub, a command-line that makes git easier, before you continue on with this tutorial. It introduces the pull request command to git along with many other useful tools. You can find the instructions to download stuff here: [https://hub.github.com/](https://hub.github.com/)

Note that at the bottom of the hub install they are aliasing git to hub. I personally don't do this step, but you're more than welcome to. All of the commands in this post should still work whether you choose to alias or not.

### Tutorial
Let's start at the very beginning - before you've even clone / forked over the repo.

Clone the repo.
```
git clone git@github.com:example/example_project.git
cd example
```
Fork the repo.
```
hub fork
```
Add Patrick's remote repo.
```
git remote add Patrick git@github.com:Patrick/example_project.git
```
Fetch the refs from Patrick and all of his branches.
```
git fetch Patrick
```
Checkout the the branch that Patrick issued the pull request from. Here we are assuming that the branch was named "feature".
```
git checkout -b feature
```
The branch name is usually found within the pull request page in a format like `remote_name:branch_name`. It may look like this:
```
Patrick wants to merge 1 commit into example_project:master from Patrick:feature
```

Now you are able to make changes to the code. After you are done, be sure to add files to the git staging area.
```
git add some_file.txt
```
Commit the changes and squash if needed.
```
git commit -m 'fix application name'
```
Finally push your changes to Patrick's pull request. You my need to --force if it doesn't let you push. This usually happens when you have to rebase the code in some way.
```
git push Patrick feature
```
That's the end of this tutorial! I hope that you were able to get it working. If you were not able to, please message me with questions on your particular use case. This git workflow is just how I expect collaborations to be done, but you are more than welcome to ping me with changes. Thanks for the read!
