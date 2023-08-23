---
title: How to Start Labs
---
Labs are created and maintained in GitHub. When you accept the lab through GitHub Classroom you will get your own copy of the lab repo including the instructions folder. As you develop your lab solution you should commit and push frequently. That will ensure that you don't lose your progress if something bad happens.

## Accept the Assignment

**This should be done before Saturday of each week. It qualifies you for the "First commit is by Saturday" requirement of each lab which is worth 5 points.**

1. Click on the `Accept this assignment through GitHub` link on the Lab Assignment in GitHub.
2. Once the repo has been created for you, click on the assignment link. It will look something like this: <a href="about:blank" onclick="alert('This is not a real link.')">https://github.com/BYU-ITC-210/lab-1b-yourgithubname</a>
3. Click on the `instructions` folder to view the instructions for the lab.

## Clone the Repository to Your Computer Using CLI

(See below for the alternative - using VS Code)

You must have Git installed on your computer before performing these steps. If you are running WSL2 then Git will be part of the Linux image. If you have installed Git for Windows then you can use a windows command line such as "Command Prompt" or PowerShell. On a Mac, you must have installed Git for macOS.

![Clone-Screenshot](/images/Clone-Github.png){: style="max-width: 20em; border: 1px solid black;"}

1. On the GitHub repository page, click on the green "Code" button.
2. Copy the repository link by clicking on the clipboard icon in the pop-up box.
3. Open a command line (WSL2, Windows Command Line, Mac Terminal, etc.)
4. Create and navigate to the folder where you intend to keep your source code.
5. Type `git clone` and paste the URL from step 2. For example:<br/>
`git clone https://github.com/BYU-ITC-210/lab-1a-yourgithubname`
6. Navigate to the cloned repo and start working!
    1. Tip: you can type "code" and VS Code will open the folder in which you are located.

## Clone the Repository to Your Computer Using Visual Studio Code

For this to work, you must install VS Code and Git for Windows or Git for macOS. But you need those tools anyway so this is a good idea.

1. On the GitHub repository page, click on the green "Code" button.
2. Copy the repository link by clicking on the clipboard icon in the pop-up box.
3. Open VS Code
4. If you have a folder open, close it.
5. Select the Source Control icon ![Source Contro Icon](/images/SourceControIcon.png) on the left.
6. Click `Clone Repository`.
7. Paste the URL you copied on step 2.
8. Choose the *parent* folder of the folder where you want the repo to be located on your computer.
9. Click `Select as Repository Destination`
10. You're ready to start working!

