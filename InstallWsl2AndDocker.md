---
title: Installing WSL2 and Docker on Windows
---
> For MacOS, follow [these instructions](https://docs.docker.com/docker-for-mac/install/).

## Installing WSL2
The Windows Subsystem for Linux 2 enables you to install a Linux distro such as Ubuntu on Windows without the overhead of a virtual machine. It's also the platform on which Docker Desktop runs Linux containers. So, you need to install LSW2 before installing Docker.

WSL2 is the successor to the original Windows Subsystem for Linux (WSL). WSL2 functions better and faster than WSL and it's required for Docker.

Follow the instructions here: [Windows Subsystem for Linux Installation Guide for Windows](https://docs.microsoft.com/en-us/windows/wsl/install)

> Important: You must have Windows 10 build 21H1 or better for the "wsl --install" command to work. If you are current with your updates, then you are good to go.

You must reboot your computer after installation before WSL2 will function.

## Installing Docker Desktop
Docker Desktop will enable you to quickly and easily use pre-configured Linux containers such as the Apache web server, MySQL, and more.

Follow these instructions: [Get Started with Docker on WSL2](https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-containers#install-docker-desktop)

* You can skip the prerequisites because you already took care of that when installing WSL2.
* The "Develop in Remote Containers using VS Code" steps are optional.