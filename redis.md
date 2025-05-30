# Setting Up Redis and Caching in the Application

This document outlines the steps to connect to Redis, configure caching, and ensure live updates for your application. Redis will temporarily store data, allowing faster retrieval compared to querying the database every time.

## Steps to Connect to Redis and Integrate Caching

1. **Send a connection request to the database:**
   - The application sends a request to the database for the required data.
   
2. **Establish a connection to the database:**
   - The database connection is established, which might take some time.

3. **Run a query against the database:**
   - A query is executed on the database to fetch the data.

4. **Database fetches the data:**
   - The database fetches the requested data and sends it to the application.

5. **Send the response to the application:**
   - The application receives the data from the database.

6. **Close the communication:**
   - The database connection is closed.

However, this process may take some time, especially if the database is slow or if the data is frequently requested. To optimize this:

### Caching with Redis

1. **Store the first-time data in the cache (Redis):**
   - The data is stored in Redis the first time it is requested.

app ---> cache

2. **Subsequent requests check the cache:**
- If the data is available in the cache, it will be fetched directly from there.

app ---> cache (cache hit)

3. **If data is not in the cache, query the database:**
- If the requested data is not in the cache, the application will query the database and then store the data in the cache for future use.

app ---> cache ---> DB ---> cache (cache miss)

## Redis Setup Instructions

### Update DNS Record for Redis
Before starting Redis, update the DNS record if necessary to ensure proper communication.

# Connecting to Redis

## 1. Switch to Root User

```bash
sudo su -
```

## 2. Disable the Default Redis Module (if already installed)

```bash
dnf module disable redis -y
```

## 3. Enable Redis 7 Module

```bash
dnf module enable redis:7 -y
```

## 4. Install Redis

```bash
dnf install redis -y
```

## 5. Configure Redis to Allow External Connections

### Edit the Redis Configuration File

The configuration file is usually located at:

```
/etc/redis/redis.conf
```

Edit the file using `vi` or your preferred text editor:

```bash
vi /etc/redis/redis.conf
```

### Modify the `bind` Parameter

Find and update the following line:

```conf
bind 0.0.0.0
```

This allows Redis to accept connections from any IP address.

### Save and Exit

Press `Esc`, then type:

```
:wq!
```

and hit `Enter` to save and exit.

## 6. Enable and Start Redis

```bash
systemctl enable redis
systemctl start redis
```

## 7. Verify Redis is Listening on Port 6379

```bash
netstat -lntp
```

You should see output like:

```
tcp   0   0 0.0.0.0:6379   0.0.0.0:*   LISTEN
```

## Redis Default Port

- **Port Number:** `6379`

