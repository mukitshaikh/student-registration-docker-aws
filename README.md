# Student Registration Application — Docker on AWS EC2

![Docker on AWS EC2 Architecture](docs/images/docker-aws-architecture.png)

## 📌 Project Overview

The Student Registration Application is a full-stack web application developed using React.js, Spring Boot, and MariaDB.

The application was containerized using Docker and deployed on an AWS EC2 Ubuntu server. Each component runs inside its own Docker container, making the application modular, portable, and easier to manage.

This project demonstrates practical experience with Docker, AWS EC2, Linux, containerization, backend API deployment, database connectivity, and GitHub version control using SSH.

## 🛠️ Technology Stack

| Technology | Purpose |
|---|---|
| React.js | Frontend development |
| Vite | Frontend build tool |
| Java | Backend programming |
| Spring Boot | REST API development |
| MariaDB | Relational database |
| Docker | Application containerization |
| Docker Images | Packaging application components |
| Docker Containers | Running application services |
| AWS EC2 | Cloud deployment |
| Ubuntu Linux | EC2 operating system |
| Git | Version control |
| GitHub | Source code repository |
| SSH / ED25519 | Secure GitHub authentication |

## 🏗️ Application Architecture

The application consists of three Docker containers deployed on an AWS EC2 Ubuntu server.

```text
                       USERS
                         |
                         v
                    INTERNET
                         |
                         v
                AWS EC2 - UBUNTU
                Public IP: EC2 IP
                         |
                 Docker Containers
                         |
         +---------------+---------------+
         |               |               |
         v               v               v
   FRONTEND           BACKEND          DATABASE
   React + Vite     Spring Boot        MariaDB
   Port: 80         Port: 8080         Port: 3306
         |               |               |
         +-------------->+-------------->+
             API Requests   DB Connection
```

### Docker Containers

| Container Name | Image Name | Port |
|---|---|---|
| student-frontend | student-frontend:latest | 80 |
| student-backend | student-backend:latest | 8080 |
| student-db | student-db:latest | 3306 |

## ✨ Project Features

- Student registration functionality
- View registered students
- Full-stack React and Spring Boot integration
- REST API implementation
- MariaDB database integration
- Dockerized frontend, backend, and database
- Custom Docker image creation
- AWS EC2 cloud deployment
- GitHub source code management
- SSH-based GitHub authentication

## 📂 Project Structure

```text
student-registration-aws/
│
├── frontend/
│   ├── src/
│   ├── public/
│   ├── Dockerfile
│   ├── package.json
│   └── .env
│
├── backend/
│   ├── src/
│   │   └── main/
│   │       └── resources/
│   │           └── application.properties
│   └── Dockerfile
│
├── database/
│   └── Dockerfile
│
├── docs/
│   └── images/
│       └── docker-aws-architecture.png
│
└── README.md
```

## ☁️ AWS EC2 Deployment

### Step 1: Launch EC2 Instance

1. Open the AWS Management Console.
2. Navigate to EC2.
3. Launch an Ubuntu instance.
4. Select the required instance type.
5. Configure the key pair.
6. Configure the Security Group.
7. Launch the instance.

### Step 2: Connect to EC2

Connect to the EC2 instance:

```bash
ssh -i your-key.pem ubuntu@YOUR_EC2_PUBLIC_IP
```

Update the system:

```bash
sudo apt update
sudo apt upgrade -y
```

### Step 3: Install Docker

Install Docker and Git:

```bash
sudo apt update
sudo apt install -y docker.io git
```

Start Docker:

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

Verify Docker installation:

```bash
docker --version
docker ps
```

## 🐳 Docker Containerization

### Step 1: Clone the Project

```bash
git clone https://github.com/mukitshaikh/student-registration-docker-aws.git
```

Navigate into the project directory:

```bash
cd student-registration-aws
```

### Step 2: Configure Frontend API URL

The frontend uses the backend API URL configured through Vite environment variables.

Example:

```env
VITE_API_URL=http://YOUR_EC2_PUBLIC_IP:8080/api
```

Replace `YOUR_EC2_PUBLIC_IP` with the public IP address of the EC2 instance.

### Step 3: Build Docker Images

Build the frontend image:

```bash
docker build -t student-frontend:latest ./frontend
```

Build the backend image:

```bash
docker build -t student-backend:latest ./backend
```

Build the database image:

```bash
docker build -t student-db:latest ./database
```

Verify the images:

```bash
docker images
```

### Step 4: Run Docker Containers

Start the frontend container:

```bash
docker run -d \
  --name student-frontend \
  -p 80:80 \
  student-frontend:latest
```

Start the backend container:

```bash
docker run -d \
  --name student-backend \
  -p 8080:8080 \
  student-backend:latest
```

Start the database container:

```bash
docker run -d \
  --name student-db \
  -p 3306:3306 \
  student-db:latest
```

> Note: These commands illustrate the container startup configuration. Database credentials, environment variables, and Docker networking must match the actual application configuration.

## 🌐 Application Access

Frontend:

```text
http://YOUR_EC2_PUBLIC_IP
```

Backend API:

```text
http://YOUR_EC2_PUBLIC_IP:8080/api/users
```

Backend Health Check:

```text
http://YOUR_EC2_PUBLIC_IP:8080/api/health
```

## 🧪 Testing and Verification

### Check Running Containers

```bash
docker ps
```

### Check All Containers

```bash
docker ps -a
```

### Test Backend API

```bash
curl http://localhost:8080/api/users
```

### Test Health Endpoint

```bash
curl http://localhost:8080/api/health
```

### Check Container Logs

```bash
docker logs student-frontend
docker logs student-backend
docker logs student-db
```

### View Docker Images

```bash
docker images
```

## 🔄 Deployment Workflow

```text
Developer
   |
   v
Application Source Code
   |
   v
GitHub Repository
   |
   v
AWS EC2 Ubuntu Server
   |
   v
Build Docker Images
   |
   v
Run Docker Containers
   |
   +---- React Frontend
   |
   +---- Spring Boot Backend
   |
   +---- MariaDB Database
   |
   v
Test Application
   |
   v
Access Application Through EC2 Public IP
```

## 🔐 GitHub SSH Configuration

SSH authentication was configured on the EC2 server to securely push project code to GitHub.

### Generate SSH Key

```bash
ssh-keygen -t ed25519 -C "mukitshaikh@github.com"
```

### Display Public Key

```bash
cat ~/.ssh/id_ed25519.pub
```

### Test SSH Authentication

```bash
ssh -T git@github.com
```

Successful output:

```text
Hi mukitshaikh! You've successfully authenticated, but GitHub does not provide shell access.
```

### Configure Git Remote

```bash
git remote set-url origin git@github.com:mukitshaikh/student-registration-docker-aws.git
```

Verify:

```bash
git remote -v
```

### Push Changes to GitHub

```bash
git add .
git commit -m "Update project"
git push origin main
```

## 🔧 Docker Management Commands

Stop containers:

```bash
docker stop student-frontend student-backend student-db
```

Start containers:

```bash
docker start student-frontend student-backend student-db
```

Remove containers:

```bash
docker rm -f student-frontend student-backend student-db
```

Remove an image:

```bash
docker rmi student-frontend:latest
```

Inspect a container:

```bash
docker inspect student-backend
```

## 🔒 Security Best Practices

- Never expose database port 3306 publicly.
- Restrict SSH access to trusted IP addresses.
- Avoid exposing backend port 8080 directly in production.
- Never commit database passwords or private keys to GitHub.
- Store sensitive credentials using environment variables or AWS Secrets Manager.
- Use HTTPS for production deployments.
- Keep Docker images and the operating system updated.

## 📚 What I Learned

- Docker installation and configuration on Ubuntu Linux.
- Building custom Docker images.
- Running multiple containers for a full-stack application.
- Containerizing React, Spring Boot, and MariaDB.
- Deploying applications on AWS EC2.
- Configuring frontend-to-backend API communication.
- Database connectivity and application troubleshooting.
- Linux server administration.
- Git and GitHub version control.
- SSH key-based authentication.
- Cloud application deployment.

## 👨‍💻 Author

**Mukit Shaikh**

Cloud & DevOps Learner

GitHub: [mukitshaikh](https://github.com/mukitshaikh)

## 🔗 Project Repository

[student-registration-docker-aws](https://github.com/mukitshaikh/student-registration-docker-aws)
