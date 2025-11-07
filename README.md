# Student Registration Website – Deployment Documentation

## Application Information

### What is the App?
- This is a **Student Registration Website**.  
- Students who take admission in our institute are registered on this website.

---

## How to Build Each Component

### Frontend
- Node.js and npm must be installed.

### Backend
- Java Development Kit (**JDK 17** or higher) must be installed.  
- Maven must be installed.

### Database
- Install **MariaDB**.

---

## Pre-Requirements

Before starting, make sure:
- **RDS** (Relational Database Service) is created.  
- **EC2 Instance** is launched.  
- **Docker** is installed.  
- **MySQL Client** is installed.

---

## Steps and Commands

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
```
---

## Step 2: Clone the GitHub Repository
Clone your project from GitHub
```
git clone <GitHub_Repository_Link>
```

Example:
git clone https://github.com/username/student-registration.git

---

# Database Setup 
## Step 1: Connect to your RDS

Use the MySQL client on EC2 or your local system:

```
mysql -h <rds-endpoint> -u <username> -p
```

Example:
mysql -h student-db.cle2abcd123.ap-south-1.rds.amazonaws.com -u admin -p

## Step 2: Create a Database

### If your application.properties has this line:

```
spring.datasource.url=jdbc:mysql://<rds-endpoint>:3306/studentdb
```

Then create a database named studentdb:

```
CREATE DATABASE studentdb;
GRANT ALL PRIVILEGES ON springbackend.* TO 'username'@'localhost' IDENTIFIED BY 'your_password';

```

## Step 3: Verify the Database
```
SHOW DATABASES;
```
You should see studentdb in the list.

## Step 4: Exit MariaDB
```
EXIT;
```
---
# Backend Setup
## Step 1: Navigate to Backend Directory
```
cd <GitHub_Repository_Name>/backend
```

## Step 2: Configure application.properties
```
nano application.properties
```

### Edit and Update:

Replace localhost with your RDS endpoint

Update database name, username, and password

Example:

```
spring.datasource.url=jdbc:mysql://<rds-endpoint>:3306/studentdb
spring.datasource.username=admin
spring.datasource.password=yourpassword
```

### Save and exit using CTRL + S, then ENTER, then CTRL + X.

## Step 3: Create Dockerfile for Backend
nano Dockerfile


### Paste the following code:
```
FROM maven:3.8.3-openjdk-17

WORKDIR /opt
COPY . /opt

RUN rm -rf src/main/resources/application.properties
RUN cp -rf application.properties src/main/resources/
RUN mvn clean package -DskipTests

WORKDIR /opt/target
EXPOSE 8080

CMD ["java", "-jar", "student-registration-backend-0.0.1-SNAPSHOT.jar"]
```

## Step 4: Build Backend Docker Image
```
docker build -t backend:v1 .
```

## Step 5: Run Backend Docker Container
```
docker run -d -p 8080:8080 backend:v1
```

## Step 6: Check Running Containers
```
docker ps
```
---

# Frontend Setup
## Step 1: Navigate to Frontend Directory
```
cd <GitHub_Repository_Name>/frontend
```
## Step 2: Create Dockerfile for Frontend
```
nano Dockerfile
```

### Paste the following code:
```
FROM node:25-alpine3.21

WORKDIR /opt
COPY . /opt

RUN apk update && apk add apache2
RUN npm install
RUN npm run build
RUN cp -rf dist/* /var/www/localhost/htdocs/

EXPOSE 80
CMD ["httpd", "-D", "FOREGROUND"]
```

## Step 3: Build Frontend Docker Image
```
docker build -t frontend:v1 .
```

## Step 4: Run Frontend Docker Container
```
docker run -d -p 80:80 frontend:v1
```

## Step 5: Check Running Containers
```
docker ps
```
---

## Verification and Testing

#### Access website:
Open your browser and go to
```
http://<EC2-Public-IP>
```
---
