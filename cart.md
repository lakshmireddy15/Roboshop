
# Cart Service Setup - RoboShop E-commerce App

The **Cart Service** is responsible for managing the user's cart in the RoboShop application.  
It is written in **NodeJS** and depends on **Catalogue** and **Redis** services.

> ✅ **Note**: Confirm the exact version with the developer, but it works with NodeJS >= 20.

---

## 1. Configure NodeJS Version

List available NodeJS modules:

```bash
dnf module list nodejs
```

Disable default NodeJS module:

```bash
dnf module disable nodejs -y
```

Enable NodeJS 20:

```bash
dnf module enable nodejs:20 -y
```

---

## 2. Install NodeJS

```bash
dnf install nodejs -y
```

---

## 3. Create Application User

```bash
useradd --system --home /app --shell /sbin/nologin --comment "roboshop system user" roboshop
mkdir /app
```

---

## 4. Download Application Code

```bash
curl -L -o /tmp/cart.zip https://roboshop-artifacts.s3.amazonaws.com/cart-v3.zip
cd /app
unzip /tmp/cart.zip
```

---

## 5. Install NodeJS Dependencies

```bash
cd /app
npm install
```

---

## 6. Configure Systemd Service

Create the service file:

```bash
vim /etc/systemd/system/cart.service
```

Paste the following content (replace placeholders with actual IPs):

```ini
[Unit]
Description=Cart Service

[Service]
User=roboshop
Environment=REDIS_HOST=<REDIS-SERVER-IP>
Environment=CATALOGUE_HOST=<CATALOGUE-SERVER-IP>
Environment=CATALOGUE_PORT=8080
ExecStart=/bin/node /app/server.js
SyslogIdentifier=cart

[Install]
WantedBy=multi-user.target
```

---

## 7. Start and Enable the Service

```bash
systemctl daemon-reload
systemctl enable cart
systemctl start cart
```

---

## ✅ Cart Service Setup Complete

Check the status of the service:

```bash
systemctl status cart
```

---
