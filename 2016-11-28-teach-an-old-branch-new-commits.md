---
layout: post
title: Teach a new branch old commits
comments: false
description: “Accidently put all your commits on master? Let’s move them to a new branch"
keywords: “git git-branch commit github"
---

I was recently using an open source project and, like software often doesn't, it didn't do what I expected. So, I opened it up and started fiddling with the code to see if I could make it work. Soon enough, I had an improvement, so I quickly committed the code. Excited by my positive results, i kept fiddling and found myself with more improvement, so I committed more code. I found myself with what could be a reasonably helpful pull request to the projects owner, but heck darn it, I did it all in master. I forgot to make a new branch for my work. How often do you do this? Every now and then.

After some google fury, I was delighted to discover that's actually it's quite easy to retroactively move commits to a new branch. Hopefully you can learn from my trouble:

    git branch my-awesome-fix
    git reset --hard HEAD~2 
    
The `~2` above is the number of commits you want to roll back from master. So if you have 5 commits you want to move from master to a new branch the line would be `git reset --hard HEAD~5`

Boom done. Now you can push your beautiful new branch to your fork on GitHub and create a clean pull request.

What are your common git snags?
