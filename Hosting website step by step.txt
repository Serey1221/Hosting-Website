1.Install Apache2
	-sudo apt update
	-sudo apt install apache2
====================================
2.Install PHP
	-sudo apt update
	-sudo apt install php php-cli php-fpm php-json php-common php-mysql php-zip php-gd php-mbstring php-curl php-xml php-pear php-bcmath
	-php --version
====================================
3.Install MySQL
	-sudo apt update
	-sudo apt upgrade
	-sudo apt install mysql-server
	-mysql --version
=====================================
4.Install Github
	-sudo apt update
	-sudo apt install git
	-git --version
5.Install Composer
	-sudo apt update
	-sudo apt install php-cli unzip
	-Install PHP Composer
	-composer
	-composer install --ignore-platform-reqs
======================================
1.Connect github with server.
1.1 Generating a new SSH key.
	-ssh-keygen -t rsa -b 4096 -C "sereysrout93@gmail.com"
	-eval "$(ssh-agent -s)"
1.2 Adding a new SSH key to your GitHub account
	-
2.Connect Database with server.
2.1 Create User in database.
	-CREATE USER 'serey'@'%' IDENTIFIED BY 'Serey@123';
	-GRANT ALL PRIVILEGES ON *.* TO 'serey'@'%' WITH GRANT OPTION;
	-FLUSH PRIVILEGES;
2.2 Create new Database for import file Database.

+Add rule
+You can also use the shortcut Shift + ⌘ + J (on macOS) or Shift + CTRL + J (on Windows/Linux). The Browser console will open in a new window.

=========
CREATE DATABASE sereydb;
GRANT ALL PRIVILEGES ON sereydb.* TO serey@localhost IDENTIFIED BY 'Serey@123';

==============================
ដើម្បី Configure Domain យើងត្រូវ ប្ដូរ  IP យើងទៅជា Elastic IP 



==============================
 ln -s /etc/nginx/sites-available/serey01.prointix.com /etc/nginx/sites-enabled/
chown -R $USER:$USER /var/www/serey01.prointix.com/html
chmod -R 755 /var/www/serey01.prointix.com