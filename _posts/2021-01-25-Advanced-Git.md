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

![feature branch is now based on latest master](/images/rebase2.png) 

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

Simply put, "git reset" will reset your current work. It can do so both for staged and unstaged changes. With no additional tags, this will unstage any changes.

#### Soft Reset

    git reset --soft <commit>

First, lets talk about the - -soft tag. A "soft" reset will keep your changes and put you back at the HEAD of your branch. This helps you keep what you are working with and revert back to the HEAD of your branch to get rid of any commits you aren't satisfied with. 

#### Hard Reset

    git reset --hard <commit>

A hard reset will completely revert everything in your working directory to the last commit. Anything that is not commited will be lost, even if you added it as a staged and ready to be commited change. Only use this when you want to completely lose all your unsaved changes.

#### Mixed Reset

    git reset --mixed <commit>

You guessed it, this one is a mix between a hard and soft reset. You will not lose any of your changes made if they are staged to be commited, and everything else will be reset back to the HEAD of your branch.

### Git Checkout

Git checkout allows you to change the branch you are currently working in. Similar to my example above, if we want to switch from master to feature-A, we will do the following:

    git checkout feature-A

Similarly, if we want to change back to master:

    git checkout master

There are some notable options we can add to git checkout. -f will force the switch to a new branch even if the index or working tree differs from the HEAD. Any unsaved changes are lost. -b will create a new branch from your current HEAD:

    git checkout -b feature-B

Checkout can also be used to pull specific file versions.

    git checkout branch -- <file>

This will cause the specified file to change to the version in the commit specified. This is a risky command because you cannot retrieve any local changes that are reverted.

One last notable checkout command is used to changing the HEAD of your branch to a specific commit:

    git checkout <commit>

It is worth noting when using this command your branch will checkout the commit as a detached HEAD.

### Git Revert

Git Revert can be used to go back to a previous commit. For example, if your latest commit is C and want to go back to commit A, use the following:

    git revert <commit A hash>

This will take you back to the moment commit A was created, and you are now working as if commit B and C no longer exist.

### Some Examples

Now that we know all about **reset, checkout, and revert**, lets look at them in action.

#### Git Reset Example:

![git reset example](/images/gitreset1.png) 

#### Git Checkout Branch Example:

![git checkout example](/images/gitcheckout1.png) 

#### Git Checkout Commit Example:

![git checkout commit example](/images/gitcheckout2.png) 

#### Git Checkout File Example:

![git checkout file example](/images/gitcheckout3.png) 

#### Git Revert Example:

![git revert example](/images/gitrevert.png) 

## Git Submodules

A git submodule is like Inception. A git repository inside of a git repository. A submodule would allow you to treat a directory within a project as it's own project. This is useful to help keep separate changes to a specified directory from conflicting with your actual project. These are mostly used when a library is a part of a project, and its contents and changes are handled separately. One advantage to using a git submodule is that it allows you to keep the commits separated. This is hugely useful when libraries are only updated periodically and you need to see what was previously done when updating or making changes. While it is great at keeping your libraries clean and separate from your merge/commit history, there are some downsides. As you can imagine, this increases complexity of upkeep with each level of version control. If you aren't careful you can create merge conflicts that are a headache to repair if you weren't properly paying attention. If both repos are being edited at the same time, and you end up having a mismatch of versions if you aren't careful. However, that last one is more of an issue with bigger projects and/or companies if communication on updating is not solid. When used correctly, git submodules can be great additions to projects and the libraries within.