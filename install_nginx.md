# Installing Nginx on RHEL-based System

## Step 1: Switch to Root User
```bash
sudo su -
```

## Step 2: Check Nginx Module List
```bash
dnf module list nginx
```

## Step 3: Disable Nginx Module
We disable the default Nginx module to manually choose the version we want to install.

```bash
dnf module disable nginx -y
```

**Note**: `-y` automatically confirms the action.

## Step 4: Enable Specific Version of Nginx
Example: Enabling version `1.24`.

```bash
dnf module enable nginx:1.24 -y
```

## Step 5: Install Nginx
```bash
dnf install nginx -y
```

## Step 6: Start Nginx Service
```bash
systemctl start nginx
```

## Step 7: Check Nginx Service Status
```bash
systemctl status nginx
```

## Step 8: Check Nginx Process
```bash
ps -ef | grep nginx
```

## Step 9: Check Listening Ports
```bash
netstat -lntp
```

## Step 10: Verify Installation
- Copy your server's **public IP address**.
- Open a browser and paste the IP.
- If you see the **Red Hat Nginx welcome page**, installation was successful.

## Step 11: Enable Nginx on Boot
This ensures Nginx starts automatically after a server reboot.

```bash
systemctl enable nginx
```

---

## Important Directories and Files

### Main Configuration File
```bash
cd /etc/nginx/
```
- File: `nginx.conf`
- This is where you make configuration changes.

**Note**: After making changes, restart Nginx to apply them:
```bash
systemctl restart nginx
```

### Default Web Directory
```bash
cd /usr/share/nginx/html
```

- This is the default directory for website files (e.g., `index.html`).

---

✅ **Nginx is now successfully installed and running!**
