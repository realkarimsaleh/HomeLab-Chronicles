# Snipe-IT Setup Guide

**Snipe-IT** is an open-source IT asset management tool designed to provide transparency, security, and oversight of IT assets. This guide provides a step-by-step setup process.

---

## Step 1: Install Ubuntu Server 20.04

1. Install Ubuntu Server 20.04 on a physical server or virtual machine.
2. Open an SSH session through Terminal or Putty.
3. Update and upgrade packages:

   ```bash
   sudo apt update -y && sudo apt upgrade -y
   ```

---

## Step 2: Install LAMP on Ubuntu

LAMP (Linux, Apache, MySQL, PHP) is required to set up a web server environment.

### Install Apache2
1. Install Apache2:

   ```bash
   sudo apt install apache2 -y
   ```

2. Enable and start Apache2 service:

   ```bash
   systemctl start apache2 && systemctl enable apache2
   ```

3. Verify service status:

   ```bash
   systemctl status apache2
   ```

4. Allow HTTP and HTTPS traffic if the firewall is enabled:

   ```bash
   sudo ufw allow 80/tcp
   sudo ufw allow 443/tcp
   sudo ufw reload
   ```

5. Verify setup by navigating to `http://<serverip>` in a browser.

6. Enable the required Apache module:

   ```bash
   sudo a2enmod rewrite
   systemctl restart apache2
   ```

---

## Step 3: Install MySQL / MariaDB

1. Install MariaDB:

   ```bash
   sudo apt install mariadb-server mariadb-client -y
   ```

2. Enable and start MariaDB:

   ```bash
   sudo systemctl start mariadb && sudo systemctl enable mariadb
   ```

3. Run `mysql_secure_installation` to secure the installation.

---

## Step 4: Install PHP

1. Install PHP 7.4 and required extensions:

   ```bash
   sudo apt install php php-cli php-fpm php-json php-common php-mysql php-zip php-gd php-mbstring php-curl php-xml php-pear php-bcmath -y
   ```

2. Ensure all required PHP extensions are installed:

   ```bash
   sudo apt install php php-bcmath php-bz2 php-intl php-gd php-mbstring php-mysql php-zip php-opcache php-pdo php-calendar php-ctype php-exif php-ffi php-fileinfo php-ftp php-iconv php-intl php-json php-mysqli php-phar php-posix php-readline php-shmop php-sockets php-sysvmsg php-sysvsem php-sysvshm php-tokenizer php-curl php-ldap -y
   ```

---

## Step 5: Install PHP Composer

1. Install PHP Composer to manage dependencies:

   ```bash
   sudo curl -sS https://getcomposer.org/installer | php
   sudo mv composer.phar /usr/local/bin/composer
   ```

---

## Step 6: Create Database for Snipe-IT

1. Log into MariaDB:

   ```bash
   mysql -u root -p
   ```

2. Create a database and user, set a password, and grant privileges:

   ```sql
   CREATE DATABASE snipeitdb;
   CREATE USER 'snipeituser'@'localhost' IDENTIFIED BY 'Password';
   GRANT ALL PRIVILEGES ON snipeitdb.* TO 'snipeituser'@'localhost';
   FLUSH PRIVILEGES;
   EXIT;
   ```

---

## Step 7: Install Snipe-IT

1. Navigate to the web root:

   ```bash
   cd /var/www/
   ```

2. Clone the Snipe-IT repository:

   ```bash
   git clone https://github.com/snipe/snipe-it snipe-it
   ```

3. Copy the environment configuration file:

   ```bash
   sudo cp /var/www/snipe-it/.env.example /var/www/snipe-it/.env
   ```

4. Update `.env` file with application and database details:

   ```env
   APP_DEBUG=false
   APP_URL=Snipe-IT.example.co.uk
   APP_TIMEZONE='Europe/London'
   DB_DATABASE=snipeitdb
   DB_USERNAME=snipeituser
   DB_PASSWORD=Password
   ```

   Refer to [PHP-supported timezones](https://www.php.net/manual/en/timezones.php) for valid values.

5. Set permissions for Snipe-IT directories:

   ```bash
   sudo chown -R www-data:www-data /var/www/snipe-it
   sudo chmod -R 755 /var/www/snipe-it
   ```

6. Install Composer dependencies:

   ```bash
   sudo composer update --no-plugins --no-scripts
   sudo composer install --no-dev --prefer-source --no-plugins --no-scripts
   ```

7. Generate the application key:

   ```bash
   php artisan key:generate
   ```

---

## Step 8: Update Apache Virtual Host Configuration

1. Disable the default virtual host:

   ```bash
   sudo a2dissite 000-default.conf
   ```

2. Create a virtual host configuration file for Snipe-IT:

   ```bash
   sudo nano /etc/apache2/sites-available/snipe-it.conf
   ```

   Add the following configuration:

   ```apache
   <VirtualHost *:80>
       ServerName Snipe-IT.example.co.uk
       DocumentRoot /var/www/snipe-it/public
       <Directory /var/www/snipe-it/public>
           Options Indexes FollowSymLinks MultiViews
           AllowOverride All
           Order allow,deny
           allow from all
       </Directory>
   </VirtualHost>
   ```

3. Enable the new virtual host:

   ```bash
   sudo a2ensite snipe-it.conf
   ```

4. Set permissions for storage directories:

   ```bash
   sudo chown -R www-data:www-data ./storage
   sudo chmod -R 755 ./storage
   ```

5. Restart Apache:

   ```bash
   systemctl restart apache2
   ```

---

## Final Setup

Open a browser and navigate to your server’s IP or hostname to complete the Snipe-IT pre-flight setup.
