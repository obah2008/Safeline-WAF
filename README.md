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


### Installing DVWA
What is DVWA?— It's an intentionally vulnerable we application that Uses MySQL and runs on PHP, primarily used by students while learning about web application vulnerabilities.
