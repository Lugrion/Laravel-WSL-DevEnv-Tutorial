# Laravel-WSL-DevEnv-Tutorial

- [Installing WSL in Windows 11](#installing-wsl-in-windows-11)
- [Installing Ubuntu Packages](#installing-ubuntu-packages)
- [Setting up Composer and Laravel](#setting-up-composer-and-laravel)
- [Creating your Development Directory and Database](#creating-your-development-directory-and-database)
  - [Laravel Installer](#laravel-installer)
  - [Changing the MySQL Root Password (Optional)](#changing-the-mysql-root-password-optional)
  - [Creating the Database](#creating-the-database)
  - [Updating Laravel App Configuration](#updating-laravel-app-configuration)
  - [Check Migrations](#check-migrations)
- [Starting the Development Server](#starting-the-development-server)
- [Troubleshooting](#troubleshooting)


# Installing WSL in Windows 11

1) Open Windows Terminal with admin privileges.
2) Run the following commands to install and configure WSL:

```
wsl --install
```

```
wsl --set-default-version 2
```

```
wsl --install -d Ubuntu-24.04
```

3. Follow the on-screen instructions to set up your Ubuntu user and password.


# Installing Ubuntu Packages

1) Install the WSL extension for VSCode for easier file navigation and access to your instance.

2) Open your Ubuntu-24.04 LTS terminal (or connect through VSCode).

3) Update and install necessary packages:


```
sudo apt update && sudo apt upgrade
```

4) Install the required tools:

```
sudo apt install nodejs npm php php-xml php-mysql mysql-server composer
```
>__Tip: You can install additional PHP extensions as needed, e.g., php-pdo, php-pgsql, etc., for other database technologies.__

# Setting up Composer and Laravel

1) Install the Laravel installer globally via Composer:

```
composer global require laravel/installer
```

2) If the Laravel command doesn’t work globally, add Composer’s vendor/bin to your PATH:

```
nano ~/.bashrc
```

3) Append the following line at the end of the file:

```
export PATH=~/.config/composer/vendor/bin:$PATH
```

4) Reload your bash configuration:

```
source ~/.bashrc
```


# Creating your Development Directory and Database

1) Create a directory for your projects:

```
mkdir dev-projects
cd dev-projects
```
>__Tip: Open this directory in VSCode using WSL for a seamless development experience.__

2) Create a Laravel project:

```
laravel new example-app
cd example-app
```

## Laravel Installer
1) Feel free to choose between the kits:
- __No starter kit__
- Laravel Breeze
- Laravel Jetstream

2) Testing framework:
- Pest
- __PHPUnit__

3) Whether initializing a git repository or not.
- Yes
- __No__

4) Which database will your application use?
- __MySQL__    
- MariaDB                                                  
- SQLite (Missing PDO extension)                           
- PostgreSQL (Missing PDO extension)                       
- SQL Server (Missing PDO extension)

>__Tip: Install the necessary PDO extensions if you wish to use other technologies__

Whether or not to run the default database migrations:
- Yes
- __No__

If we were to run the migrations It would cause an error as no databases are ready.

If you run:

```
cd example-app
```

```
php artisan serve
```

You'll see there's a few warnings when hosting our app. Let's fix them by creating a new database for the new laravel app using MySQL.

## Changing the MySQL Root Password (Optional)

1) Check the default root password
```
sudo grep 'temporary password' /var/log/mysqld.log
```
2) If none exist then the password is empty. Press enter when prompted to log in.

```
mysql -u root -p
```

```
ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';
```

## Creating the Database

Once logged in MySQL a prompt like this should appear

__mysql\>__ 

1) Execute these commands _(Change the database, user and password as you like)_

```
CREATE DATABASE lara_test_db;
```

```
CREATE USER 'laravel'@'localhost' IDENTIFIED BY 'laravel';
```

```
GRANT ALL PRIVILEGES ON lara_test_db.* TO 'laravel'@'localhost';
```

```
FLUSH PRIVILEGES;
```

```
Exit;
```

2) Update the .env file:
- DB_DATABASE=lara_test_db
- DB_USERNAME=laravel
- DB_PASSWORD=laravel

## Updating Laravel App Configuration

1) Clear any cached configuration to apply the new database settings:

```
php artisan config:clear
```

2) Run migrations to set up your database:

```
php artisan migrate
```

## Check Migrations

```
sudo mysql -u root -p
```

```
USE lara_test_db;
```

```
SHOW TABLES;
```

# Starting the Development Server

```
php artisan serve
```

You should now see your Laravel application running on `http://127.0.0.1:8000/`



# Troubleshooting

1) If Ubuntu is not installing properly, run these commands at Windows Terminal:

```
DISM /Online /Cleanup-Image /ScanHealth 
```
```
DISM /Online /Cleanup-Image /CheckHealth 
```
```
DISM /Online /Cleanup-Image /RestoreHealth 
```
```
Source:G:\Sources\install.wim 
```
```
SFC /Scannow
```

2) Ensure your WSL instance is healthy and not corrupted. Use Windows recovery files if needed.
