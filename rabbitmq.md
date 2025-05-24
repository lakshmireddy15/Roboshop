
# RabbitMQ Setup - RoboShop E-commerce App

RabbitMQ is a **message queue** system used by some components of the RoboShop application to communicate asynchronously.

---

## 1. Configure RabbitMQ Repository

Create the repo file:

```bash
vim /etc/yum.repos.d/rabbitmq.repo
```

Paste the following content:

```ini
[modern-erlang]
name=modern-erlang-el9
baseurl=https://yum1.novemberain.com/erlang/el/9/$basearch
        https://yum2.novemberain.com/erlang/el/9/$basearch
        https://dl.cloudsmith.io/public/rabbitmq/rabbitmq-erlang/rpm/el/9/$basearch
enabled=1
gpgcheck=0

[modern-erlang-noarch]
name=modern-erlang-el9-noarch
baseurl=https://yum1.novemberain.com/erlang/el/9/noarch
        https://yum2.novemberain.com/erlang/el/9/noarch
        https://dl.cloudsmith.io/public/rabbitmq/rabbitmq-erlang/rpm/el/9/noarch
enabled=1
gpgcheck=0

[modern-erlang-source]
name=modern-erlang-el9-source
baseurl=https://yum1.novemberain.com/erlang/el/9/SRPMS
        https://yum2.novemberain.com/erlang/el/9/SRPMS
        https://dl.cloudsmith.io/public/rabbitmq/rabbitmq-erlang/rpm/el/9/SRPMS
enabled=1
gpgcheck=0

[rabbitmq-el9]
name=rabbitmq-el9
baseurl=https://yum2.novemberain.com/rabbitmq/el/9/$basearch
        https://yum1.novemberain.com/rabbitmq/el/9/$basearch
        https://dl.cloudsmith.io/public/rabbitmq/rabbitmq-server/rpm/el/9/$basearch
enabled=1
gpgcheck=0

[rabbitmq-el9-noarch]
name=rabbitmq-el9-noarch
baseurl=https://yum2.novemberain.com/rabbitmq/el/9/noarch
        https://yum1.novemberain.com/rabbitmq/el/9/noarch
        https://dl.cloudsmith.io/public/rabbitmq/rabbitmq-server/rpm/el/9/noarch
enabled=1
gpgcheck=0
```

---

## 2. Install RabbitMQ

```bash
dnf install rabbitmq-server -y
```

---

## 3. Enable and Start RabbitMQ

```bash
systemctl enable rabbitmq-server
systemctl start rabbitmq-server
```

---

## 4. Add RabbitMQ User for RoboShop

By default, RabbitMQ has a user `guest/guest` which is restricted to localhost.  
Create a new user for application access:

```bash
rabbitmqctl add_user roboshop roboshop123
```

---

## 5. Set Permissions for the User

```bash
rabbitmqctl set_permissions -p / roboshop ".*" ".*" ".*"
```

### 🔍 Explanation:

- `-p /` : Virtual host `/` (default).
- `roboshop` : Username.
- `".*"` : Regular expressions allowing full access to:
  - Configure
  - Write
  - Read

This gives the `roboshop` user **full access** to all resources in the default virtual host.

---

## ✅ RabbitMQ Setup Complete
Verify status:

```bash
systemctl status rabbitmq-server
```

---
