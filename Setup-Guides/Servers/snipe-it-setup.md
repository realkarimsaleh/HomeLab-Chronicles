# Snipe-IT Setup Guide

**Snipe-IT** is an open-source software for IT asset management that provides transparency, security, and oversight for your IT assets.

Setting it up can be challenging, but I’ll guide you through the installation in a few steps.

---

## 1. Install Ubuntu Server 20.04
You can install this on a physical server or a virtual machine.

Open SSH or Putty, then update and upgrade the packages by running:

```bash
sudo apt update -y && sudo apt upgrade -y
```

## 2. Install LAMP on Ubuntu
We need to turn Ubuntu into a web server. Follow these steps to install LAMP:

### Install Apache2
Install Apache2 using the following command:

```bash
sudo apt install apache2 -y
```

Enable and start the Apache2 service:

```bash
systemctl start apache2 && systemctl enable apache2
```

Check the status of the service:

```bash
systemctl status apache2
```

Validate the installation by checking the Apache version:

```bash
apache2 -v
```

If a firewall is enabled, allow HTTP and HTTPS traffic:

```bash
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw reload
```

Open a browser to verify the web server is ready:

```
http://<serverip>
```

Enable the required extension for Snipe-IT:

```bash
sudo a2enmod rewrite
systemctl restart apache2
```

## 3. Install MySQL / MariaDB
Install the database server and client:

```bash
sudo apt install mariadb-server mariadb-client -y
```

Enable and start the MariaDB service, then check its status:

```bash
sudo systemctl start mariadb && sudo systemctl enable mariadb
sudo systemctl status mariadb
```

Secure the installation:

```bash
sudo mysql_secure_installation
```

## 4. PHP Installation
Install PHP and its extensions. Here, we’ll use PHP 7.4:

```bash
sudo apt install php php-cli php-fpm php-json php-common php-mysql php-zip php-gd php-mbstring php-curl php-xml php-pear php-bcmath -y
```

Ensure all required PHP extensions are installed:

```bash
sudo apt install php php-bcmath php-bz2 php-intl php-gd php-mbstring php-mysql php-zip php-opcache php-pdo php-calendar php-ctype php-exif php-ffi php-fileinfo php-ftp php-iconv php-intl php-json php-mysqli php-phar php-posix php-readline php-shmop php-sockets php-sysvmsg php-sysvsem php-sysvshm php-tokenizer php-curl php-ldap -y
```

## 5. Install PHP Composer
Snipe-IT depends on certain libraries, so we need to install PHP Composer:

```bash
sudo curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer
```

## 6. Create Database in MariaDB for Snipe-IT
Open MySQL using the root user:

```bash
mysql -u root -p
```

Create the database and user, set the password, and assign privileges:

```sql
CREATE DATABASE snipeitdb;
CREATE USER 'snipeituser'@'localhost' IDENTIFIED BY 'Password';
GRANT ALL PRIVILEGES ON snipeitdb.* TO 'snipeituser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

## 7. Install Snipe-IT
Navigate to the website root directory:

```bash
cd /var/www/
```

Clone the Snipe-IT repository:

```bash
git clone https://github.com/snipe/snipe-it snipe-it
```

Navigate to the Snipe-IT folder:

```bash
cd /var/www/snipe-it
```

Copy the `.env` configuration file:

```bash
sudo cp /var/www/snipe-it/.env.example /var/www/snipe-it/.env
```

Edit the `.env` file to update application and database details:

```bash
sudo nano /var/www/snipe-it/.env
```

Set these values:

```env
APP_DEBUG=false
APP_URL=Snipe-IT.example.co.uk
APP_TIMEZONE='Europe/London'
DB_DATABASE=snipeitdb
DB_USERNAME=snipeituser
DB_PASSWORD=Password
```
For *APP_TIMEZONE* should use a [PHP-supported timezone]{https://www.php.net/manual/en/timezones.php}, and should be enclosed in single quotes.

Set permissions for the Snipe-IT directories:

```bash
sudo chown -R www-data:www-data /var/www/snipe-it
sudo chmod -R 755 /var/www/snipe-it
```

Install Composer dependencies:

```bash
sudo composer update --no-plugins --no-scripts
sudo composer install --no-dev --prefer-source --no-plugins --no-scripts
```

Generate the application key:

```bash
php artisan key:generate
```

## 8. Update Virtual Host
Disable the default virtual host:

```bash
sudo a2dissite 000-default.conf
```

Create a virtual host for Snipe-IT:

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

Enable the Snipe-IT virtual host:

```bash
sudo a2ensite snipe-it.conf
```

Update folder permissions:

```bash
sudo chown -R www-data:www-data ./storage
sudo chmod -R 755 ./storage
```

Restart Apache:

```bash
systemctl restart apache2
```

---

Now, open your browser and run the Pre-Flight for Snipe-IT
