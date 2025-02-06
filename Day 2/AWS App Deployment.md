- [AWS 2-Tier Architecture Deployment Guide](#aws-2-tier-architecture-deployment-guide)
  - [Key Pair Setup](#key-pair-setup)
  - [EC2 Instance Creation](#ec2-instance-creation)
    - [Step 1: Open the EC2 Dashboard](#step-1-open-the-ec2-dashboard)
    - [Step 2: Configure the Instances](#step-2-configure-the-instances)
    - [Step 3: Launch the Instances](#step-3-launch-the-instances)
  - [Application Setup](#application-setup)
    - [SSH into the App VM](#ssh-into-the-app-vm)
    - [Install Node.js and npm](#install-nodejs-and-npm)
    - [Clone the Repository](#clone-the-repository)
    - [Start the Application](#start-the-application)
  - [Database Setup](#database-setup)
    - [Install MongoDB](#install-mongodb)
    - [Start the MongoDB Service](#start-the-mongodb-service)
    - [Stop MongoDB (if needed)](#stop-mongodb-if-needed)
  - [Reverse Proxy Setup (NGINX)](#reverse-proxy-setup-nginx)
    - [Install NGINX](#install-nginx)
    - [Check NGINX Status](#check-nginx-status)
    - [Configure NGINX for Reverse Proxy](#configure-nginx-for-reverse-proxy)
      - [Add the following configuration:](#add-the-following-configuration)
    - [Enable the Configuration](#enable-the-configuration)
    - [Update AWS Security Group](#update-aws-security-group)


# AWS 2-Tier Architecture Deployment Guide

## Key Pair Setup

1. Navigate to the **AWS Console** → **EC2 Dashboard**.
2. Click **Key Pairs** under **Network & Security**.
3. Click **Create Key Pair**.
4. Enter a **Key Pair Name** (e.g., `tech501-umar-aws-key`).
5. Select **Key Type** → Choose **RSA**.
6. Select **Private Key Format** → Choose **.pem**.
7. Click **Create Key Pair**.
8. Download the `.pem` file and store it securely.

## EC2 Instance Creation

### Step 1: Open the EC2 Dashboard
1. Go to **AWS Console** → **EC2 Dashboard**.
2. Click **Launch Instances**.

### Step 2: Configure the Instances
- **Instance Names:**
  - **App VM:** `tech501-umar-sparta-app-vm`
  - **DB VM:** `tech501-umar-sparta-db-vm`
- **Amazon Machine Image (AMI):**
- **Instance Type:**
  - Choose **t2.micro** (Free-tier eligible) or **t3.micro** (for better performance).
- **Key Pair:**
  - Select `tech501-umar-aws-key` (created earlier).
- **Network Settings:**
  - Select **default VPC** 
- **Security Group Settings:**
  - **App VM:** Allow **SSH (22), HTTP (80), HTTPS (443)**.
  - **DB VM:** Allow **SSH (22), MongoDB (27017)** (accessible only from the App VM).
- **Storage:**
  - Keep the default **8GB** (increase if needed).

### Step 3: Launch the Instances
1. Click **Launch Instance**.
2. Wait for the instances to reach the **Running** state.
3. Copy the **Private IP** of the **DB VM** (required for app connection).

## Application Setup

### SSH into the App VM
```bash
ssh -i tech501-umar-aws-key.pem ubuntu@YOUR_APP_VM_PUBLIC_IP
```
### Install Node.js and npm
```bash
sudo yum update -y
sudo yum install -y nodejs npm
```
### Clone the Repository
```bash
ssh-keygen -t rsa -b 4096 -C "your-email@example.com"
mkdir repo
cd repo
git clone git@github.com:umarkhvn/tech501-sparta-app.git
```
### Start the Application
```bash
cd tech501-sparta-app/nodejs20-sparta-test-app/app
node app.js
```

## Database Setup

### Install MongoDB
```bash
sudo apt-get install gnupg curl
curl -fsSL https://www.mongodb.org/static/pgp/server-8.0.asc | \
    sudo gpg -o /usr/share/keyrings/mongodb-server-8.0.gpg --dearmor

echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-8.0.gpg ] https://repo.mongodb.org/apt/ubuntu noble/mongodb-org/8.0 multiverse" | \
    sudo tee /etc/apt/sources.list.d/mongodb-org-8.0.list

sudo apt-get update
sudo apt-get install -y mongodb-org=8.0.4 mongodb-org-database=8.0.4 \
    mongodb-org-server=8.0.4 mongodb-mongosh \
    mongodb-org-mongos=8.0.4 mongodb-org-tools=8.0.4
```
### Start the MongoDB Service
```bash
sudo systemctl start mongod
sudo systemctl status mongod
sudo systemctl enable mongod
```
### Stop MongoDB (if needed)
```bash
sudo systemctl stop mongod

### Configure DB_HOST
```bash
export DB_HOST=mongodb://[privateIP]:27017/posts
printenv DB_HOST
* should print mongodb://[privateIP]:27017/posts
node app.js
```

## Reverse Proxy Setup (NGINX)

### Install NGINX
```bash
sudo yum update
sudo yum install -y nginx
```
### Check NGINX Status
```bash
sudo systemctl status nginx
```
### Configure NGINX for Reverse Proxy
```bash
sudo sudo nano /etc/mongod.conf
```
#### Add the following configuration:
```nginx
server {
 
    listen 80;
 
    server_name _;
 
    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```
### Enable the Configuration
```bash
sudo nginx -t  # Check configuration for errors
sudo systemctl restart nginx  # Restart NGINX to apply changes
```
### Update AWS Security Group
- Add an **Inbound Rule** to allow **port 80 (HTTP)** access.
- Test the setup by visiting:
  ```
  http://YOUR_APP_VM_PUBLIC_IP/posts
  ```
