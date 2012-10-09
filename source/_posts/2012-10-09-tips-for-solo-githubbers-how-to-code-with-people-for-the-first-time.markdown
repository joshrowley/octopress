---
layout: post
title: "Tips for Solo GitHubbers: How to Code with People for the First Time"
date: 2012-10-09 19:43
comments: true
categories: 
---
I used Git collaboratively for the first time exactly one week ago on Day 1 of Flatiron School. It wasn't pretty, you can read a little about it in my first post. 

I had some experience using Git while doing Michael Hartl's Rails Tutorial in my spare time, but I had never used it with other people. Git is very simple when it's just you. I was pretty much only doing commits and pushing to the remote, with the occasional branch.

This is one of the biggest limitations facing beginners trying to learn how to become professional developers on their own, whether through books or online resources. You must learn how to use Git with other people! That much is very apparent from my first week at Flatiron. If a coding academy isn't an option for you, find a Meetup for beginners. Convince your friends to learn it with you. Find somebody.

Once you got your collaborators, here are some tips I'd like to share based on my first experience of being a newbie lone wolf Git user being thrown into my first group Git collaboration:

1. _Establish team's workflow process and best practices first._ It's really important that everyone understands conceptually how your repo is organized, and who should be committing where. Do not rush this step! If someone is a little shaky on the foundations, take the time to get them up to speed. Even someone with the best of intentions can cause havoc on the rest of the group's work if they misunderstand your group's workflow.

2. _Make sure you're working on the correct branch._ I pulled down the master branch, and created my own feature branch. However, at some point I checked out the master branch and didn't notice. I lazily pushed to GitHub without specifying a specific branch (I assumed was in my feature branch), and as a result committed a major party foul by pushing to the master branch. If you're making commits on the wrong branch, you're setting yourself up for confusion when trying to merge feature branches together, or merging them back to master.

3. _Do not run any commands if you don't understand what it does._ Not only that, but do you understand its consequences in your specific context? I was super eager to get started with git. I didn't remember everything, so I asked my fellow classmates about how to do this and that when I ran up against a wall. Or, I would try stumbling around with git commands until I got what I wanted. Do not do this! Git is not IRB! You can't just play around with it. If you're stuck, take as much time as you need to research and figure out what you need to do, or ask someone who would know. Otherwise, you can screw up your repo big time.

4. _One mistake can proliferate exponentially into a huge mess._ Unfortunately, my only advice is to try to not make any mistakes, especially when merging, rebasing, pulling, and pushing. Take a moment to step back and doublecheck that you are using the correct command, in the correct branch, to the correct remote. Ask yourself, when I hit enter, what will this do? If you're not 100%, don't do it.