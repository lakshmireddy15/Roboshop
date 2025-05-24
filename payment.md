
# Payment Service Setup - RoboShop E-commerce App

This guide walks through the setup of the **Payment Service** for the RoboShop application. This service is written in **Python 3.x**, and interacts with the **Cart**, **User**, and **RabbitMQ** services.

> ✅ **Note**: Confirm with the developer the exact version of Python 3.x required.

---

## 1. Install Python and Dependencies

```bash
dnf install python3 gcc python3-devel -y
```

---

## 2. Add DNS Entries

Update the private IPs of required services in your `/etc/hosts` or DNS management system as per your setup.

---

## 3. Create Application User

```bash
useradd --system --home /app --shell /sbin/nologin --comment "roboshop system user" roboshop
mkdir /app
```

---

## 4. Download Application Code

```bash
curl -L -o /tmp/payment.zip https://roboshop-artifacts.s3.amazonaws.com/payment-v3.zip
cd /app
unzip /tmp/payment.zip
```

---

## 5. Install Python Dependencies

```bash
cd /app
pip3 install -r requirements.txt
```

---

## 6. Configure Systemd Service

Create the service file:

```bash
vim /etc/systemd/system/payment.service
```

Paste the following content (replace placeholders with actual IPs):

```ini
[Unit]
Description=Payment Service

[Service]
User=root
WorkingDirectory=/app
Environment=CART_HOST=<CART-SERVER-IPADDRESS>
Environment=CART_PORT=8080
Environment=USER_HOST=<USER-SERVER-IPADDRESS>
Environment=USER_PORT=8080
Environment=AMQP_HOST=<RABBITMQ-SERVER-IPADDRESS>
Environment=AMQP_USER=roboshop
Environment=AMQP_PASS=roboshop123

ExecStart=/usr/local/bin/uwsgi --ini payment.ini
ExecStop=/bin/kill -9 $MAINPID
SyslogIdentifier=payment

[Install]
WantedBy=multi-user.target
```

---

## 7. Start and Enable the Service

```bash
systemctl daemon-reload
systemctl enable payment
systemctl start payment
```

---

## ✅ Payment Service Setup Complete
Ensure the service is running:

```bash
systemctl status payment
```

---
