---
title: "Getting Started: Install VS Code and Git"
---

[Visual Studio Code](https://code.visualstudio.com) and [Git](https://git-scm.com/) are essential tools that you will use throughout IT&C 210 and in many other classes and projects. They also work well together. Here are the steps for installing these essential tools if you don't have them on your computer already.

It is best to install VS Code *before* Git because one of the Git settings is which editor to use and VS Code is the preferred choice.

## Installing VS Code

**Visual Studio Code** is a text editor with _TONS_ of plugins and helpers, including some built-in plugins like [emmet abbreviations](https://docs.emmet.io/abbreviations/) and [Intellisense](https://code.visualstudio.com/docs/editor/intellisense). VSCode is built with HTML and CSS, which makes it incredibly easy to create plugins, which is why there are so many even though it's only been out for a few years.

> Don't confuse **Visual Studio Code** with **Microsoft Visual Studio**. Both are from Microsoft. **Visual Studio Code** is a *source code editor* while **Microsoft Visual Studio**, is an *integrated development environment (IDE)*. 

Of course, you can use a different editor but VS Code is the one we will use in all of our demos and examples.

1. Go to the [VSCode Website](https://code.visualstudio.com/) and download the proper version for your operating system

2. During the install, if you're installing on Windows, make sure to check the following boxes:

    - Add to PATH
    - Add 'open with code' to the context menu
    - Associate file extensions with code

3. Once installed, explore the `Extensions` tab, the boxy looking icon on the left-side dock

4. For instance, look up "CSS Formatter" and install the extension of that name from Martin Aeschlimann

5. Create a folder for your project on your computer, open it in Code, and start programming!

Another important feature of VSCode is the Git integration. If you open a Git repo in VSCode, it will sense that automatically and show any changes in the Git tab (the one that looks like a little node tree). There's also a `debug` tab, but you need a debug profile to use it.

Any time you need to find some kind of VSCode command, press <kbd>F1</kbd> **OR** <kbd>Ctrl-Shift-p</kbd>/<kbd>Cmd-Shift-p</kbd> to open the "Command Palette". Type "Theme" and you'll see options for changing the editor's theme, or the file icon theme. My favorite file icon theme is `vscode-icons` which you can find in the Extensions tab. 

## Installing Git

Git is a free and open source Version Control System or Source Code Manager (the terms can be used interchangeably). It is used from the Command-Line Interface (CLI) however, many add-ons including VS Code offer a Graphical User Interface (GUI) for essential functions.

Go to the [Git Downloads Page](https://git-scm.com/downloads) and download the proper version for your operating system.

The installer has tons of options that you may adjust according to your preference. Here are the ones I use and why. They are biased toward a Windows installation. Mac and Linux have similar features.

> The Windows version of Git includes `Git Bash` which is a version of the [Bash CLI Shell](https://en.wikipedia.org/wiki/Bash_(Unix_shell)) for Windows. This can be really handy as it allows you to use Linux style commands like `ls` and `grep` in Windows. It's also essential as Git uses Bash under the covers.

|Option|Setting|Why|
|------|-------|---|
|Icons on the desktop|Off|My desktop is too cluttered. I don't like app shortcuts there.|
|Windows Explorer: "Open Git Bash here"|Off|Just my preference. I launch Bash from the Windows Terminal. But many people like this option.|
|windows Explorer: "Open Git GUI here"|Off|Again, just my preference.|
|Git LFS (Large File Support)|On|Valuable when including images and video in your repo.|
|Associate .git* configuration files with the default text editor.|On|This handy for editing files like `.gitignore`|
|Associate .sh files to be run with Bash|On|This lets you launch .sh shell scripts on Git Bash.|
|Check daily for Git for Windows Updates|Off|I prefer not to be interrupted by software updates.|
|Add a Git Bash Profile to Windows Terminal|On|This is really handy.|
|(NEW!) Scalar (Git add-on to manage large-scale repositories)|Off|It's a very new feature that runs background processes to manage extremely large repositories. I don't have repos big enough to warrant its use and I don't want the extra background baggage.|
|**Later Pages**|||
|Start Menu Folder|Git|The default works well.|
|Default Editor|Use Visual Studio Code|VS Code works really well with Git. The alternatives include `Nano` and `Vim` which, unless you're familiar with them, are hard to use.<br/><br/>**Note:** To choose this option and have it work you must instal VS Code *before* Git. If you instal Git first, you must use the "Git config" command to change the "core.editor" setting. (Look up details online.) |
|Name of the initial branch in new repositories.|Override... "main"|Historically the default branch of a new repository was named "master" but over the last eight years or so the trend is to use "main" instead.|
|Adjusting your PATH environment|Use Git and optional Unix tools from the Command Prompt|This will enable Linux commands like `ls`, `grep`, `touch`, and `tail` work from the Windows [CMD](https://en.wikipedia.org/wiki/Cmd.exe) and [PowerShell](https://en.wikipedia.org/wiki/PowerShell) shells in addition to **Git Bash**. I still use the CMD shell a lot and so having these Unix-style additions is handy. But the Unix-style commands replace some similar (but different) Windows commands like `find` and `sort`. If you're not a heavy user of CMD then the second option, "Git from the command line and also from 3rd-party software" is the best choice.|
|Choosing the SSH executable|Use external OpenSSH|This doesn't really matter much. I simply chose the external OpenSSH which comes bundled with Windows to save complexity.|
|HTTPS Transport Backend|Use the native Windows Secure Channel Library|You won't notice a difference whichever choice you make. I chose to use the Windows secure channel simply to spread risk across multiple vendors.|
|Line Ending Conventions|Checkout Windows-style, commit Unix-style|This is a historic difference that will continue haunting us for years. Unix uses `\n` to end lines while Windows uses `\r\n`. Most contemporary editors and all code compilers will tolerate either. This default setting ensures maximum compatibility but the other options should also work well.|
|Terminal to use with Git Bash|Use Windows' default console window|Since Microsoft introduced the new [Windows Terminal](https://learn.microsoft.com/en-us/windows/terminal/) a few years back this has been the best choice.|
|Default behavior of 'git pull'|Rebase|This is a complex topic about which there are many tutorials and YouTube videos. It really only becomes an issue when multiple people are working on the same project which won't happen in this class. Suffice it to say that I prefer 'rebase'.|
|Choose a credential helper|Git Credential Manager|The credential manager retains your login credentials to [GitHub](https://github.com/) so that you don't have to enter your passcode constantly.|
|Enable file system caching|On|Because it 'provides a significant performance boost.|
|Enable symbolic links|Off|Not needed and setting the permissions right may be complicated.|

## Conclusion

With VS Code and Git installed you are ready for your first lab. Continue with the instructions there.


