# Laravel-WSL-DevEnv-Tutorial
- [Installing WSL in Windows 11](#installing-wsl-in-windows-11)
- [Installing necessary Ubuntu packages](#installing-necessary-ubuntu-packages)
- [Composer and Laravel settings](#composer-and-laravel-settings)
- [Create your dev directory and App DB](#create-your-dev-directory-and-app-db)
  - [Changing your root password (Optional)](#changing-your-root-password-optional)
  - [Creating the DB](#creating-the-db)
  - [Updating Laravel App Config](#updating-laravel-app-config)
  - [Check the DB migrations](#check-the-db-migrations)
- [Begin development](#begin-development)
- [Troubleshooting](#troubleshooting)

# Installing WSL in Windows 11

Open a Windows Terminal

```
wsl --install
```

```
wsl --set-default-version 2
```

```
wsl --install -d Ubuntu-24.04
```

Create your user and password


# Installing necessary Ubuntu packages

I recommend installing the WSL extension in VSCode to quickly connect to your instance's files.

Open your Ubuntu-24.04 LTS app or connect with VSCode. 

Execute the following commands:

```
sudo apt update && sudo apt upgrade
```

```
sudo apt install nodejs npm php php-xml php-mysql mysql-server composer
```

# Composer and Laravel settings

```
composer global require laravel/installer
```

In my instance Laravel Installer didn't work by default, let's change that by adding it to the bashrc configuration.
```
nano ~/.bashrc
```

Save the following line to the end of the file
```
export PATH=~/.config/composer/vendor/bin:$PATH
```

Update the bashrc config

```
source ~/.bashrc
```


# Create your dev directory and App DB

```
mkdir dev-projects
```

```
cd dev-projects
```

I fully recommend opening a VSCode environment using WSL at __~/dev-projects/__ to continue the following steps in this tutorial.

```
laravel new example-app
```

Feel free to choose between the kits:
- __No starter kit__
- Laravel Breeze
- Laravel Jetstream

Testing framework:
- Pest
- __PHPUnit__

Whether initializing a git repository or not.
- Yes
- __No__

Which database will your application use?
- __MySQL__                                                    
- MariaDB                                                  
- SQLite (Missing PDO extension)                           
- PostgreSQL (Missing PDO extension)                       
- SQL Server (Missing PDO extension)

__Install the necessary PDO extensions if you wish to use other technologies__

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

## Changing your root password (Optional)

Check the default root password
```
sudo grep 'temporary password' /var/log/mysqld.log
```
If it doesn't exist then the password is empty. Just press enter when the password prompt appears in your console inside VSCode and you will log in.

```
mysql -u root -p
```

```
ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';
```

## Creating the DB

Once logged in MySQL a prompt like this should appear

__mysql\>__ 

Execute this commands _(Change the database, user and password as you like)_

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

Update the .env file:
- DB_DATABASE=lara_test_db
- DB_USERNAME=laravel
- DB_PASSWORD=laravel

Make sure you are at example-app

```
cd ~/dev-projects/example-app
```

## Updating Laravel App Config

Let's clear the previous configuration and migrate the necessary data

```
php artisan config:clear
```

```
php artisan migrate
```

## Check the DB migrations

```
sudo mysql -u root -p
```

```
USE lara_test_db;
```

```
SHOW TABLES;
```

# Begin development

```
php artisan serve
```

# Troubleshooting

If Ubuntu is not installing properly, run these commands at Windows Terminal:

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
