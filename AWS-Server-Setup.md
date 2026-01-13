---
title: AWS Server Setup
---
# Setting Up Your Live Server on AWS
Your Production Environments (Live Servers) will be hosted on Amazon Web Services(AWS) a popular online cloud hosting platform. 

## Using AWS Academy
1. You should have already received an email to join an AWS Academy classroom. If you have not received an email reach out to an IT210 TA. 
2. Follow the instructions in the [AWS Academy Learner Lab Student Guide PDF](AWS-Academy-Learner-Lab.pdf)

> Note: If you do not have a Canvas account you will need to create one. Your BYU Canvas account will not work. 

## Setting Up Your EC2 Instance
1. On the AWS Management Console, go to the top left `Services` drop down and select `Compute` > `EC2`.
2. On the left-hand sidebar under `Instances`, select `Instances`. This dashboard is where you can start, stop, and monitor your live server. 
3. Click the orange `Launch instances` button in the top right corner. 
4. On the `Quick Start` tab of the `Application and OS Images` section, select `Ubuntu`. The correct AMI should be selected by default, but make sure it is `Ubuntu Server 24.04 LTS` and says `Free tier eligible`. You should not need to edit any other configurations in this section.
5. For the Instance Type make sure `t3.micro` is selected. Again, this should be selected by default.
6. The `Key pair (login)` section contains information about SSH keys. **SSH** means **S**ecure **Sh**ell, which is a way to access a remote server via any terminal/command prompt as long as you have the key. An SSH key is a simple unique text document that you will store on your computer to access your EC2 instance. Treat your ssh key like a password. Do not lose your key file, do not publish your key file online anywhere. 
### Warning
>If you lose your key pair you will have to create a new server instance!
7. Click on `Create new key pair`. Name your key something useful like `IT210key`. Keep the default settings (RSA pair type and .pem key file format), then click `Create Key Pair` and store the key file somewhere safe on your computer. 
8. In the `Network settings` section, check the `Allow HTTPs traffic from the internet` and `Allow HTTP traffic from the internet` boxes
9. Click the orange `Launch instance` button in the panel on the right side of the screen.
10. Once the instance has been successfully launched, you can see it by clicking `View all instances` in the bottom right corner. 

## Connect to Your Instance

Both Windows and Mac come with an SSH client installed by default. You can verify this by opening a CLI terminal and typing "ssh". On Windows, it doesn't matter whether you use PowerShell, Git Bash, or Command Prompt.

Open a terminal and change to the directory where you saved the `.pem` file with your SSH key.

1. On the `Instances` page of your AWS console, find your running instance in the table and right-click on it. In that pop-up menu, click `Connect`. Find the `SSH client` tab and follow those instructions on your computer's terminal to SSH into your AWS machine using the SSH key you downloaded.
    > You only need the `chmod` command if you are running on MacOS or Linux. You might need to use `sudo chmod`.
2. Once you are connected to the server, you should be at a prompt that looks something like this:
    ```
    ubuntu@ip-<SOME IP ADDRESS>:~$ 
    ```

## Install Apache2

To serve your website we will need to install apache2.
1. While in your SSH AWS prompt as done above, run these two commands. The first command updates your system to the latest packages. The second command installs the Apache web server:
    ```sh
    sudo apt update -y && sudo apt upgrade -y

    sudo apt install apache2
    ```
2. You can control your server's state with `sudo service apache2 <option>`, where `<option>` is one of `{start|stop|graceful-stop|restart|reload|force-reload}`. The usages of these are fairly easy to know just by the name.
3. Run these two commands to start apache2 and make sure it is running:
    ```sh
    sudo service apache2 start

    sudo service apache2 status
    ```
4. You can now see if apache is working by going back to your AWS Instances page selecting your instance and copying and pasting either the `Public IPv4 address` or the `Public IPv4 DNS` into a browser. The default Apache2 page has a red banner across the top that says "It Works!". 
    > Note: Make sure to visit your site using HTTP not HTTPS because you do not have HTTPS set up.

## Clone your code

1. On GitHub.com, look at your repo and click the button that says "Clone or download" and then copy the URL in the box.
2. Navigate to the Apache WebRoot:
    ```sh
    cd /var/www
    ```   
3. Make yourself the owner of this directory:
    ```sh
    sudo chown -R $USER . && sudo chgrp -R $USER .
    ```
    Note:  The next three instructions will be used in most of the labs to update the live server to the new website. Make note of them or remember where to find them.
4. Delete the current `html` folder that's found there:
    ```sh
    rm -rf html
    ```
    > Note: Be VERY careful using this. You can break your server if you `rm` the wrong file.
5. Clone your repo:
    ```sh
    git clone <the URL you copied from github>
    ```
    > Unless you have saved credentials, it will prompt you for your username and password. For the username, use your GitHub username. For the password, use your GitHub **Personal Access Token** that you created in step 2.2. (Do not use your GitHub password.)
6. Create a "Symbolic Link" called `html` that points to the `src` folder in the repo that you cloned:
    ```sh
    ln -s <folder of your repo>/src html
    ```
    > Note: Remove the < and > symbols when replacing the folder name. This applies to anytime < and > are used.  
    
    > Explanation: This creates a "shortcut" to your src folder. When Apache tries to access `/var/www/html`, it will be
    pointed to the `src` folder in your project now.

    ### __Remember this command!__ 
    In future labs, you will need to clone more repos to your live server and point apache to the right directory. This involves `rm`ing the old symbolic link and making a new one using this command. This is so you don't have to make another `sites-available` file (which we'll cover next) and make Apache reload.
    
## Change Default Config
Apache comes with a default configuration file, and as of the time of writing this, it should be called `000-default.conf`.
You'll create your own new site config.

1. Navigate to the `sites-available` directory for Apache:

    ```sh
    cd /etc/apache2/sites-available
    ```

2. Copy the default config into a new config:

    ```sh
    sudo cp 000-default.conf it210_lab.conf
    ```

3. Learn how to open your file with a text editor
    To open your file in an editor as root, using
    ```sh
    sudoedit it210_lab.conf
    ```
    The default editor is `nano`, but `vim`, `emacs`, or `ed` are also standard options. You can change which editor `sudoedit` uses by installing the editor and then running this command, following its prompts:
    ```sh
    # Optional
    sudo update-alternatives --config editor
    ```

4. Disable the directory listing in `/etc/apache2/sites-available/it210_lab.conf` or `/etc/apache2/apache2.conf` (you'll need to search the web to find out how to do this).

    > Note: Directory listing means when you navigate to a directory in the browser without an index file
    (like the `src/css` folder, i.e. `<your ip>/css/`, which contains only `.css` files) it will _list_ the
    contents of the _directory_. We don't want this for your servers. **This is case sensitive.**

   > Be careful when editing `conf` files. Making incorrect changes could break apache and stop it from running. Make a copy of any `conf` files before you make changes so there is a backup in case something does go wrong. 

5. Make sure a user can view your website without having to specify the `index.html` file in the URL.

6. Save the new document and then run the following commands to disable the default site and enable your new one:

    ```sh
    sudo a2dissite 000-default.conf
    sudo a2ensite it210_lab.conf
    ```

7. It says you need to reload Apache, so do it:

    ```sh
    sudo service apache2 reload
    ```

## Test it out

You should be able to access your `src/index.html` file by typing in your AWS EC2 instance IP address in any browser. Typing `http://<your ip>/css` should say **Forbidden**, and `http://<your ip>/does_not_exist`
should say **Not Found**.
 
## Apache Error Logs
 
Apache error logs contain information about errors that occur while the web server is running. This can include issues with server configurations, failed requests, and problems with specific files or directories. By reviewing the error logs, a system administrator or developer can troubleshoot and fix issues with the web server and ensure that it is running correctly. Additionally, Error logs can also provide insights on security issues, for example, if there are repeated failed login attempts, which could indicate a brute force attack.
 
If you have issues with the website not displaying correctly, the apache error logs are a good place to look.
 
1. Look up how to find and read apache2 error logs.
