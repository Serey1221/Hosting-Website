# How To Deploy a Laravel Application with Nginx on Ubuntu 16.04

## Step 1 — Installing Package Dependencies

Start by updating the package manager cache.

```
sudo apt-get update
```

```
sudo apt-get upgrade
```

Then Install Nginx

```
sudo apt install Nginx
```

## Step 2 — Configuring MySQL

Log into the MySQL root administrative account.

```
mysql -u root -p
```

You will be prompted for the password you set for the MySQL `root` account during installation.

Start by creating a new database called `laravel`, which is what we’ll use for the website. You can choose a different name, but make sure to remember it because you’ll need it later.

```
CREATE DATABASE laravel DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;
```

Next, create a new user that will be allowed to access this database. Here we use `laraveluser` as the username,
but you can customize this too. Remember to replace `password` in the following line with a strong and secure password.

```
GRANT ALL ON laravel.* TO 'laraveluser'@'localhost' IDENTIFIED BY 'password';
```

Flush the privileges to notify the MySQL server of the changes.

```
FLUSH PRIVILEGES;
```

And exit MySQL.

```
EXIT;
```

You have now configured a dedicated database and the user account for Laravel to use. The database components are ready, so next, we’ll set up the demo application.

# Step 3 — Setting Up the Demo Application

First, create a directory within the Nginx web root which will hold the application. Because the demo application is named quickstart, let’s use /var/www/html/quickstart.

```
sudo mkdir -p /var/www/html/quickstart
```

Next, change the ownership of the newly created directory to your user, so that you will be able to work with the files inside without using sudo.

```
sudo chown sammy:sammy /var/www/html/quickstart
```

Move to the new directory and `clone the demo application using Git`.

```
cd /var/www/html/quickstart
git clone https://github.com/laravel/quickstart-basic .
```

Next, we need to install the project dependencies. Laravel utilizes Composer to handle dependency management, which makes it easy to install the necessary packages in one go.

```
composer install
```

## Step 4 — Configuring the Application Environment

Open the Laravel environment configuration file with nano or your favorite text editor.

```
sudo nano /var/www/html/quickstart/.env
```

You’ll need to make the following changes to the file. Make sure to update the placeholder variables, like password and example.com, with the appropriate values.

```
/var/www/html/quickstart/.env
APP_ENV=production
APP_DEBUG=false
APP_KEY=b809vCwvtawRbsG0BmP1tWgnlXQypSKf
APP_URL=http://example.com

DB_HOST=127.0.0.1
DB_DATABASE=laravel
DB_USERNAME=laraveluser
DB_PASSWORD=password

. . .
```

Save the file and exit.
The database configuration section is a little more straightforward:

- `DB_DATABASE` is the name of the database.
- `DB_USERNAME` is the name of the MySQL user that the app should use.
- `DB_PASSWORD` is the database password for that user.
  Next, we have to run database migrations, which will populate the newly created database with necessary tables for the demo application to run properly.

```
php artisan migrate
```

## Step 5 — Configuring Nginx

Let’s change the group ownership of the storage and bootstrap/cache directories to www-data.

```
sudo chgrp -R www-data storage bootstrap/cache
```

Then recursively grant all permissions, including write and execute, to the group.

```
sudo chmod -R ug+rwx storage bootstrap/cache
```

We now have all the demo application files in place with the appropriate permissions. Next, we need to alter the Nginx configuration to make it correctly work with the Laravel installation. First, let’s create a new server block config file for our application by copying over the default file.

```
sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/example.com
```

Open the newly created configuration file.

```
sudo nano /etc/nginx/sites-available/example.com
```

There are few necessary changes you’ll have to make:

- Removing the `default_server` designation from `listen` directives,
- Updating the web root by changing the `root` directive,
- Updating the the `server_name` directive to correctly point to a domain name for the server,
- Updating the request URI handling by changing the `try_files` directive.

The modified Nginx configuration file will look like this:

```
cd /etc/nginx/sites-enabled/example.com
```

```
server {
    listen 80;
    listen [::]:80;

    . . .

    root /var/www/html/quickstart/public;
    index index.php index.html index.htm index.nginx-debian.html;

    server_name example.com www.example.com;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    . . .
}
```

When you’ve made the above changes, you can save and close the file. We have to enable the new configuration file by creating a symbolic link from this file to the sites-enabled directory.

```
sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/
```

And finally reload Nginx to take the changes into account.

```
sudo systemctl reload nginx
```

# Hosting LARAVEL

1.Configuring MySQL

```
-CREATE DATABASE laravel DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;
-GRANT ALL ON laravel.* TO 'laraveluser'@'localhost' IDENTIFIED BY 'laravel123';
-FLUSH PRIVILEGES;
```

```
CREATE DATABASE laravel;
CREATE USER 'laraveluser'@'%' IDENTIFIED BY 'Serey@123';
GRANT ALL PRIVILEGES ON *.* TO 'laraveluser'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

Configuring Nginx

```
sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/serey03.prointix.com
sudo nano /etc/nginx/sites-available/serey03.prointix.com
sudo ln -s /etc/nginx/sites-available/serey03.prointix.com /etc/nginx/sites-enabled/
```

## Video

- [How to deploy laravel project on ubuntu server step by step](https://www.youtube.com/watch?v=N3Suj3ov90o)
- [Laravel 8 Installation Nginx PHP 7.4 MySQL Ubuntu 20.04 LTS](https://www.youtube.com/watch?v=Ne08lkfXUu4)
