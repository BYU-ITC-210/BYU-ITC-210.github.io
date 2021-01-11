---
title: "Homework 1: Linux-Git CLI"
---

***

__*Each problem is worth 1 point*__

***

## 1. Linux Learning Objectives

The goal of this assignment is to have you learn and practice various Linux
commands. Note that you may use these commands at different
points in each term. Specifically, after this assignment you will be able to
use the following commands:

+ `man [-k]`
+ `echo`
+ `pwd`
+ `cat [>]`
+ `cd`
+ `ls`
+ `touch`
+ `mkdir`
+ `ps aux [|]`
+ `mv`
+ `cp`
+ `sudo`
+ `rm [-R]`
+ `chmod`
+ `chown`

> An easy way to determine what a command does is to type `man` and then the name
> of the command you are trying to learn about. For instance, `man cp` in the
> command line interface (CLI) will bring up a page about the `cp` command.
> Simply press <kbd>q</kbd> to leave that screen.

## 2. Preparation

To do this assignment you need access to a Unix-style command line shell such as __Bash__ or __zsh__. The process is different for macOS and Windows systems.

### 2a. macOS

macOS is based on the Darwin core which, like Linux, is a Unix-derived operating system. Accordingly, the shell is built-in. It is called __Terminal__ and is found under `Applications - Utilities - Terminal`. You can also use Spotlight Search or Launchpad to bring it up.

### 2b. Windows

Windows includes the classic __DOS Command Line__ and the newer __PowerShell__. But, for this assignment you will need to install the __Linux Subsystem for Windows 2 (LSW2)__ in order to get a Linux/Unix style command line. We will be using LSW2 and Docker through the rest of the semester so it's definitely worth the effort.

Follow the __Six Manual Installation Steps__ at [Windows Subsystem for Linux Installation Guide for Windows 10](https://docs.microsoft.com/en-us/windows/wsl/install-win10#manual-installation-steps). __Important:__ As of January 2021 the instructions have a "simplified install" for those using a Windows Insiders preview. Please ignore that part and do the "Manual install" six steps. A future update to Windows will probably incorporate the simplified approach. For the Linux distribution use [Ubuntu](https://www.microsoft.com/en-us/p/ubuntu/9nblggh4msv6).

## 3. Using the CLI

Before this course you should have been exposed to using a command line. But, whether you have or not, you should be able to complete this assignment. Using a command line allows you to perform necessary functions when operating a
computer that are not available in the graphical interface. These are some great tutorials on the command prompt if you need a refresher or help: [Codecademy](http://www.codecademy.com/en/courses/learn-the-command-line),
[Survival guide for Unix newbies](http://matt.might.net/articles/basic-unix),
and [Linux Survival](http://linuxsurvival.com/linux-tutorial-introduction). Use
other online resources as needed to complete the assignment. For this
assignment __you will use the CLI__ to run a series of
commands on a Linux-based system.

***

To complete this assignment, first, download the [answer template.](Linux-git_HW_SUBMISSION_2020.docx)

For each of the following tasks, compose a single CLI command. After verifying that the command works, paste the command used in *red* under each line in the template.

Once you have completed the assignment, upload it to LearningSuite for grading.

Like most assignments in this class, you will likely use web sources to help you find answers. For example, you might use a Google or Bing search to find the right command-line for a particular task.

***

### 2.1 Finding Help 

##### Problems:

1. Open the manual for `ps`.
2. Search for manual entries containing the word 'transfer'.
3. Print the current directory
4. Print the current IP address

### 2.2 File and Directory Manipulation

##### Problems:

1. Make a directory called `your_first_name` (e.g, `justin`).
3. Navigate into the new directory.
4. Create two files, one called `your_middle_name.txt` (e.g., `scott.txt`), and
   another called `your_last_name.txt` (e.g., `giboney.txt`).
5. Create another directory (inside the first) called `subdir`.
6. Copy `your_last_name.txt` and call it `your_last_name_2.txt`.
7. Move `your_last_name_2.txt` into the `subdir` directory.
8. Rename `your_last_name_2.txt` to `your_last_name_3.txt` (make sure it stays
   in the `subdir` directory).

### 2.3 File Permissions 
 
##### Problems:

1. List all of the files in the `your_first_name` directory *__with__* permission details.
2. Remove all permissions for others on `your_last_name.txt`.
3. Change the owner of `your_middle_name.txt` to `root` (You will need to type in your password).

### 2.4 File Manipulation 
 
##### Problems:

1. Add the string 'hello' to `your_last_name.txt`.
3. Add the contents of `your_last_name.txt` to `your_last_name_3.txt` (In the `subdir` directory).
4. Print the contents of `your_last_name.txt` to the console.


## 3. Git learning outcomes

Git is a source control system to help manage
versions of your projects. GitHub is an online place to store git projects.
In this class, we will be using GitHub to be able to
push our code to our production environments from our development environments. After
this assignment, you should be familiar with the following commands:
+ `git init`
+ `git add`
+ `git commit [-m]`
+ `git push [-u]`
+ `git pull` 
+ `git clone`
+ `git remote`

#### Unused commands

- `git remove` - Removes a file (or files) from git's working branch so Git no longer tracks changes to the files (Opposite of `git add`)
- `git merge` - Combines other branches into the current working branch.
- `git checkout` - Used to change the working branch (e.g. from master to experimental if I had those two branches)
- `git branch` - Used to manage branches in your Github repo (i.e. Create, list, or delete branches)

> Note: `git pull` and `git clone` won't be used in this assignment but will be used regularly in this class.  
> These two commands are similar in nature but are slightly different.  
> Both will copy whichever remote git repo you are using, However, `git pull` will  
> continue to track changes between the remote repo that was just copied (e.g. I `pull` my code from Github and  
> then modify it, I then `push` to the same remote Github repo since the changes being tracked are linked between my  
> remote Github account and my local Git repo) and `git clone` duplicates a remote repository, but doesn't establish  
> the link between the remote repository that was copied and my local version (i.e. There isn't any link between the  
> the two repositories). For this class, I recommend using `git pull` whenever modifying code and copuing from your  
> Github accounts. You can read more on [git pull](https://git-scm.com/docs/git-pull) and [git clone](https://git-scm.com/docs/git-clone) here.


## 4. Using git

Using git will extend further than usage in this class. In this class, we will
use GitHub as a remote origin for uploading our code and for moving our code
onto our production servers. In this exercise, we will be using git to help
manage our project. You will have to create a repository (sometimes called a
'repo') on GitHub in order to test and complete this assignment. Make sure you
*__make the repository private__* otherwise everyone can look at your code in your
repo (This includes other students which means they could cheat using your
code, thus violating the honor code). Helpful documentation for using git and GitHub
can be found here on [GitHub](https://help.github.com/en/github/using-git).

### 4.1 Adding to a Remote repository

##### Problems:

Place the command used as your answer:

1. Create a new GitHub repository on GitHub.com (*__make sure it's private__*) and place your Github username here as your answer.
1. __Initialise__ a new git repository on your computer (I reccommend doing this in the CLI in a new directory).
2. Create one readme.md file (On your computer in the same directory where you initialized your git).
3. __Add__ the file so git tracks changes made to them.
4. __Commit__ changes to your repository, with a description of what this commit does (i.e. `'Add files A and B'`)
3. Make sure to add your GitHub URL as a __remote__ origin for you git repo
5. __Push__ your commit to your Master branch on GitHub.
6. Modify the readme.md by adding your name to the file.
7. __Commit__ changes to your repository, with a description of what this commit does.
8. __Push__ your newest commit to your Master branch on Github.

> Note: The first time you add a new remote origin, you need to use
> `git remote add origin https://github.com/MyUserName/MyRepo.git` and
> `git push -u origin master`. After the first time, you only need
> `git push`, and the remote origin is remembered, and the upstream branch is
> remembered. The general process after you have saved any changes is add,
> commit, push: `git add files.txt that/changed.txt`, `git commit -m 'description of change'`, `git push`.  