---
title: "Git Rewrite History for Sanity"
date: 2020-11-05T04:00:00-07:00
draft: false
categories:
- SCM
- Git
tags:
- git
- development
- tricks
---

When was the last time you were preparing to finish polishing off that shiny new feature in preparation for merging to the release branch? Only to find yourself staring at a mountain of rebasing merges to pull in to your local branch?

This is a pretty common situation when your team is leveraging a rebase centric methodology. Feverishly pushing small changes and stacking up those precious commits. All the while creating an ever-growing list of rebase commits that you will eventually have to pour over and ensure not to break your code or that of others!

## Find your origin commit

Now that you have written all that code, created tests, added assets, and, and, and... It's time to review all that history and decide where you would like to begin your new SUPER commit. Let's take a look at the logs!

Enter the git log, your portal into the magic that is Git. The `log` subcommand is great, to show a nice concise history without much clutter I tend to use the following to review my git history.

`git log --graph --decorate=short --abbrev-commit --pretty=oneline`

This will show you a condensed graph view of your commit history, you can review it and locate the commit SHA reference of an earlier commit or even the SHA of the commit the branch was created from. I tend to try and use this original commit as my squash "target" if you will. Of course the longer your branch lives the more likely it becomes this optimal commit may change. Whatever the case may be, find the commit you want then copy down its SHA for later use.

## The squash

Remember that SHA that we were just talking about? Now, is it's time to shine! The next command we will use is where the magic happens, we will be replaying the history of your local branch and 'squashing' all those amazing commits into just one super-duper commit.

`git rebase -i {COMMIT SHA}`

This tells Git to start an interactive rebase on your current HEAD. You will be presented with your editor of choice and shown a listing containing the entire history between your current HEAD and the SHA that you provided the command. Each of these commits by default will be prefixed by the word 'pick'

```
pick 324abf2 Fixed the bug in the widget
pick 8823aa2 Added tests for supporting widget model
pick aec2238 Added widget toggle state
pick fe22d92 Added new amazing widget that does a thing!

# Rebase 9923edf..324abf2 onto 9923edf
#
# Commands:
#  p, pick = use commit
#  r, reword = use commit, but edit the commit message
#  e, edit = use commit, but stop for amending
#  s, squash = use commit, but meld into previous commit
#  f, fixup = like "squash", but discard this commit's log message
#  x, exec = run command (the rest of the line) using shell
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```

So here we are, as we can see from the helpful documentation the standard commit workflow is represented by the 'pick' command. That's great, but not exactly what we wanted for this exercise. We are trying to merge all those into a single commit right? 

So what we need to do now is go through each line and either set them to 'squash' or 'fixup', generally we will be utilizing 'squash' but where it may make sense to do so 'fixup' is also available. The first line should remain as a 'pick' as this will be where the squash rolls up to.

```
pick 324abf2 Fixed the bug in the widget
squash 8823aa2 Added tests for supporting widget model
squash aec2238 Added widget toggle state
squash fe22d92 Added new amazing widget that does a thing!

# Rebase 9923edf..324abf2 onto 9923edf
#
# Commands:
#  p, pick = use commit
#  r, reword = use commit, but edit the commit message
#  e, edit = use commit, but stop for amending
#  s, squash = use commit, but meld into previous commit
#  f, fixup = like "squash", but discard this commit's log message
#  x, exec = run command (the rest of the line) using shell
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```

Once you exit this editor you will be presented with another editor where you will be able to verify the new commit log and make any changes as necessary. Edit as you see fit and save/exit when completed. Once you have exited, you will now have a rosey new commit that contains all those previous commits as one squashed bundle of joy!

## What's Next?

As this process has rewritten your previous git history, you will need to force push these updates to your remote server.

`git push origin HEAD --force`

You can now rebase changes from the release or other target branch into your local branch and then resume your normal workflow!

🍻 Sanity has been restored, and future rebasing shortened!