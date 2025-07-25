# Lab setup
## Installing Safeline
We'll be automatically deploying Safeline on the Ubuntu server by running the command below on the terminal
```bash
bash -c "$(curl -fsSLk https://waf.chaitin.com/release/latest/manager.sh)" -- --en
```
- You'll want to run the command as root
- Also if Docker is not installed on your server, the safeline CLI installer will prompt you to do so, if that happens click on yes.

![image](https://github.com/user-attachments/assets/83ddcace-152c-4dd0-b220-f81c578aad70)

- After the installation is complete copy the login credentials, then access the dashboard using the prompted address 

## Installing DVWA
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

### Mapping a domain name to the web application
Right now, to access the DVWA dashboard we'd need to use an IP address instead of a domain name, since we don't have DNS setup we'll be performing the mapping locally following the intructions below

- Use nano to open the `hosts` file
```bash 
sudo nano /etc/hosts
```
- Map the domain name to the we application
 ```bash
127.0.0.1 <name>
```
- and finally save the file

### Importing DVWA to safeline
Before we do that, import the SSL certificate that was created [here](https://github.com/obah2008/Safeline-WAF/blob/main/Generating%20an%20SSL%20certificate.md)
<img width="1362" height="699" alt="image" src="https://github.com/user-attachments/assets/2ae80aea-9e74-45e2-822c-c2a17280b73b" />

Now that we have safeline and DVWA set up as well as a configured domain name, we can import the web application to Safeline. 
- Navigate to the applications menu on the safefline dashboard
- Select Add application
- Add the domain name we created earlier
- Select Port 443 as the only listening port
- Select the SSL certificate we imported
- Put in `http://<server_ipaddress>` as the upstream
- Add an application name and click submit

<img width="1365" height="718" alt="image" src="https://github.com/user-attachments/assets/19f96078-42d4-48be-8aa3-ca53488bb73d" />


Note: Safeline also acts as a reverse proxy, and as part of the many features a reverse proxy offers safeline enables SSL termination which encrypts plain HTTP traffic coming from your web application.
