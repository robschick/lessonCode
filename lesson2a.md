---
title: "Lesson 2, Part 1"
output: 
  html_document:
    keep_md: TRUE
---



Awesome. Now we're ready to move things along with git somewhat. At this point, we've made a structure that's consistent, and can form the basics of a reproducible workflow. We've talked about one way to manually track the changes of the whole project - namely taking the whole structure, and putting it into a folder with a date-specific name, e.g. ```2017-08-06_contents-of-myRepo```

This can work, and is not a bad setup, but it requires a lot of intervention and consistency on the part of you, the project organizer. One other drawback - in my mind - is that with more than one user and/or more than one computer, it's very hard to scale. Things can get complicated and out of phase/sync quickly. The solution? A modern version control system; here we'll explore ```git```


```git``` was written by Linus Torvalds - the creator of Linux, and supposedly [not the nicest collaborator in the world](https://arstechnica.com/gadgets/2018/09/linus-torvalds-apologizes-for-years-of-being-a-jerk-takes-time-off-to-learn-empathy/): 
![](images/linus-eff-you-640x363.png)


It was an outflow of bitkeeper, and was built to manage the linux kernel. Undoubtedly this is a bigger project than you or I will ever work on. At present the kernel is approximately 17 million lines of code, with 2-3 thousand contributing developers. No wonder they need a robust version control system. 

We've talked about what ```git``` is in the lecture, and now we'll start to put it into practice. 

## Pay Attention Here

You should be in the **folder that you made that contains the whole repository**, e.g. ```my_repo```. Type ```pwd``` just to make sure you are where you think you are. 

Then let's initialize the repository:


```bash
git init
```

If successful, all you'll get a message indicating that you have initalized an empty repository. Success!

Next let's look at the status of the repository:


```bash
git status
```

You will see a message about being on branch master, that you are on the initial commit, and that there is nothing to commit. Ok, so what now? Let's add a file to the staging area. Recall that there are three states of git:

1. the working directory
2. the staging area
3. and the git repo itself - i.e. where things end up when you commit

Right now we are in the staging area, and we should have 5 ```*.R``` files - 4 that will do something eventually, and one that will run all the others. Let's add the first one ```read_data.R``` (or whatever you named the file to read in the data).



```bash
git add src/read_data.R
```

You should get nothing back - just the command prompt again. But if you ask for the status again, you'll see something different:


```bash
git status
```

Now that we've added a file to the staging area, we should see a message indicating there are changes to be committed, and it should label what the new file is. On my computer it looks like this:

![](images/gitStatus.png)

Now that we've added it, let's run our first commit:


```bash
git commit -m "First Check-in of read_data.R script"
```

Now we get some more information back from git about this commit, including the all-important sha-1 identifier. Here you only see the abbreviated sha-1, but if you type ```git log``` you'll see the full sha-1:


```bash
git log
```

Along with who made the commit (hopefully you!), the date, and the commit message. Now let's do some comparison between git and the file system. Right now we have several R scripts in the directory. What do you think you'll get when you type these commands? 

Think what the answer will be __before__ you issue the command.


```bash
ls
git ls-files
```

Why the difference? What do you think you'll see if you ask for the status? Think __before__ you issue the command:


```bash
git status
```

If you have untracked files, why is it that git knows about them?

## Going through the Cycle Again
Let's edit the file now to actually read in the data. Do this in your text editor, e.g. Sublime, Atom, etc. 

Here's what I added to mine (recall the tip to put at least a barebones comment at the start of the script):


```r
#' This script will read in raw data from the Cape Hatteras BRS
#' into a data frame called gm182, which will serve as the intermediate data for
#' subsequent analysis
gm182 <- read.csv(file = '../data/2018-11-27_Gm182-Start-CEE-Locations-Kahuna.csv')
head(gm182)
```

Now we have some new code, i.e. the file has been modified. So go back to the command prompt, and type:


```bash
git status
```

What do you see, and why is it different from what you issued git status a few lines back? 

What do you do next if you want to make a snapshot (commit) of the updated file? Add it and commit it, but this time we can do what is called an express commit. Because git already knows about ```readData.R``` we can combine the add and the commit steps:


```bash
git commit -am "Add Code to Ingest the Raw Data"
```

Notice the difference in the flag - here we use ```-am``` instead of ```-m```

Note that this can only be done for files that have _already_ been added.

Let's go ahead and add **all** of the remaining untracked files and commit them:


```bash
git add .
git commit -m "Add Remaining Blank R Files"
git status
```

Your last command should indicate that you have a clean repo. Cool! 

Back to the lectures to talk about viewing the repo.
