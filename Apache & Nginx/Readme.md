# Installing NGINX, MySQL, PHP and WordPress on Ubuntu

In this project I will install WordPress on Ubuntu 20.04. To complete that I will first install NGINX, then PHP, MySQL, and finally WordPress.


### Background

NGINX is the fastest growing and most popular web server. NGINX is a powerful web server, reverse proxy, and load balancer known for its high performance, stability, and scalability. It’s often used to serve web content, handle incoming traffic, and distribute it across multiple servers.

PHP is a widely-used open-source scripting language designed for web development. From creating dynamic web pages, PHP is now used to develop desktop applications. PHP is known for its ease of use, flexibility, and broad support across different operating systems and web servers.

MySQL is the most widely adopted open source relational database and serves as the primary relational data store for many popular websites, applications, and commercial products. Are you looking to learn more about MySQL, check my lab.

WordPress is used for creating websites, blogs, and even some web applications. It has become a popular and powerful content management system (CMS) and its flexibility allows experienced developers to create complex websites and applications.

### Prerequisite

For this project, you need:

- A running Linux Ubuntu (22.04 LTS).
- sudo privileges on the system.

### DevOps Project Outline

As a DevOps engineer, I need to install WordPress on Linux Ubuntu distribution, in order to allow my team to test an application through the web interface. To complete this project you will install NGINX, MySQL, PHP, and finally WordPress.

### Let’s have fun.

#### Step 1: Install the NGINX Web Server

- Log in to Ubuntu and open the terminal.

- Type sudo apt update to update the local list of available packages and package versions in your Linux repositories. Then enter Y to confirm.

- Type sudo apt install nginx to instal NGINX.

- After the installation, typesystemctl status nginxto check the status of NGINX. If it’s active (running), you have installed nginx successfully.

- Run another test on NGINX to see if it’s running properly. Enter the command curl http://localhost or curl http://127.0.0.1/to check

#### Step 2: Install and Configure the MySQL Database Server

- Enter sudo apt install -y mysql-serverto install MySQL.

- After installing MySQL, let’s make sure it’s running. Entersystemctl status mysqlin the command line.

- Use the mysqladmin command to create a database named “wordpress”. Typesudo mysqladmin create wordpressin the terminal.

- Connect to the MySQL server using the mysql client. 

- Enter sudo mysql.

- Next, use theCREATE USERcommand to create a database user named “wordpress”. Use theGRANT ALL command to allow full permissions to the “wordpress” database.

- Before we install PHP, let’s run a test on MySQL. Type mysql -u wordpress -p wordpress(-u is for calling user and -p is for calling password)

- If you see the screenshot below, then it’s successful.

- If not, you should see the message below. In that case, you either re-type the command mysql -u wordpress -p wordpressor reset the password with SET PASSWORD for wordpress@localhost

#### Step 3: Install PHP

WordPress is written in PHP, so we need to install PHP. We will also install many packages at once this time.  Specifically, we’re going to install PHP FPM.

- Type sudo apt install -y php-fpm php-mysql php-curl php-mbstring php-imagick php-xml php-zip in the terminal to install PHP FPM and different modules.

WordPress requires several PHP modules to function correctly. They are:

+ MySQL for connecting to the MySQL database.
+ cURL for making remote requests.
+ Mbstring to handle multibyte strings.
+ ImageMagick to perform actions such as image resizing.
+ XML to provide XML support.
+ Zip to unzip plugins, themes, and WordPress update packages.

- Finally, type systemctl status php8.1-fpm.servicein the terminal to check the status. PHP is active and running.

#### Step 4: Configure NGINX

We need to tell NGINX to send all PHP requests to PHP FPM for processing. To do this, update the default NGINX configuration. 

- Type the commandcd /etc/nginx/sites-availablein the terminal.

- Before making any changes, let’s make a backup. So it’s easy to return to the initial file if there is a mistake. Type sudo cp default.bakin the terminal.

- Use the editor of your choice to make changes to the file. I will use Nano.

First, locate this line “Add index.php to the list if your are using PHP”, change this line from

To


Second, scroll down again, and locate location/{ .Change the line from

To


The new line allows you to use pretty permalinks in WordPress.

Still in Nano, change these lines from

to


We made so many changes to the configuration, it’s time now to check for errors. Enter sudo nginx -t , it may ask your password.

Because we made a configuration change to NGINX, we need to tell nginx to reload its configuration.

Launch a test to check the reload.

Successful test! Return to the home directory: type these commands cd, then pwd.

#### Step 5: Download WordPress

Now, download WordPress. You can do that directly from your Linux system by using the curl command. Curl is mostly used to transfer data over a network such as downloading a file. The “-O” option causes the file to be saved locally with the same name that was used on the remote system.

#### Step 6: Extract WordPress and Move It Into the DocumentRoot

Now that you’ve downloaded WordPress, it’s time to extract its contents and place them into the DocumentRoot. By default, the DocumentRoot is /var/www/html, so that’s where you’ll place the files.

Then


#### Step 7: Assign File Permissions for WordPress

Assign File Permissions for WordPress. In a later step, you will use the WordPress web installer. You will answer some questions and WordPress will write a configuration file based on those answers. In order to allow it to write to the file, it needs the proper permissions. The web server runs as the “www-data” user, so one simple way to give the web application the proper permissions is to change the ownership of the configuration file to “www-data”. Do that with this command:

You should see this:


#### Step 8: Determine Your IP Address

- Type ip a to determine the IP address of your server.

Note: The first network interface is named “lo” and it’s the loopback interface. Typically, the second network interface listed is the one that has the system’s primary IP address assigned to it.

#### Step 9: Complete the Web Application Install 

- Now that the server configuration is complete, let’s move to the web interface.

- Open up a web browser on your local machine and type in the IP address of your server.

- On the screen that appears select your language and click “Continue.” On the next screen, simply click “Let’s Go!”

- Next, fill in the database connection details as follows:
Database Name: wordpress
Username: wordpress
Password: wordpress123
Database Host: localhost
Table Prefix: wp_
Click the “Submit” button. On the next screen, click “Run the installation”.

- Next, fill in the blog details as follows:
Site Title: Cloud is everything
Username: djkone
Password: djkone123
Confirm Password: Check “Confirm use of weak password”
Your Email: use your personal email.
Click the “Install WordPress” button.
You should now be on a screen that says “Success!”

- Click the “Log In” button. Log into the WordPress administration dashboard with your credentials:
Username: djkone
Password: djkone123
Click “Log In”.

Voilà! You have installed WordPress on Ubuntu, while installing NGINX, PHP and MySQL.

This lab was first seen in Linux academy course on Udemy. I added additional steps such as testing each installation.