---
title: "Lesson 3 - Git in RStudio & Time Travel"
output: 
  html_document:
    keep_md: TRUE
---



The learning objectives here are: 

1. to work through the code/add/commit cycle in RStudio, and 
2. to get a feel for how you can move around the repository 

This is pretty mind-bendy to me at times, but if you go back to the core idea of git storing a series of snapshots as nodes along a graph, you'll be able to wrap your head around the idea of revisiting any of those nodes at any time.

# Part 1
## RStudio
RStudio provides a very nice front-end IDE for R. As it continues to develop, more and more capabilities are added. Version control with ```svn``` or ```git``` has been available for quite some time. You may have even seen the git tab in RStudio and wondered what it was doing there. I use both the command line and RStudio for my git work, so it's a good idea to have a feel for both. I prefer the command line interface for some things, but being a visual person, RStudio's git support has a lot to offer. In particular, it's quite easy to write a good commit message, and to visually see the diffs/changes that you are making.

You will have seen how we did this in the Lecture - now it's your turn. 

The first thing we have to do is create a new (RStudio) Project within the directory that contains your git repository. For me it was ```my_repo```

Make some changes to a file, add and commit them. 

Here are a few additional things to try and experiment with:

* Add a new script, and note the difference in the buttons in the git tab in RStudio 
    + when do you see an A in the button vs. an M? 
    + What are the command line analogs?

## Mini-Analysis
Since you have the data, and shells of scripts, spend some time making a mini-analysis (very mini!). We're going to use a script that I wrote for you, and I'm going to have you re-factor it to make a complete "analysis." 

The script is called ```prepareDUMLData.R```, and it is in the Box link I sent; download and open it up in RStudio.

The idea behind this exercise comes from from a workshop I took at RStudio::conf in 2019 - taught by Jenny Bryan of RStudio. She put up this table as a way to think about organizing your data:

![](images/2020-01-21_workflow.png)



So, using this table and the ```prepareDUMLData.R``` script, refactor the code in this script to 4 different R scripts, using the shell scripts that you already have from Lesson 1:

1. Put reading code in ```read_data.R``` 
2. Put wrangling code in ```wrangle_data.R``` 
3. Put plotting code in ```plot_data.R``` 
4. Put ```lmer()``` code in ```run_analysis.R``` 

And use version control for it all as you go. Once you are finished, place all of the scripts into 1 controller file - the ```runAll.R``` file from Lesson 1.

When you are done, commit the changes.

Back to the lecture.

# Part 2
## Git Diff
How can we easily see what changed between two commits? We may see that one line indicated a big change that we want to inspect. Use the diff command on any two nodes in the graph:


```bash
git diff 04694c0 d57964e
```

Note that the order matters. The code above says show me what changed as the repo moved from ```04694c0``` to ```d57964e``` Here's what mine looked like:


![](images/gitDiff.png)

When you think about working with your future self - let alone any additional collaborators - these diffs are powerful ways to see what actually changed. Couple the diffs with a good commit message, and you can quickly get oriented into how and why things change over time. 

Compare using this approach this to using an R script that you just rename, or keep "saving as" each time you alter it. You may get to a point with that script, where it's broken and it's ~~hard~~ impossible to go back to the state when it last worked. 

Or you may recall having ventured down a pathway that you gave up on, only _now_ you want to get back to that path to try that experiment again. With ```git``` this is straightforward. Without ```git``` it's just about impossible.

One thing you can get in the habit of doing (if it works for you) is to look at the diffs before you commit. My typical workflow is something like:

1. write some code
2. add it
3. commit it
4. write some more code, etc.

Before you add and commit the changes, though, you can just type ```git diff``` to see what has been changed. With the code we've been running here, it's just toy syntax, but if you were working on a complicated function, it's often nice to see what you've done. 

Ok, back to the lecture.

# Part 3
## Going to a Particular Snapshot
Now that we have a few commits built up, we can go back to a particular commit and have our code (in the same file!) look exactly as it did at that commit. And we can do this without throwing away the most recent work. Take a look at your history with ```git log --oneline``` and chose a commit you want to navigate to. 

![](images/gitLogTT.png)

Yours will be different from mine, but to go to that node, we use the ```checkout``` command:


```bash
git checkout 17a07c9
```

You'll see the detached HEAD warning, and if you type ```git ls-files``` you'll see the files git was tracking at that point in time. If you look at one of these scripts, you'll its contents see it at that point in the development:

![](images/plot0a215c7.png)

Depending on the changes you have or have not made, this may not seem mind-expanding.

Two thoughts here:

1. If we wanted to recreate old figures, this is one way we could do it. Checkout an old repo, and re-issue the R code from the command line to make the file. You can also hang on to them as we mentioned in lecture via the storing of old copies of the ```results/Figures``` folder with an archival date. Just make sure you are consistent if you follow this, otherwise you may end up with an old file that has a current name, but not the current content, etc.

2. Type ```git log --oneline --decorate``` here to see what you get. You'll probably see a shortened history, because we've moved the pointer back to this particular commit, and commits know their parent commit - so the later ones aren't listed. Here's a snapshot:

![](images/det_head.png)

Not seeing all the new work may give you _great_ pause because you don't see work you've already done. There's yet another command to help you.


```bash
git reflog
```

![](images/reflog.png)
This command basically stores every command you issue in git. So you can see that now we're checked out on the 17a07c9 node, and even though we don't see all the other downstream commits when we type ```git log``` at this snapshot, we can rest assured that they commits are all still there.

This can be really useful as you progress and get into more complicated things with merging branches and rebasing the repository. The take home is that git tries really really hard not to lose your data.

To get back to the last commit:


```bash
git checkout master
```

![](images/reflogMaster.png)

And that's you done this lesson!

