---
title: Setting Up the Live Server
---

On your Production Environments, you'll need a server set up in order to see your website. Find out your personal IP address
for your server (This can be found in the "Content" tab on learning suite). It will take the form `192.168.90.11`, but the last two digits may differ. For the rest of these instructions, it will be referred to as `192.168.90.<your ip>`. Replace it with your IP address.

> If you're working from your own computer, you'll need to first
[connect to the IT&C VPN](https://gist.github.com/210TAs/ce4f6313f3b73dbe1e05ae4a4d8a948f).
Then, connect with your server in a terminal using SSH.

**SSH** means **S**ecure **Sh**ell, which is a way to access a remote server via any terminal/command prompt as long as you have
the password.

1. Run the following command:

    ```sh
    ssh webadmin@192.168.90.<your ip>
    ```

    Once you are connected to the server you should be at a prompt that looks something like this:

    ```
    webadmin@<your BYU Net ID>-210:~$ 
    ```

1. Change `webadmin`'s password by running the following command:

    ```sh
    passwd
    ```
1. Install the LAMP stack using the following command:
    ```sh
    sudo apt update && sudo apt install -y php-mbstring php-zip php-gd php-json php-curl apache2 mysql-server phpmyadmin
    ```
    As you do this, phpMyAdmin will prompt you for a few things:
    
    1. Which database:
        > Please choose the web server that should be automatically configured to run phpMyAdmin.
        >
        > Web server to reconfigure automatically:
        >
        > [ ] apache2<br>
        > [ ] lighttpd
        >
        > &lt;Ok>
        
        Press <kbd>Space</kbd> to select apache2 (it should look like `[*] apache2`), then <kbd>Enter</kbd>.
    1. Use dbconfig-common:
        > The phpmyadmin package must have a database installed and configured before it can be used. This can be optionally handled with dbconfig-common.
        >
        > If you are an advanced database administrator and know that you want to perform this configuration manually, or if your database has already been installed and configured, you should refuse this option. Details on what needs to be done should most likely be provided in /usr/share/doc/phpmyadmin.
        >
        > Otherwise, you should probably choose this option.
        >
        > Configure database for phpmyadmin with dbconfig-common?
        >
        > &lt;Yes> &lt;No>
        
        Press <kbd>Enter</kbd> to select `<yes>`.
    1. Password for phpMyAdmin:
        > Please provide a password for phpmyadmin to register with the database server. If left blank, a random password will be generated.
        >
        > MySQL application password for phpmyadmin:
        >
        > _____
        >
        > &lt;Ok> &lt;Cancel>
        
        Type in a password, and press <kbd>Enter</kbd>.
        
        > Password confirmation:
        >
        > _____
        >
        > &lt;Ok> &lt;Cancel>
    
        Type in the password again, and press <kbd>Enter</kbd>.
    
    The web root (where your website will be stored) is found in `/var/www`.

    > Useful Hint: Change the owner of your Apache web root so that you (as your user and not root)
    can add and edit files in the web root (see
    [Do I Really Need to Worry about My Shared File Permissions](
    https://lifehacker.com/a-basic-introduction-to-unix-file-permissions-5963905))

You can control your server's state with `sudo service apache2 <option>`, where `<option>` is one of `{start|stop|graceful-stop|restart|reload|force-reload}`. The usages of these are fairly easy to know just by the name.

## Clone your code

1. On GitHub.com, look at your repo and click the button that says "Clone or download" and then copy the url in the box.

1. Navigate to the Apache Web Root:

    ```sh
    cd /var/www
    ```
    
1. Make yourself the owner of this directory:

    ```sh
    sudo chown -R $USER . && sudo chgrp -R $USER .
    ```

1. Delete the current `html` folder that's found there:

    ```sh
    rm -rf html
    ```
    > Note: Be VERY careful using this. You can brick your server if you `rm` the wrong file.

1. Clone your repo:

    ```sh
    git clone <the url you copied from github>
    ```

1. Create a "Symbolic Link" called `html` that points to the `src` folder in the repo that you cloned:

    ```sh
    ln -s <folder of your repo>/src html
    ```

    > Explanation: This creates a "shortcut" to your src folder. When Apache tries to access `/var/www/html`, it will be
    pointed to the `src` folder in your project now.

    __Remember this command!__ In future labs, you will need to clone more repos to your live server, and point apache to the
    right directory. This involves `rm`ing the old symbolic link, and making a new one using this command. This is so you
    don't have to make another `sites-available` file (which we'll cover next) and make Apache reload.

1. Test your production environment by navigating to your live server's IP address in a browser on a computer on the IT network.

    > Note: The only way to access these servers is while on the IT network. You must either be on a lab computer, or hooked
    into the IT network with an ethernet cable, or use the VPN.

## Change Default Config

Apache comes with a default configuration file, and as of the time of writing this, it should be called `000-default.conf`.
You'll create your own new site config.

1. Navigate to the `sites-available` directory for Apache:

    ```sh
    cd /etc/apache2/sites-available
    ```

1. Copy the default config into a new config:

    ```sh
    sudo cp 000-default.conf it210_lab.conf
    ```

1. Open your file
    Open your file in an editor as root, using
    ```sh
    sudoedit it210_lab.conf
    ```
    The default editor is `nano`, but `vim`, `emacs`, or `ed` are also standard options, if you don't have a soul. You can change which editor `sudoedit` uses by installing the editor and then running this command, following its prompts:
    ```sh
    # Optional
    sudo update-alternatives --config editor
    ```

1. Disable the directory listing in `/etc/apache2/sites-available/it210_lab.conf` or `/etc/apache2/apache2.conf` (you'll need to search the web to find out how to do this).

    > Note: Directory listing means, when you navigate to a directory in the browser without an index file
    (like the `src/css` folder, i.e. `192.168.90.<your ip>/css/`, which contains only `.css` files) it will _list_ the
    contents of the _directory_. We don't want this for your servers.

1. Make sure a user can view your website without having to specify the `index.html` file in the URL.

1. Save the new document and then run the following commands to disable the default site and enable your new one:

    ```sh
    sudo a2dissite 000-default.conf
    sudo a2ensite it210_lab.conf
    ```

1. It says you need to reload Apache, so do it:

    ```sh
    sudo service apache2 reload
    ```

## Test it out

You should be able to access your `src/index.html` file by typing in `http://192.168.90.<your ip>` on any computer on the IT
network. Typing `http://192.168.90.<your ip>/css` should say **Forbidden**, and `http://192.168.90.<your ip>/does_not_exist`
should say **Not Found**.