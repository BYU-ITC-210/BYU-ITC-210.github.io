---
title: "Homework 1: Linux-Git CLI"
---

***

*28 points possible. 28 questions; 1 point per question.*

***
## Completing and Submitting This Assignment

For each of the exercises, compose a single CLI command. After verifying that the command works, paste the command used into the answer.

Like most assignments in this class, you will likely use web sources to help you find answers. For example, you might use a Google or Bing search to find the right command-line for a particular task.

***

## Part 1: Linux Command-Line

The goal of this assignment is to have you learn and practice various Linux commands. Note that you may use these commands at different points in each term. Specifically, after this assignment you will be able to use the following commands:

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

> An easy way to determine what a command does is to type `man` and then the name of the command you are trying to learn about. For instance, `man cp` in the command line interface (CLI) will bring up a page about the `cp` command. Simply press <kbd>q</kbd> to leave that screen.

### Preparation

To do this part, you need access to a Unix-style command line shell such as __Bash__ or __zsh__. The process is different for macOS and Windows systems.

### macOS

macOS is based on the Darwin core which, like Linux, is a Unix-derived operating system. Accordingly, the shell is built-in. It is called __Terminal__ and is found under `Applications - Utilities - Terminal`. You can also use Spotlight Search or Launchpad to bring it up.

### Windows

Windows includes the classic __DOS Command Line__ and the newer __PowerShell__. But, for this assignment, you will need to install the __Linux Subsystem for Windows 2 (LSW2)__ in order to get a Linux/Unix style command line. We will be using LSW2 and Docker through the rest of the semester so it's definitely worth the effort.

If you are current on your Windows updates (build 21H1 or later) then the simple `wsl --install` command will work. Make sure you run it from an **administrator PowerShell** and that you reboot after the installation completes.

### Using the CLI

Before this course, you should have been exposed to using a command line. But, whether you have or not, you should be able to complete this assignment. Using a command line allows you to perform necessary functions when operating a computer that are not available in the graphical interface.

Here are some resources that may be helpful:

* [IT&C 210 Introduction to Command Line](IntroToCommandLine.html)
* [Codecademy Tutorial](http://www.codecademy.com/en/courses/learn-the-command-line)
* [Survival guide for Unix newbies](http://matt.might.net/articles/basic-unix)
* [Linux Survival](http://linuxsurvival.com/linux-tutorial-introduction).

Use other online resources as needed to complete the assignment. For this assignment __you will use the CLI__ to run a series of commands on a Linux-based system.

***

### 1.1 Finding Help 

##### Exercises:

1. Open the manual for `ps`.
2. Search for manual entries containing the word 'transfer'.
3. Print the current directory
4. Print the current IP address (and other network information)
5. Use the echo command to print text on the console.

### 1.2 File and Directory Manipulation

##### Exercises:

1. Make a directory called `your_first_name` (e.g, `fred`).
3. Navigate into the new directory.
4. Create two empty files, one called `your_middle_name.txt` (e.g., `george.txt`), and
   another called `your_last_name.txt` (e.g., `weasley.txt`).
   > Hint: You usually use an editor like **Notepad**, **TextEdit**, **VS Code**, **vim**, or **nano** to create or a text file. But you can create an empty file using `touch`.
5. Create another directory (inside the first) called `subdir`.
6. Copy `your_last_name.txt` and call it `your_last_name_2.txt`. (Substitute your actual last name, of course.)
7. Move `your_last_name_2.txt` into the `subdir` directory.
8. Rename `your_last_name_2.txt` to `your_last_name_3.txt`. (Make sure it stays
   in the `subdir` directory.)

### 1.3 File Permissions 
 
##### Exercises:

1. List all of the files in the `your_first_name` directory *__with__* permission details.
2. Remove all permissions for others on `your_last_name.txt`.
3. Change the owner of `your_middle_name.txt` to `root` (You will need to type in your password).

### 1.4 File Manipulation 
 
##### Exercises:

1. Add the string 'hello' to `your_last_name.txt`.
   > Hint: You can direct the output of any command to a file using the '`>`' (redirection) operator. The '`>>`' redirection operator will concatenate the output to the end of the file. Combining that with the `echo` command you can create a text file or update its contents like this: `echo Accio Firebolt > harry.txt`. 
3. Add the contents of `your_last_name.txt` to `your_last_name_3.txt` (in the `subdir` directory).
   > Hint: The `cat` command will print the contents of a file. Consider combining that with a redirection operator.
4. Print the contents of `your_last_name.txt` to the console.

## Part 2: Git Command Line

Git is a source control system to help manage versions of your projects. GitHub is an online place to store git projects. In this class, we will be using GitHub as a shared place to move our code to our production environments from our development environments.

After this assignment, you should be familiar with the following commands:

* `git init`
* `git add`
* `git commit [-m]`
* `git push [-u]`
* `git pull` 
* `git clone`
* `git remote`

### Unused commands

The following are important Git commands that you are not likely to use in this course.

- `git remove` - Removes a file (or files) from git's working branch so Git no longer tracks changes to the files (Opposite of `git add`)
- `git merge` - Combines other branches into the current working branch.
- `git checkout` - Used to change the working branch (e.g. from *main* to *experimental* if I had those two branches)
- `git branch` - Used to manage branches in your Github repo (i.e. Create, list, or delete branches)

> Note: `git clone` and `git pull` won't be used in this assignment but will be used regularly in this class. `git clone` makes a new copy of a remote repository on your local computer and links them together to track future updates. `git pull` brings in the most recent updates from the remote repository when a link is already in place. You can read more on [git pull](https://git-scm.com/docs/git-pull) and [git clone](https://git-scm.com/docs/git-clone) at the respective links.

### Using git

Using git will extend further than usage in this class. In this class, we will use GitHub as a remote origin for uploading our code and for moving our code onto our production servers. In this exercise, we will be using git to help manage our project. You will have to create a repository (sometimes called a 'repo') on GitHub in order to test and complete this assignment. Make sure you *__make the repository private__* otherwise everyone can look at your code in your repo. Helpful documentation for using git and GitHub can be found here on [GitHub](https://help.github.com/en/github/using-git).

> Windows users can either [install git for Windows](https://git-scm.com/download/win) and run Git from the Windows command line, or you can use Git from the Linux command line you created in Part 1. Since we will be using Git a lot during the semester, installing Git for Windows is something you will likely do eventually anyway.

### 2.1 Adding to a Remote repository

##### Exercises:

1. Create a new GitHub repository on GitHub.com (*__make sure it's private__*). (Paste the URL of your new repository on the answer sheet for your answer. It should start with `"https://github.com"` and end with `".git"`. You will use this for step 6.)
2. __Initialise__ a new git repository in an empty directory on your computer. (Create a directory for this purpose.)
3. Create one __README.md__ file on your computer in the same directory where you initialized your git repo and give it some contents.
4. __Add__ the file so git tracks changes made to it.
5. __Commit__ changes to your repository, with a description of what this commit does (i.e. `'Add files A and B'`)
6. Change the name of your current __branch__ to "`main`" with the command `git branch -M main` (Make sure it's a capital 'M'.)
   > Yes, we're giving away the answer on this one. The first branch has historically been called `master` but the new convention is to call it `main`. Depending on which version of __git__ you are using, the branch may already be called `main` in which case this won't do anything.
7. Add your GitHub URL as a __remote__ origin for your git repo. (See the note below.)
8. __Push__ your commit to your `main` branch on GitHub.
9. Modify the README.md by adding your name to the file. *(If you use an editor, no need to enter anything on the answer sheet.)*
10. __Commit__ changes to your repository, with a description of what this commit does.
11. __Push__ your newest commit to your `main` branch on Github.

> Note: The first time you add a new remote origin, you need to use `git remote add origin https://github.com/MyUserName/MyRepo.git` and `git push -u origin main`. After the first time, you only need `git push`, and the remote origin is remembered, and the upstream branch is remembered. The general process after you have saved any changes is add, commit, push: `git add files_that_changed.txt`, `git commit -m 'description of change'`, `git push`.  
![image](https://user-images.githubusercontent.com/76703677/147512959-3cdf9d33-c0e0-49a2-b51b-d1a7584a3fd6.png)
