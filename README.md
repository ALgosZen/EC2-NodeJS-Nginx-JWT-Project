# EC2-NodeJS-Nginx-JWT-Project
### Lets prepare nodejs project first. make sure you have node and npm installed . open your fav editor and proceed creating project. anytime having trouble refer to this repo.

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
create server.js file as shown in repo here
test from browser or any REST client like postman
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

