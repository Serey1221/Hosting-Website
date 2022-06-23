
#	How To Set Up Nginx Server Blocks (Virtual Hosts) on Ubuntu 16.04

	
## Step 1 — Setting Up New Document Root Directories

We need to create these directories for each of our sites. The -p flag tells mkdir to create 
any necessary parent directories along the way:

```
	sudo mkdir -p /var/www/serey.prointix.com/html
	sudo mkdir -p /var/www/serey01.prointix.com/html
	sudo mkdir -p /var/www/theara.proinitx.com/html
	sudo mkdir -p /var/www/serey02.prointix.com/html

```

The permissions of our web roots should be correct already if you have not 
	modified your umask value, but we can make sure by typing:
```
	sudo chmod -R 755 /var/www
```

## Step 2 — Creating Sample Pages for Each Site

Create an index.html file in your first domain:
```
	nano /var/www/serey.prointix.com/html/index.html
	nano /var/www/serey01.prointix.com/html/index.html
	nano /var/www/serey02.prointix.com/html/index.html
	nano /var/www/theara.prointix.com/html
```

Inside the file, we’ll create a really basic file that indicates what site we are 
currently accessing. It will look like this:
```
	<html>
    	<head>
       	 <title>Welcome to serey.prointix.com</title>
  	  </head>
    	<body>
       	 <h1>Success! The serey.prointix.comserver block is working!</h1>
    	</body>
	</html>
```
	<html>
    	<head>
       	 <title>Welcome to theara.prointix.com</title>
  	  </head>
    	<body>
       	 <h1>Success! The theara.prointix.comserver block is working!</h1>
    	</body>
	</html>

Since the file for our second site is basically going to be the same, 
we can copy it over to our second document root like this:
```
	cp /var/www/serey.prointix.com/html/index.html /var/www/serey01.prointix.com/html/
```
Now, we can open the new file in our editor:
```
	nano /var/www/serey01.prointix.com/html/index.html
```
Modify it so that it refers to our second domain:
```
	<html>
    	<head>
       	 <title>Welcome to serey01.prointix.com</title>
  	  </head>
    	<body>
       	 <h1>Success! The serey01.prointix.comserver block is working!</h1>
    	</body>
	</html>
```
## Step 3 — Creating Server Block Files for Each Domain

# Creating the First Server Block File

As mentioned above, we will create our first server block config file by copying over the default file:
```
	sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/serey.prointix.com
	sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/theara.prointix.com
```
Now, open the new file you created in your text editor with sudo privileges:
```
	sudo nano /etc/nginx/sites-available/serey.prointix.com
```
```
server {
        listen 80;
        listen [::]:80;

        root /var/www/serey.prointix.com/html;
        index index.html index.htm index.php index.nginx-debian.html;

        server_name serey.prointix.com www.serey.prointix.com;

        location / {
                try_files $uri $uri/ =404;
        }
}
```

Creating the Second Server Block File

Now that we have our initial server block configuration, we can use that as a 
basis for our second file. Copy it over to create a new file:
```
	sudo cp /etc/nginx/sites-available/serey.prointix.com /etc/nginx/sites-available/serey01.prointix.com
```
Open the new file with sudo privileges in your editor:
```
sudo nano /etc/nginx/sites-available/serey01.prointix.com
```
```
server {
        listen 80;
        listen [::]:80;

        root /var/www/serey01.prointix.com/html;
        index index.html index.htm index.php index.nginx-debian.html;

        server_name serey01.prointix.com www.serey01.prointix.com;
        server_name serey02.prointix.com www.serey02.prointix.com;

        location / {
                try_files $uri $uri/ =404;
        }
}
```
## Step 4 — Enabling your Server Blocks and Restart Nginx

Now that we have our server block files, we need to enable them. We can do this by creating 
symbolic links from these files to the sites-enabled directory, which Nginx reads from during startup.

We can create these links by typing:
```
	sudo ln -s /etc/nginx/sites-available/serey.prointix.com /etc/nginx/sites-enabled/
	sudo ln -s /etc/nginx/sites-available/serey01.prointix.com /etc/nginx/sites-enabled/
	sudo ln -s /etc/nginx/sites-available/serey02.prointix.com /etc/nginx/sites-enabled/
	sudo ln -s /etc/nginx/sites-available/theara.prointix.com /etc/nginx/sites-enabled/
```
In order to avoid a possible hash bucket memory problem that can arise from adding additional 
server names, we will also adjust a single value within our /etc/nginx/nginx.conf file. Open the file now:
```
	sudo nano /etc/nginx/nginx.conf
```
Within the file, find the server_names_hash_bucket_size directive. Remove the # symbol to uncomment the line:
```
http {
    . . .

    server_names_hash_bucket_size 64;

    . . .
}
```
Save and close the file when you are finished.

Next, test to make sure that there are no syntax errors in any of your Nginx files:
```
	sudo nginx -t
```

If no problems were found, restart Nginx to enable your changes:
```
	sudo systemctl restart nginx
```
## Step 5 — Modifying Your Local Hosts File for Testing (Optional)

## Step 6 — Testing Your Results
```
	http://serey.prointix.com
	http://serey01.prointix.com
	http://theara.prointix.com
```
## Link

[How To Set Up Nginx Server Blocks (Virtual Hosts) on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-set-up-nginx-server-blocks-virtual-hosts-on-ubuntu-16-04)

```
composer install --ignore-platform-reqs

composer --ignore-platform-req=php create-project laravel/laravel "5.0.*" --prefer-dist
```
# Add SSL🧠🧠🧠🧠🧠🧠🧠
```
certbot --nginx -d serey.prointix.com
certbot --nginx -d serey01.prointix.com
certbot --nginx -d serey02.prointix.com
```