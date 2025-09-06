# Lampstack Implementation on AWS
# Overview
# This LAMP stack project is a web service consisting of the following:
* Linux - Operating system
* Apache - Web server
* MySQL - Database
* PHP - Scripting Language
The Lamp stack was deployed on Amason EC2 instance running on Ubuntu
# 1. Prerequisites
* An AWS account
* Basic knowledge of Linux commands
* A key pair for SSH access
* AWS security group rules allowing;
  * HTTP (Port 80)
  * SSH (PORT 22)
* Local terminal (Git bash)
# 2. Step by Step Implementation
# Step 1: Launch an EC2 instance
 * Log into the AWS Management console to setup the EC2 Instance
 * Search for EC2 on the search bar
 * Click on Launch Instance
 * Enter the name of the web server
 * Choose Ubuntu server 22.04 or the latest version
 * Select an instance type
 * Configure security group to allow HTTP, HTTPS, SSH
 *  Launch and download the .pem key pair.
 *  Configure the storage to what you prefer
 *  Click on launch instance
 *  <img width="1919" height="891" alt="Screenshot 2025-09-03 161556" src="https://github.com/user-attachments/assets/cbd4ed03-e93e-41c4-bf28-017d14d2151f" />
 * Check status to confirm if the instance has been launced and running
 * <img width="1899" height="357" alt="Screenshot 2025-09-06 113101" src="https://github.com/user-attachments/assets/7759a01e-e49e-4575-b8ee-58c2fe78f9c3" />
 * Copy the public IP Address of your instance
 * <img width="1919" height="811" alt="Screenshot 2025-09-06 113704" src="https://github.com/user-attachments/assets/b3938bb9-7754-4fa5-ac04-8b63d49e6d6e" />
# Step 2: Connect to your instance
* Open your terminal (Git bash)
* <img width="388" height="148" alt="Screenshot 2025-09-06 122336" src="https://github.com/user-attachments/assets/b980f396-26e0-4b7b-81ed-f03b09985738" />
* cd downloads
* <img width="390" height="161" alt="Screenshot 2025-09-06 120017" src="https://github.com/user-attachments/assets/1973719b-676e-4b94-9968-4cf6f2c39102" />
* ssh -i lamp.pem ubuntu@<EC2_Public_IP>
* <img width="674" height="789" alt="Screenshot 2025-09-06 120104" src="https://github.com/user-attachments/assets/217b7b2e-9120-4b30-bd36-b6c233bb1955" />
# Step 3: Update the system
sudo apt update
# Step 4: Install Apache Web Server

Install Apache Web server by typing the commands below:

sudo apt install apache2

Sudo systemctl enable apache2

sudo systemctl start apache2
<img width="1823" height="822" alt="Screenshot 2025-09-03 165445" src="https://github.com/user-attachments/assets/fb8d2d42-340f-461e-bb66-2e319a1076f4" />

Test : Visit http:// <EC2 Public IP> in a browser

<img width="975" height="767" alt="Screenshot 2025-09-03 170733" src="https://github.com/user-attachments/assets/81268c14-32e4-43cf-a352-ec10871aa206" />

# Step 5: Install MySQL

Now that your web server is up and running, you need to install the database system to store and manage data for your site. MySQL is a popular database management system used within PHP environments.

Install MySQL by typing the command below:

sudo apt install mysql-server

When propmted, confirm installation by typing Y, and then Enter.
<img width="1883" height="710" alt="Screenshot 2025-09-03 173313" src="https://github.com/user-attachments/assets/2050407e-6374-4611-91de-faa0226e516b" />

The above shows that MySQL has been installed also active.

Log into MySQL by typing the command below:

sudo mysql

<img width="783" height="422" alt="Screenshot 2025-09-04 121715" src="https://github.com/user-attachments/assets/654288fb-4557-4f88-8a0e-10bc79c14b6a" />


<img width="1619" height="1023" alt="Screenshot 2025-09-03 175018" src="https://github.com/user-attachments/assets/f7661109-84bf-4cb5-a637-41c91779a40e" />

Change the root user authenication method to the one that uses a password by typing the command below:

 * ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';

After making this change, exit the MySQL Prompt by typing:

 * exit

Start the interactive script:

 * sudo mysql_secure_installation

This will ask you if you want to configure and VALIDATE PASSWORD PLUGIN. Type y, if you answer you will be asked to select a alevel of password validation'

<img width="1619" height="1023" alt="Screenshot 2025-09-03 175018" src="https://github.com/user-attachments/assets/3c80b546-7220-4c96-a385-84314ae7f1fa" />

# Step 6: Install PHP

You have Apache installed to serve your content and MySQL installed to store and manage your data. PHP is the component of our setup that will process code to display dynamic content to the final user. In addition to the php package, you’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. You will also need libapache2-mod-php to enable Apache to handle PHP files. Core PHP packages will automatically be installed as dependencies.

To install these packages, run the following command:

sudo apt install php libapache2-mod-php php-mysql

To check PHP version, type the command below:

php iv

<img width="751" height="264" alt="Screenshot 2025-09-04 121940" src="https://github.com/user-attachments/assets/a5a74c33-ec24-49ea-8e65-a5a68ae9cc6a" />

# Step 7: Creating a Virtual Host for your website using Apache

* Creat a directory for your website by running the command below:
  
sudo mkdir /var/www/projectlamp

* Next, assign ownership of the directory with the $USER environment variable, which will reference your current system user:

sudo chown -R $USER:$USER /var/www/projectlamp

* Then, create and  open a new configuration file in Apache’s sites-available directory using either vi or nano. Here we will use vi.

sudo vi /etc/apache2/sites-available/projectlamp.conf

* Press i and paste the folowing into the blank file:

<VirtualHost *:80>

        ServerName projectlamp
        
        ServerAlias www.projectlamp
       
        ServerAdmin webmaster@localhost
       
        DocumentRoot /var/www/projectlamp
       
        ErrorLog ${APACHE_LOG_DIR}/error.log
       
        CustomLog ${APACHE_LOG_DIR}/access.log combined
        
  </VirtualHost>


To save and close the file completely:
  * Hit the esc button on the keyboard
  * Type :
  * Type wq. W for write and q for quit
  * Hit Enter to save the file
    
With the above VirtualHost configuration, we are telling Apache to serve projectlamp using /var/www/projectlamp as its web root directory.

To enable the new virtual host, run the command belwo:

sudo a2ensite projectlamp

To disable Apache’s default website, run the command below:

sudo a2dissite 000-default

To make sure your configuration file doesn’t contain syntax errors, run the following command:

sudo apache2ctl configtest


Finally, reload Apache so these changes take effect:

sudo systemctl reload apache2

# Step 8: Test PHP Processing on your Web Server

Create a test file for your empty web root

<img width="549" height="165" alt="Screenshot 2025-09-06 142700" src="https://github.com/user-attachments/assets/ee372b73-6f9c-4fe2-8ac4-11e370643b04" />

Visit your browser and try to open your website URL using IP address:

http://<EC2_Public_IP_address>:80

If you see the text from 'echo' command you wrote to the index.html file, then it means your Apache virtual host is working.

# Step 9: Enable PHP on the Website

To change the behavior of the DirectoryIndex, run the command below:

sudo vim /etc/apache2/mods-enabled/dir.conf

Change: DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm

To: DirectoryIndex index.php index.html index.cgi index.pi index.xhtml index.htm

Save and close the file, then reload Apache with the command below:

sudo systemctl reload apache2

# Create a new file called index.php inside the web root

Run the command below to create index.ph:

vim /var/www/projectlamp/index.php

Add the following text inside the file:

<?php

phpinfo():

?>

Save and close the file, refresh the page and you will see this:
<img width="1001" height="865" alt="Screenshot 2025-09-06 105858" src="https://github.com/user-attachments/assets/dd60ce2d-e130-4273-9487-d1e826cd936d" />




