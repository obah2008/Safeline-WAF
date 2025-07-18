# Safeline-WAF

## Overview 
In this project I'll be setting up two virtual machines, One an Ubuntu server which will be hosting DVWA(Damn Vulnerable Web Application), and another Kali linux, our attacker machine. I'll also be configuring a safeline web application firewall to fend off the attacks from the kali linux virtual machine. 

### What is a WAF
A web application firewall is simply a type of firewall which filters malicious HTTP traffic, thereby preventing attacks like SQL injection, cross site scripting or Cross-site request forgery.  
Safeline is a free and open source Web application firewall created by Safepoint.

## Prerequisites
The projects assumes you have both virtual machines running on virtual box and properly configured such that they can communicate with each other.

## Lab setup
### Installing Safeline
We'll be automatically deploying Safeline on the Ubuntu server by running the command below on the terminal
```bash
bash -c "$(curl -fsSLk https://waf.chaitin.com/release/latest/manager.sh)" -- --en
```
- You'll want to run the command as root
- Also if Docker is not installed on your server, the safeline CLI installer will prompt you to do so, if that happens click on yes.

![image](https://github.com/user-attachments/assets/83ddcace-152c-4dd0-b220-f81c578aad70)

- After the installation is complete copy the login credentials, then access the dashboard using the prompted address 

### Installing DVWA
Before installing DVWA we'll have to set up Apache2 the webserver we'll be running DVWA on we'll also be installing a bunch of packages we'll be using in the project.
```bash
sudo apt install apache2 mariadb-server php php-mysqli git unzip -y
```
the above command installs the following packages:
- apache2
- mariadb-server
- php
- php-mysqli
- git
- unzip

After the installation is complete we can confirm if apache2 has been successfully installed by running the command below:
```bash
sudo systemctl status apache2
```
<img width="1364" height="743" alt="image" src="https://github.com/user-attachments/assets/9e2fbd60-d413-4ad3-a1fd-592321981651" />

#### What is DVWA? 
DVWA is an intentionally vulnerable web application that Uses MySQL and runs on PHP, primarily used by students while learning about web application vulnerabilities.



#### Cloning DVWA to Apache's Web directory
Use the command below to open Apache's default web directory and clone DVWA to it from [here](https://github.com/digininja/DVWA.git)
```bash
cd /var/www/html
sudo rm index.html && sudo git clone https://github.com/digininja/DVWA.git 
```
- Manually mapping the web application IP address to a domain name:

- Next step we'll be createing a MySQL database using the MariaDB shell.
1. Open the MariaDBshell using `sudo mysql -u root -p`
2. Run the command below to Create the database, create a new user and set a password for that user
```SQL
CREATE DATABASE dvwa;
GRANT ALL PRIVILEGES ON dvwa.* TO '<your_username>'@'localhost' IDENTIFIED BY 'Your_password'>;
FLUSH PRIVILEGES;
EXIT;
```
- Create and Configure DVWA’s Config File:
1.	Go to DVWA’s config directory
```bash
cd /var/www/html/config
``` 
2. 	Make a copy of the default config template
```bash
sudo cp config.inc.php.dist config.inc.php
```
- Edit the config file
1. open the config file
```bash
sudo nano config.inc.php
```
2. Look for the lines below and edit them
```php
$_DVWA[ 'db_user' ] = 'root';
$_DVWA[ 'db_password' ] = 'p@ssw0rd';
```

3. Change the login credentials to the one we set in the database earlier
```php
$_DVWA[ 'db_user' ] = '<your_username>';
$_DVWA[ 'db_password' ] = 'Your_password'>;
```

- We can now access the DVWA dashboard by pasting the servers IP address into a browser
<img width="1365" height="666" alt="Screenshot 2025-07-16 093449" src="https://github.com/user-attachments/assets/ef7d0e52-eea8-4ec7-9108-b5ada5a7d1e9" />

### Importing DVWA to safeline
Now that we have safeline and DVWA set up and configured we can imp
