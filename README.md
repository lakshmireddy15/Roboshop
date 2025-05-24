# Roboshop

## Tiers

- **Frontend Tier**
- **Backend Tier**
- **Database Tier**

---

## Frontend

- **Nginx**
- **Client**
  - Serves: HTML pages

---

## Backend Applications

### 1. Catalogue Application
- Language: **Node.js**
- Dependencies:
  - **MongoDB**

### 2. User Application
- Language: **Node.js**
- Dependencies:
  - **MongoDB**
  - **Redis DB**

### 3. Cart Application
- Language: **Node.js**
- Dependencies:
  - **Redis DB**
  - **Catalogue Application**

### 4. Shipping Application
- Language: **Java**
- Dependencies:
  - **MySQL DB**
  - **Cart Application**

### 5. Payment Application
- Language: **Python**
- Dependencies:
  - **Cart Application**
  - **User Application**
  - **RabbitMQ DB**

### 6. Dispatch Application
- Language: **Golang**
- Dependencies:
  - **RabbitMQ DB**
