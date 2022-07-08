# EC2-NodeJS-Nginx-JWT-Project

Project to run nodejs application in AWS EC2 , and use Nginx as a reverse proxy for Node.js server! 

## Lets prepare nodejs project first. make sure you have node and npm installed . open your fav editor and proceed creating project. anytime you are stuck, refer to this repo.

### npm init - y
install few packages required for this project
express- 
dotenv-
jsonwebtoken-
### npm i express jsonwebtoken dotenv
also install nodemon- a Simple monitor script for use during development . this is for development only.
### npm i --save-dev nodemon
create dev tag inside package json scripts 
### "dev": "nodemon server.js"
start node server
### npm run dev
create server.js - copy the code from this repo here.
with nodemon running code should refresh automatically.
Test from browser or any REST client like postman .

### http://localhost:3000/posts

you can stop here and proceed to deploy the app to AWS EC2 instance 

### assuming you have an AWS account, go ahead and spinup an EC2 instance - amazon linux 2 distribution will do for this project. sec. inbound rules ssh and tcp .

once EC2 is up , following the steps below.

2. add inbound rules to the attached security group to open ssh and all tcp (for now). 

3.  connect to EC2 from local using ssh with credential pem file (preferably create a separate user login and avoid using root )
### ssh -i "yourkeyfile.pem" ec2-user@ec2-blabla.amazonaws.com
4. Install apt updates and other nodejs dependencies
### sudo yum update 
###  sudo yum install -y gcc-c++ make
5. Install node and check node and npm versions 
### curl -sL https://rpm.nodesource.com/setup_16.x | sudo -E bash -
### sudo yum install -y nodejs
### 
6. Install git and clone your application code 
### sudo yum install -y git
7. clone the repo
### git clone https://github.com/ALgosZen/EC2-NodeJS-Nginx-JWT-Project.git

8. change directory and install packages
### cd EC2-NodeJS-Nginx-JWT-Project/
### npm i
9. start the node app 
### node server.js

10. And visit the EC2 public DNS url to test the application
### http://ec2-blabbla.us-west-2.amazonaws.com:3000/posts

11. now with application running successfully , lets go ahead and setup nginx. before that we need to install process manager.
12. Install PM2 - process manager 
PM2 is a production process manager for Node.js applications with a built-in load balancer. It allows you to keep applications alive forever, to reload them without downtime and to facilitate common system admin tasks. You can read more about pm2 here.

### npm install pm2 -g

13. now start app using pm2 and save so even on a reboot application comes back up automatically!

### pm2 start server.js
### pm2 save
you will see the following .
pm2 save
[PM2] Saving current process list...
[PM2] Successfully saved in /home/ec2-user/.pm2/dump.pm2
now we have successfully hooked pm2 

14. Lets move to install and setup ngnix server . 
### sudo amazon-linux-extras list | grep nginx
### sudo amazon-linux-extras enable nginx1
### sudo yum clean metadata
### sudo yum -y install nginx
check if nginx is installed successfully.
### nginx -v 
start nginx service
### sudo systemctl start nginx
visit EC2 dns from browser. it should display messsage.
"Welcome to nginx on Amazon Linux!"

15. move inside nginx folder and edit nginx.conf for  reverse proxying
### cd /etc/nginx
### sudo vim nginx.conf
Add the following lines under server tag right below current location directives
### location / {
###                 proxy_set_header  X-Real-IP  ### $remote_addr;
###                 proxy_set_header  Host       $http_host;
###                 proxy_pass        http://127.0.0.1:3000;
###        }

And finally restart the nginx
### sudo systemctl restart nginx

Now you will be able to run the nodejs app , and visit the app on EC2 public DNS without using port number.

At this point we are good to block all ports from direct access. 
NOTE: we can simply use EC2 sec. group inbound rules to take care of all DENY. and block all kinds of access.

We can do the same using ufw. lets install and enable it.


### sudo amazon-linux-extras install epel
### sudo yum install --enablerepo="epel" ufw
### ufw --version
### sudo ufw enable
### sudo ufw status

thats it !!! Exercise to run nodeJS on EC2 Amazon Linux 2 completed .
We have successfully deployed a Node.js application on  AWS EC2 and setup Nginx as reverse proxy. In addition we've also learned to use PM2 for running app without any EC2 restart interruption.

Next we will learn how to .. Configure domain name , configure SSL With Lets Encrypt etc.,

## If you are done here , make sure to terminate all resources in your AWS account !