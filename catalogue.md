
# Catalogue Microservice Setup Guide

## What is Catalogue?
Catalogue is a structured collection of product information. It is a microservice responsible for serving all item listings displayed on the Roboshop application. This is a Node.js-based application.

---

## Node.js Setup

### Switch to root user:
```bash
sudo su -
```

### Check available Node.js versions:
```bash
dnf module list nodejs
```

### Enable Node.js 20 version:
```bash
dnf module disable nodejs
dnf module enable nodejs:20 -y
dnf install nodejs -y
```

---

## Service Check Commands

### Check Nginx: In frontend server
```bash
ps -ef | grep nginx
```

### Check MongoDB: in mongodb server
```bash
ps -ef | grep mongo
```

---
here mongo and nginx are the system users
## User Setup and Permissions

- Services should not run on root credentials for security.
- Create a **system user** for running services:
```bash
useradd --system --home /app --shell /sbin/nologin --comment "roboshop system user" roboshop
```

---

## Application Setup

### Create application directory:
```bash
mkdir /app
```

### Download and unzip the application:
```bash
curl -o /tmp/catalogue.zip https://roboshop-artifacts.s3.amazonaws.com/catalogue-v3.zip
cd /app
unzip /tmp/catalogue.zip
```

---

## Install Dependencies

### View `package.json`:
```bash
cat package.json
```

### Install node dependencies:
```bash
npm install
```

---

## Create a Systemd Service File

### Create a new service file:
```bash
vim /etc/systemd/system/catalogue.service
```

### Paste the following:
```ini
[Unit]
Description = Catalogue Service

[Service]
User=roboshop
Environment=MONGO=true
Environment=MONGO_URL="mongodb://mongodb.lakshmireddy.site:27017/catalogue"
ExecStart=/bin/node /app/server.js
SyslogIdentifier=catalogue

[Install]
WantedBy=multi-user.target
```

---

## Start and Enable the Service
```bash
systemctl daemon-reload
systemctl enable catalogue
systemctl start catalogue
```

### Check if service is running:
```bash
netstat -lntp
```

---

## MongoDB Client Setup

### Add MongoDB repository:
```bash
vim /etc/yum.repos.d/mongo.repo
```

#### Paste:
```ini
[mongodb-org-7.0]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/9/mongodb-org/7.0/x86_64/
enabled=1
gpgcheck=0
```

### Install MongoDB client:
```bash
dnf install mongodb-mongosh -y
```

---

## Load Master Data into MongoDB

### Navigate to db folder and load script:
```bash
cd /app/db
mongosh --host mongodb.lakshmireddy.site </app/db/master-data.js
```

---

## Verify MongoDB Data Load

### Connect and check:
```bash
mongosh --host mongodb.lakshmireddy.site
show dbs
use catalogue
show collections
db.products.find()
```

### Test MongoDB connection from server:
```bash
telnet lakshmireddy.site 27017
```

---

## Frontend Configuration

### Update NGINX configuration:
```bash
vim /etc/nginx/nginx.conf
```

### Restart NGINX:
```bash
systemctl restart nginx
```

### Open Browser and access:
```
http://lakshmireddy.site
```

---

## NGINX Logs

- Access Logs: `/var/log/nginx/access.log`
- Message Logs: `/var/log/message`
