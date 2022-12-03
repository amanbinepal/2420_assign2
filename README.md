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




