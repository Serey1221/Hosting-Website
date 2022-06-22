Hosting
-Domain and website
-file manager and FTP for uploade source code
-Database manager for source database



Hosting Server on Linux Ubuntu
-install Apache for PHP
	+command for install apache:
	==> sudo apt install apache2 <==

	==> sudo systemctl enable --now apache2 <==
	
	==> sudo ufw status <==

	==> sudo ufw allow 'Apache' <==
#####################################

local Apache var/www/html
#####################################

@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
-install Mysql for Database

I/Step1:Update/Upgrade Package Repository

  1.Open the ubuntu and run the following command:
	==> sudo apt update <==

  2.Enter your Passworld and wait for the update finish.
  3.Next. Run 
	==> sudo apt upgrade

  4. Enter Y when prompted to continue with the upgrade and hit ENTER. Wait for the upgrade to finish.

II/Step2 Install MySQL
  1. After successfully updating the package repository, install MySQL Server by running the following command:
	==> sudo apt install mysql-server

  2. When asked if you want to continue with the installation, answer Y and hit ENTER.
	The system downloads MySQL packages and installs them on your machine.
	
	Note: If you only want to connect to a remote MySQL server instead of 
	hosting a database on your machine, install only the MySQL Client by running:
	==> sudo apt install mysql-client <==

  3. Check if MySQL was successfully installed by running:
	==> mysql --version
	The output shows which version of MySQL is installed on the machine.
	
	Note: If you are using Windows, learn how to install and configure MySQL on a Windows Server.

Step 3: Securing MySQL
The MySQL instance on your machine is insecure immediately after installation.

1. Secure your MySQL user account with password authentication by running the included security script:
	==> sudo mysql_secure_installation <==

2. Enter your password and answer Y when asked if you want to continue setting up the VALIDATE PASSWORD component. The component checks to see if the new password is strong enough.

3. Choose one of the three levels of password validation:

0 - Low. A password containing at least 8 characters.
1 - Medium. A password containing at least 8 characters, including numeric, mixed case characters, and special characters.
2 - Strong. A password containing at least 8 characters, including numeric, mixed case characters, and special characters, and compares the password to a dictionary file.
Enter 0, 1, or 2 depending on the password strength you want to set. The script then instructs you to enter your password and re-enter it afterward to confirm.

Any subsequent MySQL user passwords need to match your selected password strength.

	Note: Even though you are setting a password for the root user, this user does not require password authentication when logging in.
The program estimates the strength of your password and requires confirmation to continue.

4. Press Y if you are happy with the password or any other key if you want a different one.

5. The script then prompts for the following security features:

Remove anonymous users?
Disallow root login remotely?
Remove test database and access to it?
Reload privilege tables now?
The recommended answer to all these questions is Y. However, if you want a different setting for any reason, enter any other key.

For example, if you need the remote login option for your server, enter any key other than Y for that prompt.
	Note: Check out our tutorial if you want to create a new MySQL user and grant privileges.

tep 4: Check if MySQL Service Is Running
Upon successfully installing MySQL, the MySQL service starts automatically.

Verify that the MySQL server is running by running:
==> sudo systemctl status mysql <==
The output should show that the service is operational and running:

Step 5: Log in to MySQL Server
Finally, to log in to the MySQL interface, run the following command:

	==> sudo mysql -u root <==

Now you can execute queries, create databases, and test out your new MySQL setup. Take a look at our MySQL Commands Cheat Sheet for some important MySQL commands to know.

	Note: You my find helpful our article on how to resolve an 
	error 'Can't connect to local MySQL server through socket '/var/run/mysqld/mysqld.sock (2)' 
	that can appear when logging into the MySQL interface.

*****************
------------------------\---------------------\---------------------------
composer install --ignore-platform-reqs
=======================================
Server Script
Client Script
****************************
1.localHosting


2.Hosting Provider


3.VPS 

