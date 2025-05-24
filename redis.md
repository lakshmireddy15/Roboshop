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

### Connecting to Redis

1. **Switch to root user:**
```bash
sudo su -
Disable the default Redis module (if already installed):
dnf module disable redis -y
Enable Redis 7 module:
dnf module enable redis:7 -y
Install Redis:
dnf install redis -y
Configure Redis to allow connections:

Edit the redis.conf file (usually located at /etc/redis/redis.conf):

Bind Redis to 0.0.0.0 (allow external connections).

Ensure the following line is present:
bind 0.0.0.0
Save and exit the file:
:wq!
systemctl enable redis
systemctl start redis
netstat -lntp
tcp   0   0 0.0.0.0:6379   0.0.0.0:*   LISTEN
Redis Default Port
Port Number: 6379
