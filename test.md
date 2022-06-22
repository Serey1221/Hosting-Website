# Deploy Yii2 with nginx

Update server

```sh
sudo apt update
```

Install ngnix

```sh
sudo apt install nginx
```

Check go to your server IP (http://your_server_ip)

And then to nginx configure

```sh
cd /etc/nginx/sites-available
```

Before we can create vhost I want you to install php first. I'm using php 7.4 but Ubuntu pacakge manager don't have the php 7.4 anymore, So we need to add PPA for supported PHP versions with many PECL extensions.
See [here](https://launchpad.net/~ondrej/+archive/ubuntu/php/)

Run the following command

```sh
sudo add-apt-repository ppa:ondrej/php
sudo apt update
```

Now we can install php 7.4
The nignx working with php using FastCGI so we need to install php fpm (FastCGI Process Manager) see https://www.php.net/install.fpm. database driver, imagemagick for capthca
you can add more.

```sh
sudo apt install php7.4 php7.4-fpm php7.4-gd php7.4-mysql
```

And create configure file with your domain name. Example

```sh
sudo vim nham-bot.prointix.com.conf
```

And then past this config. See [here](https://www.yiiframework.com/doc/guide/2.0/en/start-installation#recommended-nginx-configuration)

```nginx
server {
    charset utf-8;
    client_max_body_size 128M;

    listen 80; ## listen for ipv4
    #listen [::]:80 default_server ipv6only=on; ## listen for ipv6

    server_name nham-bot.prointix.com;
    root        /var/www/nham-bot.prointix.com/web;
    index       index.php;
    server_tokens off;

    access_log  /var/log/ngnix/nham-bot.prointix.com/access.log;
    error_log   /var/log/nginx/nham-bot.prointix.com/error.log;

    location / {
        # Redirect everything that isn't a real file to index.php
        try_files $uri $uri/ /index.php$is_args$args;
    }

    # uncomment to avoid processing of calls to non-existing static files by Yii
    location ~ \.(js|css|png|jpg|gif|swf|ico|pdf|mov|fla|zip|rar)$ {
        try_files $uri =404;
    }
    #error_page 404 /404.html;

    # deny accessing php files for the /assets directory
    location ~ ^/assets/.*\.php$ {
        deny all;
    }

    location ~ \.php$ {
        include fastcgi_params;
        # uncomment this if you using HTTPS
        #fastcgi_param HTTPS on;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        #fastcgi_pass 127.0.0.1:9000;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock; # if you are using another version feel free to change it by your own.
        try_files $uri =404;
    }

    location ~* /\. {
        deny all;
    }
}
```

And then create project root directory base on config above.

```sh
# this is project directory and Yii2 basic source code
sudo mkdir /var/www/nham-bot.prointix.com
```

And then create website log

```sh
sudo mkdir /var/log/ngnix/nham-bot.prointix.com
sudo touch /var/log/ngnix/nham-bot.prointix.com/access.log
sudo touch /var/log/ngnix/nham-bot.prointix.com/error.log
```

And the create syslink by

```sh
sudo ln -s /ete/ngnix/sites-available/nham-bot.prointix.com.conf /ete/nginx/sites-enabled
```

Testing nginx configuration

```sh
sudo nginx -t
```

result:

```
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

Now we need to reload/restart nginx server

```sh
sudo systemctl reload nginx
```

Now you can go to your website to it work. Example http://nham-bot.prointix.com

Our Yii2 basic need to store data. So we are using MySQL.
Let's install MySQL on our server. Your can see detail
[here](https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-20-04 "digitalocean").

```sh
sudo apt install mysql-server
```

Ensure that the server is running using the systemctl start command:

```sh
sudo systemctl start mysql.service
```

you’ll want to run the DBMS’s included security script.
Run the security script with `sudo`:

```sh
sudo mysql_secure_installation
```

Login to mysql to create new user. We can't use root to connect to our database;

```sh
sudo mysql
```

Now we can create user

```sh
CREATE USER 'yii2'@'localhost' IDENTIFIED WITH mysql_native_password BY 'ge*@7a^WQSAn#f#P';
```

We grant access user to all permissions

```sh
GRANT ALL PRIVILEGES ON *.* TO 'yii2'@'localhost' WITH GRANT OPTION;

FLUSH PRIVILEGES;
```

Exit the root user and login with user we create above

```sh
exit;
```

```sh
mysql -u yii2 -p
```

Now we create database to use in our application.
In mysql console

```sh
CREATE DATABASE yii2basic
```

Now to our application config `/var/www/nham-bot.prointix.com/config`

```sh
sudo vim db.php
```

and the edit configuration to match we create above and save it.
Go to our website Example: http://nham-bot.prointix.com
if you website can upload file only 2M you can changing `upload_max_filesize = 2M` in `/etc/php/7.4/fpm/php.ini`. if you are use other version feel free to change the version you use.

Our website don't have SSL yet. So we use Let's Encrypt. See [here](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-20-04)

Note: if you using AWS you need to enable port 443 on Security Groups and skip `Step 3` on url above

Now install the packages

```sh
sudo apt install certbot python3-certbot-nginx
```

Obtaining an SSL Certificate

```sh
sudo certbot --nginx -d nham-bot.prointix.com
```

Verifying Certbot Auto-Renewal

```sh
sudo systemctl status certbot.timer
```

Now our website is have SSL.

Congrats 👏
