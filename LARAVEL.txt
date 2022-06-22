	Hosting LARAVEL
=================================================
1.Configuring MySQL
-CREATE DATABASE laravel DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;
-GRANT ALL ON laravel.* TO 'laraveluser'@'localhost' IDENTIFIED BY 'laravel123';
-FLUSH PRIVILEGES;
==============
CREATE DATABASE laravel;
CREATE USER 'laraveluser'@'%' IDENTIFIED BY 'Serey@123';
GRANT ALL PRIVILEGES ON *.* TO 'laraveluser'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
=========================================
	Configuring Nginx
-sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/serey03.prointix.com
-sudo nano /etc/nginx/sites-available/serey03.prointix.com
-sudo ln -s /etc/nginx/sites-available/serey03.prointix.com /etc/nginx/sites-enabled/