# 🧾 Student Registration Website – Deployment Documentation

## 📘 Application Information

### What is the App?
- This is a **Student Registration Website**.  
- Students who take admission in our institute are registered on this website.

---

## ⚙️ How to Build Each Component

### 🖥️ Frontend
- Node.js and npm must be installed.

### 🧠 Backend
- Java Development Kit (**JDK 17** or higher) must be installed.  
- Maven must be installed.

### 🗄️ Database
- Install **MariaDB**.

---

## 🔧 Pre-Requirements

Before starting, make sure:
- ✅ **RDS** (Relational Database Service) is created.  
- ✅ **EC2 Instance** is launched.  
- ✅ **Docker** is installed.  
- ✅ **MySQL Client** is installed.

---

## 🪜 Steps and Commands

### Step 1: Prepare the EC2 Instance
```bash
# Switch to root user
sudo -i

# Update the instance
apt update

# Install Docker
apt install docker.io -y

# Install MySQL client
apt install mysql-client -y

🧩 Step 2: Clone the GitHub Repository
# Clone your project from GitHub
git clone <GitHub_Repository_Link>

# Example:
# git clone https://github.com/username/student-registration.git
