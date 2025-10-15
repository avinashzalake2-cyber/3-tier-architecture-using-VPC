# AWS 3-Tier Architecture Project (Simple Version)

This project demonstrates a **simple 3-tier architecture** setup on **AWS**, including:
- **Frontend (HTML/CSS)**
- **Backend (PHP)**
- **Database (MySQL)**

It does not use RDS, ALB, or other managed services — just EC2 instances in separate subnets.

---

## 🧩 Architecture Overview

**Layers:**
1. **Frontend Layer (Web Tier):**
   - Contains HTML and CSS files.
   - Deployed on a Public Subnet EC2 with Apache.

2. **Backend Layer (App Tier):**
   - Contains PHP logic to process requests.
   - Deployed on a Private Subnet EC2 with PHP and Apache.

3. **Database Layer (DB Tier):**
   - Contains MySQL database for storing data.
   - Deployed on a Private Subnet EC2 with MySQL.

---

## ⚙️ Steps to Set Up

### 1️⃣ Create Network (VPC and Subnets)

1. Create a VPC (CIDR: 10.0.0.0/16)
2. Create 3 Subnets:
   - Public Subnet → Frontend
   - Private Subnet 1 → Backend
   - Private Subnet 2 → Database
3. Attach Internet Gateway to VPC.
4. Create Route Tables:
   - Public RT → 0.0.0.0/0 → IGW
   - Private RT → internal communication only.

---

### 2️⃣ Launch EC2 Instances

| Tier | Subnet Type | Purpose | Software |
|------|--------------|----------|-----------|
| Frontend | Public | Host HTML/CSS | Apache |
| Backend | Private | Handle PHP logic | Apache + PHP |
| Database | Private | Store data | MySQL |

---

### 3️⃣ Frontend Setup

1. SSH into frontend instance:
   ```bash
   sudo yum install httpd -y
   sudo systemctl enable httpd
   sudo systemctl start httpd
   ```
2. Copy your files:
   ```bash
   sudo mv index.html style.css /var/www/html/
   ```
3. Edit **index.html** → replace `<BACKEND-PRIVATE-IP>` with backend instance private IP.

---

### 4️⃣ Backend Setup

1. SSH into backend instance:
   ```bash
   sudo yum install httpd php -y
   sudo systemctl enable httpd
   sudo systemctl start httpd
   ```
2. Upload `submit.php` into `/var/www/html/`
3. Edit **submit.php** → replace `<DB-PRIVATE-IP>` with database instance private IP.

---

### 5️⃣ Database Setup

1. SSH into DB instance:
   ```bash
   sudo yum install mysql-server -y
   sudo systemctl enable mysqld
   sudo systemctl start mysqld
   ```
2. Log in to MySQL:
   ```bash
   mysql -u root -p
   ```
3. Run SQL script:
   ```bash
   SOURCE /path/to/studentdb.sql;
   ```

---

## 🌐 How It Works

- User visits **Frontend** via public IP.
- Frontend form sends data to **Backend** (`submit.php`).
- Backend connects to **MySQL** on the **Database** instance.
- Data gets stored in the database table `students`.

---

## 📁 Folder Structure

```
3-tier-architecture-aws/
│
├── frontend/
│   ├── index.html
│   └── style.css
│
├── backend/
│   └── submit.php
│
├── database/
│   └── studentdb.sql
│
└── README.md
```

---

## 💡 Notes

- Security Groups:
  - Frontend → allow HTTP (80) + SSH (22)
  - Backend → allow HTTP (80) from Frontend SG only
  - Database → allow MySQL (3306) from Backend SG only
- Replace placeholders `<BACKEND-PRIVATE-IP>` and `<DB-PRIVATE-IP>` in your files.

---

## 🏁 Author
**Arkan Tandel**
