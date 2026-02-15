# Travel Memory Application Deployment

## Overview

This guide explains how to deploy the Travel Memory MERN-stack application using AWS services.

The deployment covers:

- Hosting the application on an EC2 instance
- Connecting with MongoDB Compass
- Deploying both Frontend and Backend on EC2
- Creating multiple instances using AWS Launch Templates
- Using Nginx to serve the app on port 80
- Attaching an AWS Load Balancer
- Connecting a domain using Cloudflare

---

## Architecture Flow

User → Cloudflare Domain → AWS Load Balancer → EC2 Instances → Nginx → Node.js App → MongoDB

---

# Setting Up the Application on AWS EC2

## Step 1: Launch EC2 Instance for Backend

- Launch an Ubuntu EC2 instance.
- Connect via SSH.

---

## Step 2: Install Required Packages (Backend Setup)

```bash
sudo apt update -y
sudo apt install nodejs -y
sudo apt install npm -y

nodejs -v
npm -v
Step 3: Deploy Backend Code
sudo bash
cd ~
git clone https://github.com/UnpredictablePrashant/TravelMemory.git
cd TravelMemory/backend
Step 4: Create .env File
nano .env
Add:

PORT=3001
MONGO_URI='ENTER_YOUR_MONGODB_CONNECTION_STRING'
Save and exit.

Step 5: Install Dependencies
npm install
Step 6: Start Backend Server
node index.js
Your backend should now be running on:

http://<EC2_PUBLIC_IP>:3001
Connecting the Application to MongoDB Atlas
This guide explains how to set up MongoDB Atlas and connect it to your application and MongoDB Compass.

Steps to Set Up MongoDB on Atlas
1. Log in to MongoDB Atlas
Visit: https://cloud.mongodb.com/
Log in to your MongoDB Atlas account.

2. Create Organization and Project
From the dashboard, create:

An Organization

A Project inside that organization

3. Create a Cluster
Click Create Cluster

Name the cluster: herocluster1

Select the M0 Free Plan

Click Create Deployment

4. Create a Database User
Go to Database Access

Click Add New Database User

Set username and password

Click Create DB User

5. Configure Network Access
Go to Network Access

Click Add IP Address

Enter:

0.0.0.0/0
⚠ Security Tip: For production environments, avoid using 0.0.0.0/0. Whitelist only trusted IP addresses.

6. Connect MongoDB to Compass
Go to the Database section

Click Connect

Select Compass

Choose: I already have MongoDB Compass

7. Copy the Connection String
Example format:

mongodb+srv://<username>:<password>@cluster0.mongodb.net/?retryWrites=true&w=majority
8. Connect via MongoDB Compass
Open MongoDB Compass

Paste the connection string

Click Connect

You're Connected ✅

Running Frontend & Backend with Custom Domains
Frontend:

https://binduweb.site
Backend:

https://api.binduweb.site
Step 1: Point Domains to EC2 IP
Type	Name	Value
A	@	<EC2_PUBLIC_IP>
A	www	<EC2_PUBLIC_IP>
A	api	<EC2_PUBLIC_IP>
Step 2: Create Separate Nginx Server Blocks
# Frontend
server {
    listen 80;
    server_name binduweb.site www.binduweb.site;

    location / {
        proxy_pass http://<EC2_PUBLIC_IP>:3000;
    }
}

# Backend
server {
    listen 80;
    server_name api.binduweb.site;

    location / {
        proxy_pass http://<EC2_PUBLIC_IP>:3001;
    }
}
Reload:

nginx -t
systemctl reload nginx
Install Certbot and Enable SSL (Frontend)
sudo apt update
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d binduweb.site -d www.binduweb.site
Access securely:

https://binduweb.site
https://www.binduweb.site
Install Certbot and Enable SSL (Backend)
sudo certbot --nginx -d api.binduweb.site
Access securely:

https://api.binduweb.site
Deployment Complete 🎉
Your Travel Memory application is now:

Secure (HTTPS)

Scalable (ASG + ALB)

Highly Available

Production-ready

Author
bindu
GitHub: https://github.com/bindu818309-bit


