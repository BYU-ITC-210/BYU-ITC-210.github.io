---
title: How to Start Labs
---
IT&C 210 Labs are created and maintained in GitHub. When you accept the lab through GitHub Classroom you will get your own copy of the lab repo including the instructions folder. As you develop your lab solution you should commit and push frequently. Doing so will ensure that you don't lose your progress if something bad happens.

> **Important!** The first five points of your lab credit come from accepting the lab and making your first commit *by end-of-day Friday* before the lab is due. You must do all of these steps including committing at least one change by that time. When passing off your lab, the TAs will check your commit history in GitHub to verify that five-point credit.

If you are using your own computer to complete the lab assignments you must install [Visual Studio Code](https://code.visualstudio.com) and [Git](https://git-scm.com/) on your computer. [Follow these instructions](InstallCodeAndGit) to install these essential tools. You will also need [Docker](https://www.docker.com/). [Follow these instructions](InstallWsl2AndDocker) to install WSL2 (if on Windows) and Docker (regardless of MacOS or Windows).

## Accept the Assignment

1. Click on the `Accept this assignment through GitHub` link on the Lab Assignment in LearningSuite.
2. Once the repo has been created for you, click on the assignment link. It will look something like this: <a href="about:blank" onclick="alert('This is not a real link.')">https://github.com/BYU-ITC-210/lab-1b-yourgithubname</a>
3. Click on the `instructions` folder to view the instructions for the lab.

> **If you get an error message:**{: style="font-size: 1.5em"}<br/>
    For reasons we haven't yet identified, you sometimes get a "Repository Access Issue" error or something similar when trying to accept the lab and access the cloned repository. If this happens to you then do the following to fix the error:<br/>
    <br/>
    1. Open [GitHub](https://github.com) and log in.<br/>
    2. Click on your profile image in the upper-right corner of the page (any page).<br/>
    3. Click on **Organizations**.<br/>
    4. You should see an invitation to the `BYU-ITC-210-Students` organization. Click there to accept the invitation and you should have access to your lab repo.<br/>
    <br/>
    *If this happens to you, and you get the chance, please grab a screenshot of the error message and also of the organizations list with the "Accept Invitation" button. Then send it to your instructor or a TA along with your best memory of how it happened. We are trying to isolate the pattern that causes this problem.*

## Clone the Repository to Your Computer

You can clone the repo using the Command-Line Interface (CLI) or using the built-in Git support in [VS Code](https://code.visualstudio.com/). It's valuable to learn both methods. The Git support in VS Code is convenient and only involves a click or two. The Git CLI has more options for handling complicated matters.

![Clone-Screenshot](/images/Clone-Github.png){: style="max-width: 40em; border: 1px solid black;"}

### Option 1: Clone the Repo Using CLI

1. On the GitHub repository page, click on the green "Code" button.
2. Copy the repository link by clicking on the clipboard icon in the pop-up box.
3. Open a command line (WSL2, Windows Command Line, Mac Terminal, etc.)
4. Navigate to the *parent* folder of where you want this repo folder to appear (e.g. `source/ITC210`).
5. Type `git clone` and paste the URL from step 2. For example:<br/>
`git clone https://github.com/BYU-ITC-210/lab-1a-yourgithubname`
    1. If this is the first time you are using Git from this computer, you will be prompted to log into GitHub. Git should save your credentials and you won't have to log in for future operations.
6. Navigate to the cloned repo and start working!
    1. Tip: you can type `code` from the CLI and VS Code will open the folder in which you are located.

### Option 2: Clone the Repository to Your Computer Using Visual Studio Code

1. On the GitHub repository page, click on the green "Code" button.
2. Copy the repository link by clicking on the clipboard icon in the pop-up box.
3. Open VS Code
4. If you have a folder open, close it.
5. Select the Source Control icon ![Source Control Icon](/images/SourceControIcon.png) on the left.
6. Click `Clone Repository`.
7. Paste the URL you copied on step 2.
8. Choose the *parent* folder of the folder where you want the repo to be located on your computer.
9. Click `Select as Repository Destination`
    1. If this is the first time you are using Git from this computer, you will be prompted to log into GitHub. Git should save your credentials and you won't have to log in for future operations.

## Review the Lab Instructions

It's best to view the lab instructions on GitHub.com in the `instructions` folder of your repo. Alternatively, you can access the instructions in the copy you have cloned to your computer. The instructions are in [MarkDown](https://www.markdownguide.org/) format (something we will discuss in more detail shortly). The GitHub.com view will display them nicely. If you want to open the instructions in VS Code, use the MarkDown preview icon in the upper-right for a friendly view.

## Make An Edit

1. Make at least one change to your code. You could add a comment to source code, create a README.md file or (best of all) get started on the lab itself.

## Commit and Push Your Changes

Commit and push your at least one change to GitHub before end-of-day Friday so that you earn the first five points on your lab assignment. You can do this from the command-line or within VS Code.

### Option 1: Commit and Push With the CLI

1. Open a terminal window in the folder where your repo is located.
2. Type `git add *` to stage all of your changes.
3. Type `git commit -m "commit message"` but substitute a description of the changes for "commit message".
    1. If you forget the `-m` argument then Git will open your default editor for you to enter a message.
4. Type `git push` to push the commit to GitHub.

### Option 2: Commit and Push with VS Code

1. Open the repo folder in VS code (you have likely done so already).
2. Select the Source Control icon ![Source Control Icon](/images/SourceControIcon.png) on the left.
3. Enter a commit message in the box above the blue `Commit` button.
4. Click the `Commit` button.
    1. If you forgot to enter a commit message, VS code will open a new window for you to enter the commit message. Enter the message there and click the checkbox in the upper-right corner.
5. Click the `Sync Changes` button that replaced the `Commit` button.

## Get to Work

Follow the lab instructions to complete the lab. Take advantage of Lab Time after the class period to get tips, share with peers, and get TA help.

> **Remember:** Commit early and commit often. Inevitably you will mess something up, a computer will crash, or something will go wrong. Whenever that happens you can just return to any prior commit point and continue where you left off. Your computer could completely crash and you just `git clone` on a lab computer to continue working. Git may seem awkward at first but someday it will save your bacon!
