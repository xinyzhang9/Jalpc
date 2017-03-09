---
layout: post
title:  "Friend Circles(IV)"
date:   2016-08-09
desc: "Friend Circles part 4"
keywords: "fullstack, deployment, cloud"
categories: [Javascript,Backend]
tags: [FullStack,Deployment,Cloud]
icon: icon-javascript
---
# Deployment
I created a droplet on digital ocean to serve this project. The parameter of the server is:  

* 512 MB Memory,
* 20 GB Disk,
* SF01 - Ubuntu 14.04.4X64  

I will list some memos for the deployment process for future reference.  

## Set up Linux Server
* ssh root@{ip} In the ubuntu terminal: These establish some basic dependencies for deployment and the Linux server.  
* update packages  
```
sudo apt-get update
sudo apt-get install -y build-essential openssl libssl-dev pkg-config
```
* In the ubuntu terminal, one at a time because they require confirmation: (these install basic node and npm)  
```
sudo apt-get install node
sudo apt-get install npm
sudo npm cache clean -f
```
* In the ubunt terminal: These install the node package manager n and updated node.  
```
sudo npm install -g n
sudo n stable (or whichever node version you want e.g. 5.9.0)
```
* node -v should give you the stable version of node, or the version that you just installed.  
* Install NGINX and git:  
```
sudo apt-get install nginx
sudo apt-get install git
```
* Make file folder:
```
sudo mkdir /var/www
```
* Enter the folder:
```
cd /var/www
```
* Clone the project: 
```
sudo git clone {{your project file path on github/bitbucket}}
```

## Set up Nginx

* Go to nginx’s sites-available directory:
```
cd /etc/nginx/sites-available
```
* Enter vim: 
```
sudo vim {{pname}}
```
* Paste and modify the following code into vim after hitting i:
```
server {
    listen 80;
    location / {
        proxy_pass http://{{PRIVATE-IP}}:8000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```
This code says: have the reverse proxy server (nginx) listen at port 80. When going to root /, listen for http requests as though you were actually http:// your private ip and the port your server is listening e.g @8000 or @6789 etc.  

* Remove the defaults from /etc/nginx/sites-available  
```
cd /etc/nginx/sites-available  
sudo rm default
```
* Create a symbolic link from sites-enabled to sites available
```
sudo ln -s /etc/nginx/sites-available/{{pname}} /etc/nginx/sites-enabled/{{pname}}
```
* Remove the defaults from /etc/nginx/sites-enabled/
```
cd /etc/nginx/sites-enabled/ 
sudo rm default
```

## Project Dependencies and PM2
* Install pm2 globally [pm2.5](https://www.npmjs.com/package/pm2.5) or [pm2](https://www.npmjs.com/package/pm2). This is a production process manager that allows us to run node processes in the background.
```
sudo npm install pm2 -g
```
* Try some stuff with pm2!
```
pm2 start server.js
pm2 stop 0
pm2 restart 0
sudo service nginx reload && sudo service nginx restart
```
* You might have some components that you still need to install: (get your dependencies from npm (assuming your git project has a package.json))
```
npm install
```
* IF USING BOWER (assuming you have a bower.json)
```
sudo npm install bower -g
sudo bower install --allow-root
```
## MongoDB
* Set up a key
```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
```
* setup mongodb in a source list
```
echo "deb http://repo.mongodb.org/apt/ubuntu precise/mongodb-org/3.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.0.list
```
* re-update to integrate Mongod
```
sudo apt-get update
```
* install mongo
```
sudo apt-get install -y mongodb-org
```
* Start mongo (probably already started)
```
sudo service mongod start
```
* Restart your pm2 project and make sure the nginx config’s are working:  
```
pm2 stop 0
pm2 restart 0
sudo service nginx reload && sudo service nginx restart
```

At this point, all the works are done. The project is on live.  

## References
Notes from AWS Deployment, CodingDojo
