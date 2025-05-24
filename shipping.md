
# Shipping Service Setup - RoboShop E-commerce App

The **Shipping Service** is a Java-based application that handles shipping logic in the RoboShop platform.  
It is dependent on **Cart** and **MySQL** services.

---

## Java Application Basics

- Java source files: `.java`
- Compiled bytecode: `.class`
- Packaged application: `.jar` file
- Build Tool: **Maven**
- Build Configuration File: `pom.xml`

---

## 1. Install Maven (installs Java too)

```bash
sudo dnf install maven -y
java --version
mvn --version
```

---

## 2. Create Application User

```bash
useradd --system --home /app --shell /sbin/nologin --comment "Roboshop system user" roboshop
mkdir /app
```

---

## 3. Download and Extract Application Code

```bash
cd /app
curl -L -o /tmp/shipping.zip https://roboshop-artifacts.s3.amazonaws.com/shipping-v3.zip
unzip /tmp/shipping.zip
```

---

## 4. Build the Application with Maven

```bash
cd /app
mvn clean package
```

This process creates:

- `target/shipping-1.0.jar` → Java packaged bytecode
- Compiled `.class` files under `target/classes`

---

## 5. Rename the JAR File

```bash
mv target/shipping-1.0.jar shipping.jar
```

---

## 6. Create Systemd Service File

```bash
vim /etc/systemd/system/shipping.service
```

Paste the following content (replace IP placeholders):

```ini
[Unit]
Description=Shipping Service

[Service]
User=roboshop
Environment=CART_ENDPOINT=<CART-SERVER-IPADDRESS>:8080
Environment=DB_HOST=<MYSQL-SERVER-IPADDRESS>
ExecStart=/bin/java -jar /app/shipping.jar
SyslogIdentifier=shipping

[Install]
WantedBy=multi-user.target
```

---

## 7. Enable and Start the Service

```bash
systemctl daemon-reload
systemctl enable shipping
systemctl start shipping
```

Check for errors in logs:

```bash
less /var/log/messages
```

---

## 8. Configure MySQL Database

Install MySQL client:

```bash
dnf install mysql -y
```

Run schema and seed scripts:

```bash
mysql -h <MYSQL-SERVER-IPADDRESS> -uroot -pRoboShop@1 < /app/db/schema.sql
mysql -h <MYSQL-SERVER-IPADDRESS> -uroot -pRoboShop@1 < /app/db/app-user.sql
mysql -h <MYSQL-SERVER-IPADDRESS> -uroot -pRoboShop@1 < /app/db/master-data.sql
```

Restart the shipping service:

```bash
systemctl restart shipping
```

---

## 9. Validate the Service

```bash
netstat -lntp
less /var/log/messages
```

---

## ✅ Shipping Service Setup Complete

This completes the setup for the Java-based Shipping Service in the RoboShop architecture.

---
