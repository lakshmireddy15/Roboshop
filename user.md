
# User Service Setup (Node.js - Roboshop)

This guide walks you through setting up the **User** microservice of Roboshop using **Node.js**.

---

## 📦 Node.js Setup

```bash
dnf module disable nodejs -y
dnf module enable nodejs:20 -y
dnf install nodejs -y
```

---

## 👤 Create Application User

```bash
useradd --system --home /app --shell /sbin/nologin --comment "roboshop system user" roboshop
```

---

## 📁 Download and Extract User Application

```bash
mkdir /app
curl -L -o /tmp/user.zip https://roboshop-artifacts.s3.amazonaws.com/user-v3.zip
cd /app
unzip /tmp/user.zip
```

---

## 📥 Install Dependencies

```bash
npm install
```

---

## ⚙️ Create `user.service` SystemD File

```bash
vim /etc/systemd/system/user.service
```

### Paste the Following Content:

```ini
[Unit]
Description = User Service

[Service]
User=roboshop
Environment=MONGO=true
Environment=REDIS_URL='redis://redis.lakshmireddy.site:6379'
Environment=MONGO_URL="mongodb://mongodb.lakshmireddy.site:27017/users"
ExecStart=/bin/node /app/server.js
SyslogIdentifier=user

[Install]
WantedBy=multi-user.target
```

---

## 🔁 Reload & Start Service

```bash
systemctl daemon-reload
systemctl enable user
systemctl start user
```

---

## 📡 Validate Service

```bash
netstat -lntp
telnet redis.lakshmireddy.site 6379
```

---


