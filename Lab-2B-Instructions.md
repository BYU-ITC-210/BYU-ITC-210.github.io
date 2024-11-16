# Lab 2B - Secure Web

DNS, HTTPS, and Certificates

### Overview

This lab focuses on creating a DNS entry for a server, obtaining a TLS certificate (a.k.a. SSL certificate), and enabling HTTPS. By completing this lab, you will gain practical experience in listing services on the DNS and in setting up secure web connections.

### Functionality

1. Create a DNS entry for your server.
2. Obtain an SSL certificate using Certbot.
3. Configure your web server to enable HTTPS with the obtained certificate.

### Concepts

1. DNS (Domain Name System)
2. SSL (Secure Sockets Layer) certificate
3. HTTPS (Hypertext Transfer Protocol Secure)
4. Bash scripting for task automation

### Technologies

- DNS - Domain Name System
- HTTPS - HyperText Transport Protocol Secure
- Digital Certificates
- Public and Private Keys
- Bash Scripts

## Resources

- [What is a DNS Record?](https://www.cloudflare.com/learning/dns/dns-records)
- [DNS Record Types Explained](https://phoenixnap.com/kb/dns-record-types)

## Step 1: DNS Record

A TLS certificate must be issued on a domain name. It may not be issued on an IP address. So first, you must give a DNS domain name to your server. We have set up a DNS registrar and service for the class on which you can claim domains. The class DNS is `.itc210.net`. So, if you claim the domain "jorg", the full DNS name will be `jorg.itc210.net`. Once you have claimed a domain, you can create DNS records on that domain.

1. Claim a domain name
    - In LearningSuite go to IT&C 210A and select `Content > DoRe DNS Registrar > IT&C 210 DNS Registrar`.
    - Click `Add Record`.
    - For Action select `Claim Domain`. Choose a domain name you will use for this lab, enter something for context and click `Submit`.
> Note: You may claim up to six domains for in IT&C 210A and 210B combined.
> Note: In operation, the domain name you choose will be followed by `.itc210.net`
    - If you attempt to claim a domain someone else already owns, you'll get an error message. Just try another name.
> Note: You may create records on subdomains of the domain you claim. For example, if you claim `jorg` you can create a record on `www.jorg.itc210.net` or `sub2.sub1.jorg.itc210.net`.

2. Get Public IPv4 Address
    - Before we can link the production server to a domain we must establish a static IP.  
    - In the AWS console under the `Network & Security` section select `Elastic IPs`
    - On the right hand side select `Allocate Elastic IP address`
    - Leave the default settings and select `Allocate`
    - Select the IP address under the `Allocated IPv4 address` column
    - Select `Associate Elastic IP address`
    - In the text box labeled `Instance` select the name of the instance you want to be associated with the static IP address and then select `Associate`
    - To check that this worked see if your static ip address matches the public IPv4 address in your instance summary.
3. Assign your domain name to your server's IP address
    - In LearningSuite go to IT&C 210A and select `Content > DoRe DNS Registrar > IT&C 210 DNS Registrar`'.
    - Click `Add Record`.
    - Create an `A Record` This will map your domain to the public IP.
    - Enter the domain you claimed in step 1 into the appropriate section.
    - Enter the IPv4 address of your public IP in the next field, then hit `Submit`. 

> Note: **A** records are used to map a domain to a specific IP. **CNAME** records are another way of mapping domains, but they do not directly map to an IP address. Instead these records must point to another domain name called the "target domain." CNAME records are useful for making alternate domains for a server, but they should be connected a domain with an **A** record to reduce potential overhead. If you create a CNAME record on a domain you cannot create any other DNS records on that domain.
> For example, if you create a CNAME record on `www.jorg.itc210.net` then you can't create an A record on that same domain name. Nevertheless, you could put an A record on `jorg.itc210.net` and a CNAME record on `www.jorg.itc210.net`.

4. Verify that you can access your server by its domain name. Enter  `<your_domain>.itc210.net` into the url of your web browser.

## Step 2: TLS Certificate

Digital certificates indicate that you are the legitimate owner or administrator of a domain name. They are issued by [Certificate Authorities](https://en.wikipedia.org/wiki/Certificate_authority) (CAs) that are trusted by all or most web browsers. Most CAs charge for issuing certificates because they must verify that you are who you claim to be and that you have control over the domain for which you are requesting a certificate.

For our class we will use the [LetsEncrypt](https://letsencrypt.org/getting-started/) CA which offers certificates for free. To keep costs down, all that LetsEncrypt certifies is that you have control over your domain. They do this using a small application called `Certbot` that you run on your web server. While other certificates may last one to two years, Let's Encrypt certificates expire after only 90 days. However, Certbot automatically renews certificates so you don't have to manually install new ones.

1. Server Set Up
    - Update and upgrade your server 
     ```
    sudo apt update -y && sudo apt upgrade -y
    ```
    - We now need to create a TLS certificate that will be used by the front and back end
        - It will first ask for an email. Enter an email of your choice.
        - After entering an email, when it asks for your domain, make sure you enter it as just `<your_domain>.itc210.net` and nothing else

     ```sh
    sudo snap install core; sudo snap refresh core
    sudo snap install --classic certbot
    sudo ln -s /snap/bin/certbot /usr/bin/certbot
    sudo certbot --apache
    ```
    
    - Since the SSL certificate is owned by root we need to change it to be owned by ubuntu so apache can use it.
    ```sh
    sudo chown ubuntu -R /etc/letsencrypt
    ```

    - Note: This will verify your domain, install a cert, create a method to auto renew, and configure HTTPS. Still walk through step 3 to understand changes in your configuration files. They may be named slightly differently.

## Step 3: Inspect the Configuration files

- Locate the configuration file for your web server.
- Go to conf file at `/etc/apache2/apache2.conf` This is the main config file for apache2. We won't modify this file but it is a good reference if you want to understand the structure of the conf files and the tools apache uses to enable them.
- Certbot enabled the ssl mode and created an ssl.conf file in your `/etc/apache2/sites-available` directory based on your `it210_lab.conf` file. This file references the certificate and its key. We also associate the domain so that the server will know which ssl certificate to use for your domain if multiple ssl.conf files are active.
- Hint: Certbot allows you to find the path to all your LetsEncrypt certificates and keys with the following command: 
    ```sh
    sudo certbot certificates
    ```
- Certbot also edited the bottom of your it210_lab.conf to redirect HTTP traffic to your domain to HTTPS

- Visit your domain using HTTP and HTTPS to verify a secure connection.

## Step 4: Other DNS Records

**Create a CNAME record for a new domain.**

- Create a CNAME record assigned to a domain with an A record.
> Note: the CNAME and A Record cannot be the same name unless used with another subdomain. Your CNAME record does not need an A record but it must be tied to a domain that has an A record. 


> Note: You can visit your website with the alternate url just created, but your browser will recognize it as unsecure and attempt to block it.

- Redirect the new domain - typically you would redirect an old domain to a new domain, but this is for practice.
  - Go to `/etc/apache2/sites-available` and sudo into `it210_lab.conf`. Insert the following code under `RewriteEngine On` 
```sh
RewriteCond %{SERVER_NAME} =<your_alias_domain>.itc210.net
RewriteRule ^ https://<your_domain>.itc210.net%{REQUEST_URI} [END,NE,R=permanent]
```
- Now go to `it210_lab-le-ssl.conf` and add 
```sh
RewriteEngine on
RewriteCond %{SERVER_NAME} =<youraliasdomain>.itc210.net
RewriteRule ^ https://<yourdomain>.itc210.net%{REQUEST_URI} [END,NE,R=permanent]
```
   - We do this so that redirects will be issued if requests are made to port 80 or 443.

- Go back to the class DNS and create a TXT record on your first domain. Assign the value to `I love 210`. TXT files can be used for specific messages, verfication of domain ownership, email verification and other tasks.

- Find your TXT record by opening a new Ubuntu terminal and use the following command
```sh
nslookup -type=txt <yourdomain>.itc210.net
```

## Step 5: Automated BASH Script for Server Setup

### What is Bash ?

Bash, which stands for Bourne Again SHell, is a popular command-line interpreter widely used on Linux and macOS systems. It allows users to interact with the operating system by typing commands into a terminal. A Bash script is a text file containing a series of commands that Bash can execute. When you run a Bash script, it automates the execution of these commands in the specified order. This automation makes it incredibly useful for repetitive tasks, such as system updates, and software installations. For this lab you will be using it to configure your server as well as clone your repository and obtain a TLS certificate for your domain.

Your script will be an exercise in how to do scripting but it's also a safety net. If, later in the class, your server gets messed up, all you have to do is run your script to configure a new, operational server.

1. On AWS create another Server Instance to use your bash script on. follow the same set up as in lab 1a in `setting_up_aws_live_server.md` until you reach the section called `Install Apache2`. Stop there. This will take you to the step where you are logged into the machine. 
2. Once you are logged in create your bash script and name it something like `setup_server.sh` in your main folder. This script will contain the instructions from `setting_up_aws_live_server.md` as well as the steps to get a TLS Certificate as shown in `Step 2` of this lab. The `domain` that you will use for this script will be a new domain with a corresponding `A Record`, this way you will have 2 domains with `TLS certificates`, as well as a CNAME to the first domain.

> Note: you will end with 3 claimed domains, 2 **A** Records, and 1 CNAME record. 

- This is what your bash script should do
    - Your bash script will need to update and upgrade your server
    - install apache2
    - start apache and check the status
    - change the owner of /var/www
    - remove symbolic `html` link
    - clone github repository for lab-2b
    - create symbolic `html` link for the cloned github repo
    - navigate to the sites-available directory
    - copy 000-default.conf to it210_lab.conf
    - execute the a2dissite and a2ensite command
    - reload apache2
    - include all of the commands in step 2 of this lab

This the command that you will use to start your code for bash. 
```
#!/bin/bash
```
Run this command before you execute your bash script.
```
chmod +x setup_server.sh
```
This command will execute the bash script.
```
./setup_server.sh
```

```The 2 domains with an **A** Record will need a TLS Certificate. Meaning both domains with 'A Records' need a TLS certificate, but on different servers.```


> Note: Ensure that it210_lab.conf is correctly configured and available in /etc/apache2/sites-available.
Running scripts with sudo can be risky, especially when downloading and executing commands from the internet (like cloning a repo). Make sure your sources are secure and you understand each command's impact.


# Tips

1. Remember that the `<` and `>` characters included with the example code snips above should always be deleted. 

## Certbot Tips

1. Follow the Certbot instructions carefully to successfully obtain the SSL certificate.
1. LetsEncrypt is special because they make configuration easy, other cert authorities do not provide this luxury. 
1. If you are testing the redirect feature remember to clear your browser's cache

## DNS Tip

1. If you have an error creating a CNAME record, make sure that you claimed the new domain. Also check that the new domain has no other DNS records associated.

# Passoff
> Note: Passoff will be in person with a TA

- [ ] 5 points - First commit is on or before Friday.
- [ ] 5 points - You have a custom domain on .itc210.net
- [ ] 5 points - One of your custom domains has a CNAME record
- [ ] 10 points - Other custom domain has an A record for your live server
- [ ] 5 points - One of your custom domains has a TXT record with a note in it
- [ ] 10 points - You have a TLS certificate on your website for both domains with an **A** Record and you can access it using https://yourdomain.itc210.net.
- [ ] 10 points - Your website redirects http access to https
- [ ] 5 points - You verify DNS entries using nslookup
- [ ] 10 points - Bash Script runs without Errors
    - Your bash script will need to update and upgrade your server
    - install apache2
    - start apache and check the status
    - change the owner of /var/www
    - remove symbolic `html` link
    - clone github repository for lab-2b
    - create symbolic `html` link for the cloned github repo
    - navigate to the sites-available directory
    - copy 000-default.conf to it210_lab.conf
    - execute the a2dissite and a2ensite command
    - reload apache2
    - include all of the commands in step 2 of this lab

## Extra Credit

> Note: TAs cannot help you with extra credit!
> After you have completed all passoff requirements you can receive extra credit at any time before the end of the term. 

- [ ] 8 points - Create an MX record on your domain and receive email to that address. To do so, you will need an email server configured to accept email to your domain. (Hint: Mailinator.com will accept email sent to any address or domain. See [this blog post](https://jasonpearce.com/2014/08/21/create-your-own-personal-mailinator-alternate-domain/).)

# Writeup Questions

- What is the difference between http and https?
- What does the A record do in your DNS domain?
- Which key does the `certbot` tool send to Let's Encrypt to be embedded in the certificate; the public key or the private key?
- What is the TTL setting in DNS, what are the units, and what does it do?
- The DNS registrar tool is new this year. What did you like about it? What could we do to improve it? (Any answer gets full credit.)
- How would you incorporate  bash scripts in your future?
