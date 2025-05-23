
# MongoDB Setup on RedHat (RHEL 9)

## Introduction

- **MongoDB** is a NoSQL database.
- It is **free** and **open-source**.
- Stores data in **key-value** pairs using **JSON format**.
- In SQL, we have **tables**; in MongoDB, we have **collections**.
- MongoDB is suitable for **big data** and **real-time applications**.

### Example Document Format

```json
{
  "name": "MongoDB",
  "email": "mongodb.@gmail.com"
}
```

---

## Setting Up MongoDB Repository

### Default Repo Location

```
/etc/yum.repos.d/
```

### Switch to Root User

```bash
sudo su -
```

### Create MongoDB Repo File

```bash
vim /etc/yum.repos.d/mongo.repo
```

Paste the following content:

```ini
[mongodb-org-7.0]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/9/mongodb-org/7.0/x86_64/
enabled=1
gpgcheck=0
```

Save and exit:

```
:wq!
```

---

## Install MongoDB

```bash
dnf install mongodb-org -y
```

---

## Enable and Start MongoDB

```bash
systemctl enable mongod
systemctl start mongod
```

---

## Verify MongoDB is Listening

```bash
netstat -lntp
```

By default, MongoDB listens on `127.0.0.1`, which means **local access only**.

---

## Allow External Connections

### Edit MongoDB Config

```bash
vim /etc/mongod.conf
```

Update the bind IP from:

```
bindIp: 127.0.0.1
```

To:

```
bindIp: 0.0.0.0
```

> `0.0.0.0` allows MongoDB to accept connections from **any external IP address**.

### Restart MongoDB

```bash
systemctl restart mongod
```

### Confirm Public Access

```bash
netstat -lntp
```

Now MongoDB is open to public connections.
