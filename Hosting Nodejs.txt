How to Deploy 
	Node.js/Express.js
 Application on Ubuntu Server
==================================
|<*+Note+*>|	Install Node JS
sudo apt update
sudo apt info nodejs
sudo apt install nodejs
======================================================
|<*+Note+*>| Now install npm (Node Package Manager) with APT command

sudo apt install npm
======================================================
Create a new Node.js/Express.js Application
Create a new Directory (if required) and initialize a new Node.js/Express.js Project

mkdir nodeApplication 
cd nodeApplication
npm init
======================================================
Nginx Configuration address: /etc/nginx/
Nginx Main Server Root address: /var/www/
=========================================================
sudo mkdir -p /var/www/serey02.prointix.com/html
=========================================================
sudo chmod -R 755 /var/www/serey02.prointix.com
=========================================================
sudo chown -R $USER:$USER /var/www/example.com/html
=========================================================
nano /var/www/servername.com/html/index.html
=========================================================
<html> 

     <head> 
        <title>Welcome to servername.com!</title> 
    </head> 
 
    <body> 
        <h1> 

Success! The servername.com server block is working successfully! 

        </h1> 
        
 <b>Meow Meow!</b> 
 
    </body> 

 </html>
=========================================================
sudo nano /etc/nginx/sites-available/servername.com
=========================================================
server { 
        listen 80; 
        listen [::]:80; 
 
        root /var/www/servername.com/html; 
        index index.html index.htm index.nginx-debian.html; 
 
        #Here, you can put your domain name for ex: www.servername.com 
 
        server_name 42.35.40.01; 
 
        location / { 
                try_files $uri $uri/ =404; 
        } 
} 
=========================================================
sudo ln -s /etc/nginx/sites-available/servername.com /etc/nginx/sites-enabled/
=========================================================
nginx -t

sudo systemctl restart nginx
=========================================================
	Install Node
sudo apt install wget

wget -qO-https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
=========================================================
source ~/.bashrc
source ~/.profile
