# 🚀 Three-Tier CRUD Application on AWS EC2

![AWS](https://img.shields.io/badge/AWS-EC2-orange?style=for-the-badge&logo=amazon-aws)
![React](https://img.shields.io/badge/React-Vite-blue?style=for-the-badge&logo=react)
![Node.js](https://img.shields.io/badge/Node.js-Express-green?style=for-the-badge&logo=node.js)
![MySQL](https://img.shields.io/badge/Database-MySQL-blue?style=for-the-badge&logo=mysql)
![PM2](https://img.shields.io/badge/Process_Manager-PM2-red?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)

---

## 📌 Project Overview

A **production-style Three-Tier CRUD Web Application** deployed on **AWS EC2**.
This project demonstrates real-world cloud deployment skills including server
configuration, process management, and full-stack application deployment on AWS.

> 💡 Built as part of my DevOps learning journey under the guidance of
> **[Awais Latif](https://www.linkedin.com/in/awais-latif)** — Senior DevOps Engineer

---

## 🏗️ Architecture

```
                    ┌─────────────────────┐
                    │      Browser        │
                    └──────────┬──────────┘
                               │
                    ┌──────────▼──────────┐
                    │   EC2 Public IP     │
                    │   Ubuntu Server     │
                    └──────────┬──────────┘
                               │
               ┌───────────────┼───────────────┐
               │                               │
   ┌───────────▼───────────┐   ┌───────────────▼──────────┐
   │  TIER 1 — Frontend    │   │   TIER 2 — Backend       │
   │  React.js + Vite      │   │   Node.js + Express      │
   │  Port: 5000           │◄──│   Port: 3000             │
   │  Managed by PM2       │   │   Managed by PM2         │
   └───────────────────────┘   └───────────────┬──────────┘
                                               │
                               ┌───────────────▼──────────┐
                               │   TIER 3 — Database      │
                               │   MySQL                  │
                               │   Port: 3306             │
                               └──────────────────────────┘
```
---

## 🔄 CI/CD Pipeline

```
Developer pushes code to GitHub
            ↓
GitHub Actions workflow triggers
            ↓
Self-Hosted Runner on EC2 picks up job
            ↓
Installs dependencies (npm install)
            ↓
Restarts PM2 processes automatically
            ↓
App deployed in ~27 seconds! ✅
```

| Deploy Success |
|----------------|----------------|
| ![cicd](assets/github-actions.png) |

---


---

## ✨ Features

- ➕ **Create** — Add new user records
- 👁️ **Read** — View all users in a table
- ✏️ **Update** — Edit existing user details
- 🗑️ **Delete** — Remove user records
- 🔄 **REST API** — Clean API endpoints
- ☁️ **Cloud Deployed** — Live on AWS EC2
- 🤖 **Auto Deploy** — GitHub Actions CI/CD
---

## 🛠️ Tech Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| 🖥️ Frontend | React.js + Vite + Axios | User Interface |
| ⚙️ Backend | Node.js + Express + Sequelize | REST API |
| 🗄️ Database | MySQL | Data Storage |
| 🔄 Process Manager | PM2 | 24/7 App Running |
| ☁️ Cloud Server | AWS EC2 (Ubuntu) | Hosting |
| 🔒 Security | AWS Security Groups | Port Management |
| 🤖 CI/CD | GitHub Actions + Self-Hosted Runner | Auto Deployment |
| 📦 Version Control | Git + GitHub | Source Code |

---

## ☁️ AWS Services Used

| Service | Usage |
|---------|-------|
| EC2 | Ubuntu server to host the app |
| Security Groups | Open ports 3000 and 5000 |

---

## 📋 Prerequisites

Before you begin, make sure you have:

- ✅ AWS EC2 Ubuntu instance running
- ✅ Node.js v18+
- ✅ MySQL installed
- ✅ PM2 installed globally
- ✅ Git installed

---

## 🚀 Deployment Steps

### Step 1 — Clone Repository
```bash
git clone https://github.com/TUMHARA_USERNAME/3tier-react-node-mysql.git
cd 3tier-react-node-mysql
```

### Step 2 — MySQL Database Setup
```bash
sudo mysql
```
```sql
CREATE DATABASE crud_operations;
CREATE USER 'cruduser'@'localhost' IDENTIFIED BY 'Password@123';
GRANT ALL PRIVILEGES ON crud_operations.* TO 'cruduser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

### Step 3 — Backend Setup
```bash
# Go to backend folder
cd backend

# Install dependencies
npm install

# Create environment file
cp .env.example .env
nano .env
```
Add these values in `.env`:
```env
DB_HOST=localhost
DB_USER=cruduser
DB_PASSWORD=Password@123
DB_DATABASE=crud_operations
```
```bash
# Start backend with PM2
pm2 start index.js --name api-server --watch --env PORT=3000

# Check status
pm2 status
```

### Step 4 — Frontend Setup
```bash
# Go to frontend folder
cd ../frontend

# Install dependencies
npm install

# Create environment file
cp .env.example .env
nano .env
```
Add this value in `.env`:
```env
VITE_API_URL=http://YOUR_EC2_PUBLIC_IP:3000
```
```bash
# Start frontend with PM2
pm2 start npm --name "react-app" -- run dev -- --host 0.0.0.0

# Check status
pm2 status
```

### Step 5 — AWS Security Group
```
AWS Console → EC2 → Security Groups → Inbound Rules → Add:
✅ Port 3000 — Backend API
✅ Port 5000 — Frontend App
Source: 0.0.0.0/0
```
### Step 6 — Setup CI/CD (GitHub Actions Self-Hosted Runner)
```bash
# Create runner directory
mkdir actions-runner && cd actions-runner

# Download runner
curl -o actions-runner-linux-x64-2.334.0.tar.gz -L \
https://github.com/actions/runner/releases/download/v2.334.0/actions-runner-linux-x64-2.334.0.tar.gz

# Extract
tar xzf ./actions-runner-linux-x64-2.334.0.tar.gz

# Configure (get token from GitHub → Settings → Actions → Runners)
./config.sh --url https://github.com/YOUR_USERNAME/YOUR_REPO --token YOUR_TOKEN

# Install as service
sudo ./svc.sh install
sudo ./svc.sh start
```

### Step 7 — Access Application
```
🌐 Frontend : http://YOUR_EC2_IP:5000
🔌 Backend  : http://YOUR_EC2_IP:3000
```

## 📸 Screenshots

### 🖥️ Frontend UI
![Frontend](assets/frontend-home.png)

### ➕ Add User
![Add User](assets/add-user.png)

### ✏️ Edit User
![Edit User](assets/edit-user.png)

### 🗑️ Delete User
![Delete User](assets/delete-user.png)

### 📋 User List
![User List](assets/added-user-list.png)

---

### ⚙️ Backend
![Backend](assets/backend-page.png)

### 📡 PM2 Status
![PM2](assets/pm2-status.png)

---

### ☁️ AWS EC2 Instance
![EC2](assets/ec2-instance.png)

### 🔐 Security Group
![Security Group](assets/security-group.png)

---

### 🔄 GitHub Actions CI/CD
![GitHub Actions](assets/github-actions.png)

### 🏗️ Architecture Diagram
![Architecture](assets/architecture.png)

---


## 📚 What I Learned

```
✅ AWS EC2 instance setup and configuration
✅ MySQL database setup on Linux server
✅ Node.js process management with PM2
✅ React + Node.js full-stack deployment
✅ AWS Security Groups and port management
✅ Environment variables configuration
✅ End-to-end 3-tier architecture deployment
✅ GitHub Actions CI/CD pipeline setup
✅ Self-hosted runner configuration on EC2
✅ Automated deployment on every code push
```

---

##  Credits & Acknowledgements

| Role | Person |
|------|--------|
| 🎯 Project Guided by | **[Awais Latif](https://www.linkedin.com/in/m-awais-latif)** — Senior DevOps Engineer |
| 📦 Original Project | **[Mahadi Hassan Razib](https://github.com/mahadihassanrazib)** |

---

## 👩‍💻 Author

** ILSA MUKHTAR **

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=flat&logo=linkedin)](https://linkedin.com/in/ilsa-mukhtar)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-black?style=flat&logo=github)](https://github.com/ilsamukhtar)

---

## 📄 License

This project is licensed under the **MIT License** — see the
[LICENSE](LICENSE) file for details.

---

<div align="center">

### ⭐ If you found this helpful, please star this repo! ⭐


</div>
