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
## Deploy
I created a droplet on digital ocean to serve this project. The parameter of the server is:  

* 512 MB Memory,
* 20 GB Disk,
* SF01 - Ubuntu 14.04.4X64  

I will list some memos for the deployment process for future reference.  

### Set up Linux Server
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
