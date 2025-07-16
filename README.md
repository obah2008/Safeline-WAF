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



   inet 127.0.0.1/8 scope host lo
    inet6 ::1/128 scope host noprefixroute 
    inet 192.168.1.15/24 brd 192.168.1.255 scope global dynamic noprefixroute ens33
    inet6 fe80::20c:29ff:fe2b:77eb/64 scope link 
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
    inet 192.168.0.1/24 brd 192.168.0.255 scope global safeline-ce
    inet6 fe80::6c39:13ff:fe3b:26c4/64 scope link 
    inet6 fe80::707d:cbff:fe8c:4eed/64 scope link 
    inet6 fe80::a841:c3ff:fe3b:33a1/64 scope link 
    inet6 fe80::d4ff:2fff:feb3:c1aa/64 scope link 
    inet6 fe80::3cd7:1dff:fec4:22bb/64 scope link 
    inet6 fe80::b497:baff:fef5:a6da/64 scope link 
    inet6 fe80::4ae:9ff:fe3f:16e0/64 scope link 
obah@obah-VMware-Virtual-Platform:~$ 

