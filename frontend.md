# RoboShop Frontend Setup with Nginx

This guide describes how to configure the **Frontend service** in RoboShop to serve web content using **Nginx** on a **Linux server**.

---

## 🛰 Update Private IP to Route 53

Update the private IP in Route 53 so it can be resolved by a custom domain.

---

## 💻 System Architecture

- **Linux** is the physical server.
- **Nginx** is a virtual service running on the Linux server.
- When we deploy the application on Nginx, it becomes accessible through a browser.

---

## 🧑‍💻 Step-by-Step Setup

### Step 1: Switch to Root User
```bash
sudo su -
```

### Step 2: Check Available Modules
```bash
dnf module list
```

### Step 3: Disable Default Nginx Module
```bash
dnf module disable nginx -y
```

### Step 4: Enable Specific Nginx Version
```bash
dnf module enable nginx:1.24 -y
```

### Step 5: Install Nginx
```bash
dnf install nginx -y
```

### Step 6: Start Nginx
```bash
systemctl start nginx
```

### Step 7: Check Nginx Status
```bash
systemctl status nginx
```

---

## 🧹 Clean Default Content

### Default Directory
```bash
cd /usr/share/nginx/html
```

### Remove Default Content
```bash
rm -rf /usr/share/nginx/html/*
```

---

## 📁 Create Directory for Frontend

```bash
mkdir -p /usr/share/nginx/html
```

---

## ⬇️ Download Frontend Content

```bash
curl -o /tmp/frontend.zip https://roboshop-artifacts.s3.amazonaws.com/frontend-v3.zip
```

### Verify Download
```bash
cd /tmp
ls -l
```

---

## 📦 Extract Frontend Content

```bash
cd /usr/share/nginx/html
unzip /tmp/frontend.zip
```

---

## ⚙️ Update Nginx Configuration

### Open Configuration File
```bash
vim /etc/nginx/nginx.conf
```

### Delete Existing Content
Inside vim:
```
:%d
```

### Paste the Following Configuration
```nginx
user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log notice;
pid /run/nginx.pid;

include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile            on;
    tcp_nopush          on;
    keepalive_timeout   65;
    types_hash_max_size 4096;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    include /etc/nginx/conf.d/*.conf;

    server {
        listen       80;
        listen       [::]:80;
        server_name  _;
        root         /usr/share/nginx/html;

        include /etc/nginx/default.d/*.conf;

        error_page 404 /404.html;
        location = /404.html {
        }

        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
        }

        location /images/ {
          expires 5s;
          root   /usr/share/nginx/html;
          try_files $uri /images/placeholder.jpg;
        }

        location /api/catalogue/ { proxy_pass http://localhost:8080/; }
        location /api/user/ { proxy_pass http://localhost:8080/; }
        location /api/cart/ { proxy_pass http://localhost:8080/; }
        location /api/shipping/ { proxy_pass http://localhost:8080/; }
        location /api/payment/ { proxy_pass http://localhost:8080/; }

        location /health {
          stub_status on;
          access_log off;
        }
    }
}
```

---

## 🔄 Restart Nginx to Apply Changes

```bash
systemctl restart nginx
```

---

## 🌐 Access RoboShop Frontend

- Open a browser and enter the **public IP address** of your server.
- You should see the **RoboShop Frontend** page.

---

## 🌍 Domain Configuration

- Instead of sharing the public IP with customers, configure a domain name using Route 53.
- Share the domain URL for better accessibility and branding.

---

✅ **Your RoboShop Frontend is now live and accessible!**
