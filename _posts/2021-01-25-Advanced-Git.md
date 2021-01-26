---
layout: post
title:  "Advanced Git"
date:   2021-01-25 5:33:23 -0700
categories: blog post
---
# Less Commonly Used Git Features
Git can get complex quickly, and it is important to know what to do when problems arise with any version control system. Since every developer relies on some sort of version control system, lets take a deep look at Git's more advanced features that are less often used but important to know when you need to get yourself out of a jam.

# Git Rebase
What is rebasing? It might sound complicated, but the simplest way it can be explained is to take a branch that was created off of one commit, and place it as a branch on another "base" commit. For example, imagine you are developing a new feature that lives on a new branch. This was created off of the main branch in your company's organizational git repository. Your coworker was working on something else that is relevant to your feature and merged their changes to the main branch. Well, now the original branch you were working off of is now out of date. You *could* merge the new main branch version into your branch so it is up to date, but now your merge history with this feature is all funked up and harder to work with. Instead, you decide to slightly change the history of your current branch and base it off the new version of the main branch. This helps you continue working with the updated code, but now your commit history on this branch is the same as it was as if we moved it forward in time. Nothing in your branch changed, and it will look as if you started working off of the most recent commit in the main branch, and you can continue working as if nothing happened. 

Confusing right? Lets look at an example:
![feature branch is out of date from master](/images/rebase1.png) 

I created a branch off of hash **e127e9b** and did some work. Since then, there have been two commits added to master with more up to date work. To continue working with my branch, I need to incorporate the new version of master into my branch without merging. I need to *rebase* onto the newest version of master. This is very easy to do, first we will make sure we have checked out the feature-A branch. Then, **git rebase master**. The syntax of this is easy, as we are just telling git to rebase to the desired branch. Here are the results:

![feature branch is out of date from master](/images/rebase2.png) 

From this screenshot, you can now see the two commits in feature-A, are now in front of the master branch. You can see the dotted circles are where feature-A used to be. From doing this, we have successfully brought our branch and all of it's history infront of master. We can work with the most update version of master while finishing feature-A, and ensure we are working out any issues with the latest version. This is what version control is all about! Our commit history is intact, doesn't interfere with what was recently done in master, and we can keep working with confidence.

#### Changing history

With the last example, we did not edit the history at all for what was done in our branch. Everything was carried over, the good and the bad. Now, let's say we have a somewhat messy history and don't want to pull every commit over, just the ones that matter? Luckily for us, we have **Interactive rebasing**. This takes into account everything we just talked about, but with a tag added before we define the branch we want to rebase to. For example:

    git rebase -i master

This will give us a new prompt to choose the commits that we want to have move with us.

    pick 549d460 Tanner Fronk
    pick 0e026d9 Starting new features
    
    # Rebase 8db7e8b..fa20af3 onto 8db7e8b
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

From here we can change the history about our branch and the commits. We can just bring the commit over (pick), edit the commit message (reword), delete (squash), or combine (fixup). Interactive rebasing is very powerful and is useful when wanting to cleanup a branch that may have a lot of clutter we don't want to bring.

Okay, so rebasing is useful for a lot of different reasons, but when should you **not** use it? The best way to quickly sum it up is: conflicts. If you use rebase wrong, you can cause even bigger headaches than you think you may fix. Never, under any circumstance, rebase master onto a branch. This will cause git to think your branch and master are completely different. If you are able to merge the two together, your merge history is going to have twice the amount of commits and be very hard to go back to in case you have read the repository's history. What is more likely to happen when using rebase wrong, is conflicts. Git will think that you are trying to merge two conflicting versions, and you may end up losing a lot of the progress you wanted to keep. Especially when working with bigger code bases, the risk will only rise. So, the best rule of thumb is to only ever rebase your specific branch around master, and leave master where it is. There is no reason in a regular git workflow that master should ever be moved ahead of itself.

## Stage Based Git

When working with Git, you are always working with what is staged, and unstaged. You will be thinking about what you want to work with, what you want to track, delete, or branch out. Since Git is an incredibly powerful tool, there are some great commands to help you along the way.

### Git Reset

Simply put, "git reset" will reset your current work. It can do so both for staged and unstaged changes. 

#### Soft Reset

    git reset --soft

First, lets talk about the - -soft tag. A "soft" reset will keep your staged changes (the ones ready to be commited), and reset everything not currently being edited to the last working commit.