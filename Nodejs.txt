How to Deploy 
	Node.js/Express.js
 Application on Ubuntu Server
======================================================
1. Connect with Server using SSH
Step 1: Connect with remote Server using SSH
Everything starts by connecting with a remote server. I use PuTTY to connect to the server using ssh.
use the given command
-------------------------------------------
+Syntax
ssh <user>@<ip-address>
# ssh root@123.45.67.890
-------------------------------------------
2. Installation of Node.js
Ubuntu 20.04, By default, contains a version of node.js in the default repository.
We only need to install it. By using APT, for that, refresh your local package index
-------------------------------------------
# sudo apt update

# sudo apt info nodejs
-------------------------------------------
file setup script and save it into a file locally

# curl -sl https://deb.nodesource.com/setup_16.x -o nodesource_setup.sh
# nano nodesource_setup.sh
# bash nodesource_setup.sh
# sudo apt info nodejs

Now install Node.js

# sudo apt install nodejs
-------------------------------------------
Now install npm (Node Package Manager) with APT command

# sudo apt install npm
Checking the version of node.js and npm
# node -v & npm -v
-------------------------------------------
3. Create a new Directory (if required) and initialize a new Node.js/Express.js Project

# mkdir nodeApplication 
# cd nodeApplication
# npm init
-------------------------------------------
Answer the questions and add Express to the project

 # npm install express --save
-------------------------------------------
Edit the file and add the code given below

 # nano app.js 
#To Open/Edit File
#Add Folowing code
const http = require("http");

const server = http.createServer((req, res) => {
  const urlPath = req.url;
  if (urlPath === "/overview") {
    res.end('Welcome to the "overview page" of the nginX project');
  } else if (urlPath === "/api") {
    res.writeHead(200, { "Content-Type": "application/json" });
    res.end(
      JSON.stringify({
        product_id: "xyz12u3",
        product_name: "NginX injector",
      })
    );
  } else {
    res.end("Successfully started a server");
  }
});

server.listen(3000, "localhost", () => {
  console.log("Listening for request");
});

-------------------------------------------
For testing your application, makeapp.js executable :

 # chmod +x ./app.js
-------------------------------------------
Now Run the application.

 # node app.js
 4. Install PM2 & Manage Application with It
PM2 is a process manager for Node.js applications. It provides an easy way to manage and demonize applications (run them in the background as a service).
Run the command given below to install PM2.

 # sudo npm install -g pm2
-------------------------------------------
The -g option tells npm to install the module globally in this way it is available system-wide.
Execute the app.js command in the background by using the following command:

 # pm2 start app.js
 #  pm2 list
 # netstat -plant
-------------------------------------------
5. Setup Nginx as a reverse proxy server
As our application is working fine at localhost now we need to setup Nginx as a reverse proxy server so that it is accessible for the users.

Run the command to edit the following file:

 # sudo nano /etc/nginx/sites-available/default
-------------------------------------------
    location / {
        proxy_pass http: //localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }  
-------------------------------------------
Exit the file after saving it.

To verify there is no syntax error run the command given below:

 # sudo nginx -t
-------------------------------------------
Restart Nginx.

 # sudo systemctl restart nginx

You should now be able to access your application via 
the Nginx reverse proxy by accessing your server’s URL.



