# 2420_assign2

# Step 1 - Initial Setup

## VPC
### Step 1
On Digital Ocean go on Networking
### Step 2
Click on Create VPC Network
### Step 3
Choose SFO3 for the region
### Step 4
Choose a name for the VPC and click Create VPC Network

## Create 2 Droplets
### Step 1
Make sure to create 2 droplets
### Step 2 
Choose appropriate names for the droplets (eg. ubuntu-01, ubuntu-02)
### Step 3
Choose a SSH key. If haven't done so already go and create one using ```ssh key-gen```
### Step 3
Add a tag called Web
### Step 4
Select appropriate project and click Create Droplet

## Load Balancer

### Step 1
Go on Networking
### Step 2
Click Create Load Balancer
### Step 3
Click on the region that your 2 droplets are on
### Step 4
Select the VPC network you made
### Step 5
Under Connect Droplets type in the Web tag
### Step 6
Click Create Load Balancer

## Firewall

### Step 1
Under Networking click Firewalls
### Step 2
Click Create Firewall
### Step 3
Under Inbound Rules, create a new HTTP rule, get rid of default sources and choose your load balancer
### Step 4
Under Apply to Droplets, add the Web tag

# Step 2 - Create Regular User for Both Droplets
## Note: Do these steps in both droplets
### Step 1
SSH into root using ```ssh -i ~/.ssh/(key_name) root@(serverip)```
### Step 2
Add new user with ```useradd -ms /bin/bash (name)```
### Step 3
Add to sudoers with ```usermod --aG sudo (name)```
### Step 4
Use ```rsync --archive --chown=(name):(name) ~/.ssh /home/(name)```
### Step 5
Give password to user with ```passwd (name)```, proceed to enter password
### Step 6
SSH into user using ```ssh -i ~/.ssh/(key_name) (name)@(serverip)```
### Step 7
Once you in, do ```sudo apt update``` then ```sudo apt upgrade```

# Step 3 - Installing Caddy

## Note: Perform these steps on both droplets

### Step 1

Type command ```wget https://github.com/caddyserver/caddy/releases/download/v2.6.2/caddy_2.6.2_linux_amd64.tar.gz``` to download tar.gz file

### Step 2
Type command ```tar xvf caddy_2.6.2_linux_amd64.tar.gz``` to unarchive tar.gz file

### Step 3
Type command ```sudo chown root: caddy``` to change owner and group to root

### Step 4
Type command ```sudo cp caddy /usr/bin/``` to copy to bin directory

# Step 4 - Write Web App
### Step 1
In wsl, create new directory called ```2420-assign-two```
### Step 2
Inside of ```2420-assign-two``` create 2 directories called ```html``` and ```src```
### Step 3
Inside of the ```html``` directory, create a file called ```index.html``` and create a simple html page like hello world in it.
![image](https://user-images.githubusercontent.com/97579029/205425448-e389c848-539e-4eda-9e3d-c4bbd7b016e8.png)

### Step 4
Inside of the ```src``` directory, type command ```npm init``` then ```npm i fastify to create new node project
### Step 5
Create an ```index.js``` in the ```src``` directory and type the following
![image](https://user-images.githubusercontent.com/97579029/205425206-b61ee630-1c34-4f3b-9595-3cd5a4849667.png)
### Step 6
Test server and move 2420-assign-two which contains the html and src directory to servers using sftp
In WSL:
1. ```sftp -i ~/.ssh/(key_name) (name)@(serverip)```
2. ```put -r 2420-assign-two```

# Step 5 - Write Caddyfile
### Step 1
Create Caddyfile in ```/etc/caddy``` directory
### Step 2
Edit Caddyfile with command ```sudo vim /etc/caddy/Caddyfile```
### Step 3
Type the following in the Caddyfile
### Note: Ensure that you use your load balancer's ip
![image](https://user-images.githubusercontent.com/97579029/205425681-fcc943eb-6253-4af3-9047-7a8eb60d6e93.png)

## caddy.service

### Step 4
Type command ```sudo vim /etc/systemd/system/caddy.service``` to edit caddy.service file
### Step 5
Type the following in the caddy.service
![image](https://user-images.githubusercontent.com/97579029/205425918-134a609b-d670-4b85-9467-14e2ce26e313.png)

### Step 6
Type the following commands
#### 1.```systemctl daemon-reload```
#### 2.```systemctl enable caddy.service```
#### 3.```systemctl start caddy.service```
To check status of caddy.service
```systemctl status caddy.service```

# Step 6 - Install node and npm with Volta
Firstly type the command ```curl https://get.volta.sh | bash```
Secondly type ```source ~/.bashrc```
Thirdly type ```volta install node```
Once completed, restart the servers to ensure changes are made

# Step 7 - Write Service File to start node application
### Step 1
Type command ```sudo vim /etc/systemd/system/hello_web.service``` to edit hello_web.service file
### Step 2
Type the following in the hello_web.service
![image](https://user-images.githubusercontent.com/97579029/205426589-5950d452-551c-4dab-b95a-eed911a61a32.png)
### Step 3
Type the following commands
#### 1.```systemctl daemon-reload```
#### 2.```systemctl enable hello_web.server```
#### 3.```systemctl start caddy.service```
To check status of hello_web.service
```systemctl status hello_web.service```
# Step 8

### Step 1
Use sftp to move files to the servers; use the process in the last part of step 4
### Step 3
Move the files to apprpriate locations
1. Service files to ```/etc/systemd/system/```
2. Caddyfiles to ```/etc/caddy/```

# Step 9

Test the load balancer by visiting the following:

http://24.199.71.14/api

















