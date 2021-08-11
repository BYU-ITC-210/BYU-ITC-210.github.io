---
Title: Intro to Command Line
---
This is a little tutorial into the command line for Linux and Windows. These commands and others in the command line will help you greatly in your computing career (and in this class). The next task is to access your remote server which we will call your "live server." You will only ssh or command line access to your live server. Therefore we are doing this tutorial for those who are completely new to the command line and also for your information as an aspiring IT professional. Most of the commands we will go over will have accompanying Linux and Windows commands and both will be briefly explained. If you are proficient in the command line you may skip this introduction. Generally speaking, Linux commands also work on MacOS.

### cd - Change Directory

In the command line (Linux or Windows) the `cd` command moves you around the file system. Here are some typical uses Linux: (`$` and `>` mean the Linux and Windows command prompts respectively)

```
$cd Desktop
$cd my\ folder 
$cd /var/www
```

Here are some typical uses for Windows:

```
>cd Desktop
>cd "My Documents" 
>cd c:\Users\Tim
```

There are many things here to discuss so lets start by talking about Windows and Linux paths. Just know that for Windows they use \ for their paths and Linux uses /. Also know that paths with spaces in Windows have quotes (") while Linux escapes the space ("\ " [backslash space] escapes a space).

### Absolute and relative paths

An absolute path states exactly where the folder is you want to go from the root of the file system (c:\ for Windows or / for Linux). A relative path states where you want to go from whereever you are currently. (Your current location is usually stated in your command prompt like "tim@timonium:/var/www" or "C:\initpub\webroot").

### Current and parent directory

A single dot (`.`) refers to the current directory and a double dot (`..`) refers to the parent directory. So `cd ..` will change to the parent directory of the current directory. These are, of course, *relative paths*.

If you have multiple drives on a Windows computer, you change the current drive by typing the drive letter and a colon. For example:

```
>e:
```

Changes the current drive to "e".

### ls and dir - List Contents of a Directory

In Linux, `ls`, and in Windows, `dir` will list the contents of the directory. For Linux it is good to know two optional parameters. -a will show all contents, including hidden contents (things starting with a ".") and -l will show the long version including permissions. For example, ls -la would list all the contents of a folder in long form.

### cp and mv - Copy and Move

In Linux and in Windows `cp` is copy and `mv` is move. It is a simple command where you give the source and destination.

```
$cp fromHere.txt toHere.txt 
$mv fromHere.html /var/www/toHere.html 
$cp -r fromDirectory Desktop/
```

The first example copies the file, `fromHere.txt` to `toHere.txt` in the same directory. The second example moves the file `fromHere.html` to `/var/www/toHere.html`. The third example copies the entire folder `fromDirectory` and puts it in `Desktop` also named `fromDirectory`. If a name is not specified in the destination path the file will keep its name.

### ifconfig or ipconfig - Current IP Interface Configuration

For Linux `ifconfig` and Windows `ipconfig` will list the current information for your current IP interface configurations. This is most useful to see what your IP address is.

### man/info or -h/--help - Help for a Command

For Linux man or info or for Windows -h or --help will give documentation for how to use a command. Examples:

```
$man ifconfig 
$info chmod 
>ipconfig -h 
>netstat --help
```

### vim or nano - Linux command line text editors

Often you may want to edit a file in Linux from the command line, for instance, when you are using ssh to access a remote computer. `Vim` and `Nano` are two popular editors but you will need some pointers to use them. They are not necessarily easy, but once you have learned one you can see how they are very powerful and useful. `Nano` is more user friendly and you can probably figure out how to use it after a little bit.

`Vim` is preferred by many but needs some explanation. When you open the file you are in the "command" interface. In this interface you can move the cursor, copy, paste, delete, search ,etc but not edit the text. To get to the "edit" interface you must press `i` and you can edit where the cursor is but not move the cursor. To get back into the command interface you must press esc. To save and exit you need to be in the command interface and press :wq which means command, write, quit respectively. Vim has many other great uses. Find them on Google.

```
$vim dir.conf 
$nano identity.html
```

### sudo - Linux Super User Do

In Linux. `sudo` allows you to execute a command as the "root" user. If you see "Operation not permitted" error after running a command, make sure your command is right and put sudo in front of the command. Example

```
$sudo vim dir.conf
```

### passwd - Change password

In Linux, `passwd` changes the password of a user.

```
$passwd 
$sudo passwd tom
```

The first example will change the current user's password. It will prompt for the current password and ask for the new password. The second example uses sudo (root permissions) to change tom's password.

### apt-get - Package Manager

In Linux apt-get will use a package manager to install a program. In Linux most programs are in repositories and you just need to download them and install them through a package manager. Synaptic Package Manager is a interface for apt-get. Here is how to install or update something in the command line. (Notice you always have to use sudo for this to work.

```
$sudo apt-get install apache2 
$sudo apt-get update 
$sudo apt-get upgrade
```

> Windows users are *finally* getting a package installer. `winget` is [in preview](https://docs.microsoft.com/en-us/windows/package-manager/winget/) and should be included in Windows 11 and the next major update to Windows 10. (As of August 2021).

### chmod - Linux Change Permission

In Linux chmod will change the permissions on a file or folder. Here's some examples:

```
$chmod 664 myFile.txt 
$sudo chmod -R 775 /var/www
```

This takes a little explaining. The 3 numbers after chmod specify the permissions for the owner, group and everyone respectively. Now think of each number as 3 binary digits (so 110 = 6 or 100 = 4 or 111 = 7). The binary digits represent read, write and execute respectively. Therefore 7 (111) means read, write and execute. 6 (110) represents read and write. 5 (101) represents read and execute. 4 (100) represents read only. So chmod 664 myFile.txt sets the permissions of myFile.txt to read/write for owner and group and read only for everyone else.

`-R` means recursive (or -r for many other terminal commands). That means this folder and everything within it. Therefore, `sudo chmod -R 775 /var/www` means run this command as root, change permissions recursively to read/write/execute for owner and group and read/execute for everyone else on /var/www. Another important fact to know is a directory needs to be executable or you cannot open the directory.

### chown - Linux change owner/group

As discussed above, Linux grants permissions based on the owner and group assigned to a file or directory.  Many times the best answer is not to change the permission but, instead, change the owner or group.  Here's some examples:

```
$sudo chown tim /var/www
$sudo chown -R tim:www-data /var/www
```

The first example is changing /var/www to be owned by *tim*.  Usually sudo is required to run chown, after chown you specify the user and then what you want to change the ownership of.  The second example changes everything in /var/www to be owned by *tim* with the group of www-data. `-R` means recursive like chmod.  If you are specifying the group you do `<user>:<group>` with the : between them.  

> A note on the www-data group, this is the group the Apache (web server) process belongs to.  You can add yourself to the www-data group if you want to. This is significant because this means if you have permissions of rwxrwxr-x tim www-data then tim and www-data (or Apache) can write to that folder.  If it were rwxrwxr-x tim tim then Apache would not be able to write to that folder (or edit the file).  For this class, Apache needs permissions to write to the things in it's folder. So leave it as rwxrwxr-x tim www-data.

### ssh - Secure Shell

ssh is a protocol/program that allows you to securely remote into another computer. What this means is that when you ssh into another computer, the terminal is no longer your local computer but that remote computer as if you were sitting over there in a terminal. You are viewing the file, programs etc. of that remote server. To accomplish this the remote server needs a username and password that you know. Here's how to do it.

```
$ssh tim@rockncomputer.com 
$ssh webadmin@192.168.210.210 
$ssh -p 43224 webadmin@it.et.byu.edu
```

The first example shows `tim` logging into `rockncomputer.com`. The second is webadmin logging into the computer at IP address 192.168.120.1 which would be on the local network (due to the 192.168 prefix). The last case indicates the port to use when connecting to ssh instead of using the default (22). In any case it will ask you for a password and then you will see a prompt like `tim@rockncomputer.com:~>` or `webadmin@192.168.120.1:~>` where the `:~` means you are in that user's home directory. To get out just type exit and it will return to the local computer.

> "Shell," "Terminal," and "Command Line" all mean pretty much the same thing. (Though there are nuanced differences.)

ssh works on Windows, Mac, and Linux and you frequently use it to bridge between them. For example, you might use ssh on your Windows computer to access your live Linux server.

### scp - Secure copy

`scp` is a way to use ssh to copy files from and to remote places. This means a file on a local computer can be copied to a remote computer, visa versa, or even copy a file from a remote computer to another remote computer. It is a mix of ssh and cp. Here's how it goes.

```
$scp /var/www/index.html webadmin@192.168.210.210:/var/www/ 
$scp webadmin@192.168.210.210:~/stuff.txt ../notes.txt 
$scp -r myfolder/ webadmin@192.168.210.210:~/putHere 
$scp -P 424242 thisFile webadmin@itrocks.com:~/
```

The first example copies the file `/var/www/` from the local computer to the computer at address `192.168.210.210` using username `webadmin` and placing it in `/var/www/`. Notice that it does not specify the name of the file once it is copied, therefore it wil keep the name index.html. The second example copies a file from the remote computer at `192.168.210.210` using username `webadmn` and copies the file `~/stuff.txt` (remember ~ is the home directory). The destination for the file is on the local computer `../notes.txt` (up to the parent directory and name it notes.txt). The third example copies an entire folder from the local computer to the remote computer and names the remote folder `putHere`. The last example specifies a port number for ssh different from default port `22` on the remote computer.

### ~ - Linux user's home directory

In Linux ~ means the current user's home directory. So cd ~ on a Linux machine would take you to /home/[username].

### ./ - This directory
In Linux, sometimes you want to go to a program and then execute the program. If you have used cd to get to the program, to run it you must put ./ in front of the program you want to call. Example ./myProgram.pl will run myProgram.pl in the current directory.

### Other Useful Commands

(Look these up.)
* ping 
* tracepath (Linux) / tracert (Windows)
* netstat
* cat (Linux) / type (Windows)
* grep 
* touch 
* groups 
* adduser 
* usermod 
* The operands: `|` and `>`
