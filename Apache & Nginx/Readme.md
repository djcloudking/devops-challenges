# Installing NGINX, MySQL, PHP and WordPress on Ubuntu

In this project I will install WordPress on Ubuntu 20.04. To complete that I will first install NGINX, then PHP, MySQL, and finally WordPress.

![image](https://github.com/djcloudking/DevOps-demo/assets/122766532/645b6d2d-1369-488f-aa23-72f025753404)

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

![image](https://github.com/djcloudking/DevOps-demo/assets/122766532/c0605ec2-2e56-4856-94cc-8bb2530a021f)

- Type sudo apt install nginx to instal NGINX.

![image](https://github.com/djcloudking/DevOps-demo/assets/122766532/8c0fb19f-a315-4e4a-a384-78cd47baa918)

- After the installation, typesystemctl status nginxto check the status of NGINX. If it’s active (running), you have installed nginx successfully.

![image](https://github.com/djcloudking/DevOps-demo/assets/122766532/a30d23fb-77e1-4dec-9189-e7f74d497adc)

- Run another test on NGINX to see if it’s running properly. Enter the command curl http://localhost or curl http://127.0.0.1/to check.

![image](https://github.com/djcloudking/DevOps-demo/assets/122766532/c02bd7c8-51d3-4ea7-8a9c-850b04456cb8)


#### Step 2: Install and Configure the MySQL Database Server

- Enter sudo apt install -y mysql-serverto install MySQL.

![image](https://github.com/djcloudking/DevOps-demo/assets/122766532/8df5edd5-f571-402d-9326-8abeccc7a662)

- After installing MySQL, let’s make sure it’s running. Entersystemctl status mysql in the command line.

![image](https://github.com/djcloudking/DevOps-demo/assets/122766532/7f6027a1-de17-4922-85f6-7ffee26c754a)

- Use the mysqladmin command to create a database named “wordpress”. Typesudo mysqladmin create wordpressin the terminal.

- Connect to the MySQL server using the mysql client. 

- Enter sudo mysql.

- Next, use theCREATE USERcommand to create a database user named “wordpress”. Use theGRANT ALL command to allow full permissions to the “wordpress” database.

![image](https://github.com/djcloudking/DevOps-demo/assets/122766532/101d0c49-c45d-47ab-a5be-8ed3c8714ace)

- Before we install PHP, let’s run a test on MySQL. Type mysql -u wordpress -p wordpress (-u is for calling user and -p is for calling password)

![image](https://github.com/djcloudking/DevOps-demo/assets/122766532/243a6f65-549a-467e-88d6-3a2a4f1662b9)

- If you see the screenshot below, then it’s successful.

![image](https://github.com/djcloudking/DevOps-demo/assets/122766532/182111c6-436e-4e08-ac1c-b6183a7b364f)

- If not, you should see the message below.

![image](https://github.com/djcloudking/DevOps-demo/assets/122766532/6d197182-b124-4e2f-bf08-029ba7465957)

- In that case, you either re-type the command mysql -u wordpress -p wordpressor reset the password with SET PASSWORD for wordpress@localhost


#### Step 3: Install PHP

WordPress is written in PHP, so we need to install PHP. We will also install many packages at once this time.  Specifically, we’re going to install PHP FPM.

- Type sudo apt install -y php-fpm php-mysql php-curl php-mbstring php-imagick php-xml php-zip in the terminal to install PHP FPM and different modules.

![image](https://github.com/djcloudking/DevOps-demo/assets/122766532/3664c43f-babe-424e-8749-a49cacf2c0e5)

WordPress requires several PHP modules to function correctly. They are:

+ MySQL for connecting to the MySQL database.
+ cURL for making remote requests.
+ Mbstring to handle multibyte strings.
+ ImageMagick to perform actions such as image resizing.
+ XML to provide XML support.
+ Zip to unzip plugins, themes, and WordPress update packages.

- Finally, type systemctl status php8.1-fpm.servicein the terminal to check the status. PHP is active and running.

![image](https://github.com/djcloudking/DevOps-demo/assets/122766532/6a580d72-b482-4d44-9002-2c64a91a69c0)


#### Step 4: Configure NGINX

We need to tell NGINX to send all PHP requests to PHP FPM for processing. To do this, update the default NGINX configuration. 

- Type the commandcd /etc/nginx/sites-availablein the terminal.

![image](https://github.com/djcloudking/DevOps-demo/assets/122766532/a043476c-119c-47b8-bca3-e93737ed5a93)

- Before making any changes, let’s make a backup. So it’s easy to return to the initial file if there is a mistake. Type sudo cp default.bakin the terminal.

![image](https://github.com/djcloudking/DevOps-demo/assets/122766532/842a0cf3-892c-4fd8-8302-05573bc237f5)

- Use the editor of your choice to make changes to the file. I will use Nano.

First, locate this line “Add index.php to the list if your are using PHP”, change this line from

![image](https://github.com/djcloudking/DevOps-demo/assets/122766532/d2561434-024b-47da-bb1e-2c81d1bd4ea9)

To

![image](https://github.com/djcloudking/DevOps-demo/assets/122766532/c05646b1-c135-409c-b657-288b2171b4aa)

Second, scroll down again, and locate location/{ .Change the line from

![image](https://github.com/djcloudking/DevOps-demo/assets/122766532/9f5d8156-280a-46cb-b4fd-ea6bba545a22)

To

![image](https://github.com/djcloudking/DevOps-demo/assets/122766532/474fe9fe-5ec7-450e-92dd-22fc78f9e5db)

The new line allows you to use pretty permalinks in WordPress.

Still in Nano, change these lines from

![image](https://github.com/djcloudking/DevOps-demo/assets/122766532/0829ba74-3edf-45e6-9aa7-80695f7cf7de)

to

![image](https://github.com/djcloudking/DevOps-demo/assets/122766532/2109e812-1088-4499-8471-2f20c13a81db)

- We made so many changes to the configuration, it’s time now to check for errors. Enter sudo nginx -t , it may ask your password.

![image](https://github.com/djcloudking/DevOps-demo/assets/122766532/9d484e17-85fd-446c-8293-63961fa0cb2f)

- Because we made a configuration change to NGINX, we need to tell nginx to reload its configuration.

![image](https://github.com/djcloudking/DevOps-demo/assets/122766532/8c2f4045-a724-401d-af1c-8057d472ef94)

- Launch a test to check the reload.

![image](https://github.com/djcloudking/DevOps-demo/assets/122766532/563611be-f70f-4d3f-bb53-7defcaa9e457)

- Successful test! Return to the home directory: type these commands cd, then pwd.

![image](https://github.com/djcloudking/DevOps-demo/assets/122766532/1bad2998-78a6-415a-bac3-a13f61998fb5)

#### Step 5: Download WordPress

- Now, download WordPress. You can do that directly from your Linux system by using the curl command. Curl is mostly used to transfer data over a network such as downloading a file. The “-O” option causes the file to be saved locally with the same name that was used on the remote system.

![image](https://github.com/djcloudking/DevOps-demo/assets/122766532/c102a1ed-d9ea-4374-bde5-d74d8d9a702f)

#### Step 6: Extract WordPress and Move It Into the DocumentRoot

- Now that you’ve downloaded WordPress, it’s time to extract its contents and place them into the DocumentRoot. By default, the DocumentRoot is /var/www/html, so that’s where you’ll place the files.

![image](https://github.com/djcloudking/DevOps-demo/assets/122766532/bf0abd2c-f80d-4b76-ad1e-885329c3ab3e)

Then

![image](https://github.com/djcloudking/DevOps-demo/assets/122766532/9db6e069-612e-420f-99ab-e9e6510f4356)


#### Step 7: Assign File Permissions for WordPress

- Assign File Permissions for WordPress. In a later step, you will use the WordPress web installer. You will answer some questions and WordPress will write a configuration file based on those answers. In order to allow it to write to the file, it needs the proper permissions. The web server runs as the “www-data” user, so one simple way to give the web application the proper permissions is to change the ownership of the configuration file to “www-data”. Do that with this command:

![image](https://github.com/djcloudking/DevOps-demo/assets/122766532/e7ad5124-8862-4097-a11b-379af2b7e1dd)

You should see this:

![image](https://github.com/djcloudking/DevOps-demo/assets/122766532/236df80f-f35f-463f-aa23-41c556394fd3)

#### Step 8: Determine Your IP Address

- Type ip a to determine the IP address of your server.

![image](https://github.com/djcloudking/DevOps-demo/assets/122766532/38319533-5736-4d9d-9741-9bab5e1d9a45)

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
