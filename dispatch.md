
# Dispatch Service Setup - RoboShop E-commerce App

The **Dispatch Service** is the third-party service in the RoboShop e-commerce application.  
It is responsible for **dispatching the product after purchase**. The service is written in **Go** and acts as a **consumer** from RabbitMQ where the **Payment Service** publishes the message.

---

## 1. Install Golang

```bash
dnf install golang -y
```

---

## 2. Create Application User

```bash
useradd --system --home /app --shell /sbin/nologin --comment "roboshop system user" roboshop
mkdir /app
```

---

## 3. Download Application Code

```bash
curl -L -o /tmp/dispatch.zip https://roboshop-artifacts.s3.amazonaws.com/dispatch-v3.zip
cd /app
unzip /tmp/dispatch.zip
```

---

## 4. Download Dependencies and Build

```bash
cd /app
go mod init dispatch
go get
go build
```

---

## 5. Configure Systemd Service

Create the service file:

```bash
vim /etc/systemd/system/dispatch.service
```

Paste the following content (replace placeholder with actual RabbitMQ IP):

```ini
[Unit]
Description=Dispatch Service

[Service]
User=roboshop
Environment=AMQP_HOST=RABBITMQ-IP
Environment=AMQP_USER=roboshop
Environment=AMQP_PASS=roboshop123
ExecStart=/app/dispatch
SyslogIdentifier=dispatch

[Install]
WantedBy=multi-user.target
```

---

## 6. Start and Enable the Service

```bash
systemctl daemon-reload
systemctl enable dispatch
systemctl start dispatch
```

---

## ✅ Dispatch Service Setup Complete
Ensure the service is running:

```bash
systemctl status dispatch
```

---
