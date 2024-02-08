---
layout: post
title: "🎸 How to Install WordPress on Debian 12"
categories: IT
tags: [debian, wordpress, linux]
author: thu4nvd
---

## Prerequisites
- A server with Debian 12 as OS
- User privileges: root or non-root user with sudo privileges
- Snapshot the VM before the Installation if you are using the VM

## Step 1. Update the System

   Before we start with LAMP installation, we need to update the system packages to the latest versions available.
   ```bash
   sudo apt-get update -y && sudo apt-get upgrade -y
   ```

## Step 2. Install Apache Web Server

   We will start with the Apache web server from the LAMP stack first. To install the Apache Web server execute the following command:
   ```bash
   sudo apt install apache2 -y
   ```
   Once installed, start and enable the service.
   ```bash
   sudo systemctl enable apache2 && sudo systemctl start apache2
   ```
   Check if the service is up and running:
   ```bash
   sudo systemctl status apache2
   ```
   You should receive the following output:
   ```
   root@host:~# sudo systemctl status apache2
   ● apache2.service - The Apache HTTP Server
        Loaded: loaded (/lib/systemd/system/apache2.service; enabled; preset: enabled)
        Active: active (running) since Thu 2023-08-03 06:02:42 CDT; 22h ago
          Docs: https://httpd.apache.org/docs/2.4/
      Main PID: 711 (apache2)
         Tasks: 10 (limit: 4644)
        Memory: 29.7M
           CPU: 4.878s
        CGroup: /system.slice/apache2.service
   ```

## Step 3. Install PHP8.2 with dependencies

   Next, we will install PHP. PHP8.2 is by default enabled in the Debian 12 repository, so to install PHP8.2 with extensions, execute the following commands:
   ```bash
   sudo apt-get install php8.2 php8.2-cli php8.2-common php8.2-imap php8.2-redis php8.2-snmp php8.2-xml php8.2-mysqli php8.2-zip php8.2-mbstring php8.2-curl libapache2-mod-php -y
   ```
   To check the installed PHP version, execute the following command:
   ```bash
   php -v
   ```
   You should get the following output:
   ```
   root@host:~# php -v
   Created directory: /var/lib/snmp/cert_indexes
   PHP 8.2.7 (cli) (built: Jun  9 2023 19:37:27) (NTS)
   Copyright (c) The PHP Group
   Zend Engine v4.2.7, Copyright (c) Zend Technologies
       with Zend OPcache v8.2.7, Copyright (c), by Zend Technologies
   ```

## Step 4. Install the MariaDB database server
   
   The last of the LAMP stack is the MariaDB database server. To install it, execute the command below.
   ```bash
   sudo apt install mariadb-server -y
   ```
   Start and enable the mariadb.service with the following commands:
   ```bash
   sudo systemctl start mariadb && sudo systemctl enable mariadb
   ```
   Check the status of the mariadb.service
   ```bash
   sudo systemctl status mariadb
   ```
   You should receive the following output:
   ```
   root@host:~# sudo systemctl status mariadb
   ● mariadb.service - MariaDB 10.11.3 database server
        Loaded: loaded (/lib/systemd/system/mariadb.service; enabled; preset: enabled)
        Active: active (running) since Fri 2023-08-04 05:04:01 CDT; 26s ago
          Docs: man:mariadbd(8)
                https://mariadb.com/kb/en/library/systemd/
      Main PID: 8511 (mariadbd)
        Status: "Taking your SQL requests now..."
         Tasks: 16 (limit: 4644)
        Memory: 174.3M
           CPU: 907ms
        CGroup: /system.slice/mariadb.service
                └─8511 /usr/sbin/mariadbd
   ```
   Use below command to verify status of mariadb
   ```bash 
   sudo mysqladmin version
   ```
   The output should be as below:
   ```
   mysqladmin  Ver 9.1 Distrib 10.11.4-MariaDB, for debian-linux-gnu on x86_64
   Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.
   
   Server version          10.11.4-MariaDB-1~deb12u1
   Protocol version        10
   Connection              Localhost via UNIX socket
   UNIX socket             /run/mysqld/mysqld.sock
   Uptime:                 5 min 54 sec
   
   Threads: 1  Questions: 58  Slow queries: 0  Opens: 33  Open tables: 26  Queries per second avg: 0.163
   ```
   
## Step 5. Create a WordPress database and user
   Access the database
   ```bash
   sudo mysql
   ```
   Now, create a new user with root privileges and password-based access. Change the username and password to match your preferences:
   Flush the privileges to ensure that they are saved and available in the current session:
   ```
   GRANT ALL ON *.* TO 'admin'@'localhost' IDENTIFIED BY 'P@ssw0rd' WITH GRANT OPTION;
   FLUSH PRIVILEGES;
   exit
   ```
   Test admin user by command: 
   ```
   mysqladmin -u admin -p version
   ```
   Next, we need to create a WordPress database, the WordPress user, and grant the permissions for that user to the database.
   ```sql
   CREATE USER 'wordpress'@'localhost' IDENTIFIED BY 'YourStrongPasswordHere';
   CREATE DATABASE wordpress;
   GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpress'@'localhost';
   FLUSH PRIVILEGES;
   EXIT;
   ```
   
## Step 6. Download and Install WordPress
   
   Before we install WordPress, we first need to download it in the default Apache document root:
   ```bash
   cd /var/www/html
   sudo wget https://wordpress.org/latest.zip
   sudo unzip latest.zip
   sudo rm latest.zip
   ```
   Set the right permissions to files and folders.
   ```
   sudo chown -R www-data:www-data wordpress/
   cd wordpress/
   sudo find . -type d -exec chmod 755 {} \;
   sudo find . -type f -exec chmod 644 {} \;
   ```
   Now, open the wp-config.php file with your favorite editor and enter the database credentials you created in the previous step.
   ```bash
   sudo mv wp-config-sample.php wp-config.php
   sudo nano wp-config.php
   ```
   It should look similar to this:
   ```
   // ** Database settings - You can get this info from your web host ** //
   /** The name of the database for WordPress */
   define( 'DB_NAME', 'wordpress' );
   
   /** Database username */
   define( 'DB_USER', 'wordpress' );
   
   /** Database password */
   define( 'DB_PASSWORD', 'YourStrongPasswordHere' );
   ```
   
## Step 7. Create Apache Virtual Host File

   Go into the Apache directory and create a configuration file for WordPress.
   ```bash
   cd /etc/apache2/sites-available/
   touch wordpress.conf
   ```
   Open the file, paste the following lines of code, save the file and close it.
   ```
   <VirtualHost *:80>
   ServerName alivu.local
   DocumentRoot /var/www/html/wordpress
   
   <Directory /var/www/html/wordpress>
   AllowOverride All
   </Directory>
   
   ErrorLog ${APACHE_LOG_DIR}/error.log
   CustomLog ${APACHE_LOG_DIR}/access.log combined
   
   </VirtualHost>
   ```
   
   Enable the Apache configuration for WordPress and rewrite the module.
   ```bash
   sudo a2enmod rewrite
   sudo a2ensite wordpress.conf
   ```
   Check the syntax:
   ```bash
   sudo apachectl -t
   ```
   You should receive the following output:
   ```
   root@vps:~# apachectl -t
   Syntax OK
   ```
   If the syntax is OK, restartd the Apache service.
   ```bash
   systemctl reload apache2
   ```
   Once the Apache service is restarted, you can finish your WordPress installation at http://alivu.local .  
   You could put the record in the Local DNS or file hosts of the current machine, in order to access the website using the domain name.
   

## Last words
   That was all. You successfully installed and configured WordPress on Debian 12 with the LAMP stack.
   
   
## Reference
   - How to Install WordPress on Debian 12 [Link](https://www.rosehosting.com/blog/how-to-install-wordpress-on-debian-12/)
   - Install Mariadb [Link](https://www.digitalocean.com/community/tutorials/how-to-install-mariadb-on-debian-10)