# Student Registration Application — Docker & AWS EC2 Deployment

## 📌 Project Overview

This project is a full-stack Student Registration Application built using React, Spring Boot, and MariaDB. The application is containerized using Docker and deployed on an AWS EC2 Ubuntu server.

The project demonstrates application containerization, Docker image creation, container networking, AWS EC2 deployment, and GitHub version control using SSH authentication.

## 🛠️ Technology Stack

| Technology | Purpose |
|---|---|
| React.js | Frontend user interface |
| Vite | Frontend build tool |
| Java | Backend programming language |
| Spring Boot | REST API development |
| MariaDB | Relational database |
| Docker | Application containerization |
| AWS EC2 | Cloud server hosting |
| Ubuntu | EC2 operating system |
| Git | Version control |
| GitHub | Source code repository |
| SSH / ED25519 | Secure GitHub authentication |

## 🏗️ Application Architecture

```text
                    User / Browser
                          |
                          v
                AWS EC2 Ubuntu Server
                   Public IP Address
                          |
                          v
                React Frontend (Docker)
                     Port 80
                          |
                          v
              Spring Boot Backend (Docker)
                     Port 8080
                          |
                          v
                 MariaDB (Docker)
                     Port 3306
```

### Docker Containers

| Container Name | Docker Image | Port |
|---|---|---|
| student-frontend | student-frontend:latest | 80 |
| student-backend | student-backend:latest | 8080 |
| student-db | student-db:latest | 3306 |

Each application layer runs in its own Docker container.

## 📂 Project Structure

```text
student-registration-aws/
│
├── frontend/
│   ├── src/
│   ├── public/
│   ├── Dockerfile
│   ├── .env
│   └── package.json
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
└── README.md
```

*Note: The structure above represents the main application and Docker components. Adjust paths if the repository structure changes.*

## ☁️ AWS EC2 Setup

### Step 1 — Launch an EC2 Instance

1. Open the AWS Management Console.
2. Navigate to EC2.
3. Launch an Ubuntu instance.
4. Configure the required instance type.
5. Create or select a key pair.
6. Configure the Security Group.

### Step 2 — Configure Security Group

The application uses the following ports:

| Port | Service | Purpose |
|---|---|---|
| 22 | SSH | Remote server administration |
| 80 | HTTP | Frontend access |
| 8080 | Spring Boot | Backend API |
| 3306 | MariaDB | Database |

For better security, restrict SSH access to your own IP address. Database port 3306 should not be publicly accessible. Ideally, backend port 8080 should also be private and accessed through the frontend or a reverse proxy.

### Step 3 — Connect to EC2

Connect to the EC2 instance using SSH:

```bash
ssh -i your-key.pem ubuntu@YOUR_EC2_PUBLIC_IP
```

Update the system:

```bash
sudo apt update
sudo apt upgrade -y
```

## 🐳 Docker Installation

Install Docker on Ubuntu:

```bash
sudo apt update
sudo apt install -y docker.io git
```

Enable and start Docker:

```bash
sudo systemctl enable docker
sudo systemctl start docker
```

Verify installation:

```bash
docker --version
docker ps
```

## 📥 Clone the GitHub Repository

Clone the project:

```bash
git clone https://github.com/mukitshaikh/student-registration-aws.git
```

Navigate into the project:

```bash
cd student-registration-aws
```

## 🐳 Docker Image Creation

Dockerfiles are used to containerize the frontend, backend, and database components.

### Step 1 — Build Frontend Image

Configure the frontend API URL in the frontend environment configuration.

Example:

```env
VITE_API_URL=http://YOUR_EC2_PUBLIC_IP:8080/api
```

Build the frontend image:

```bash
docker build -t student-frontend:latest ./frontend
```

### Step 2 — Build Backend Image

```bash
docker build -t student-backend:latest ./backend
```

### Step 3 — Build Database Image

```bash
docker build -t student-db:latest ./database
```

### Step 4 — Verify Docker Images

```bash
docker images
```

The following images should be available:

```text
student-frontend
student-backend
student-db
```

## 🚀 Running the Docker Containers

### Step 1 — Create a Docker Network

A Docker network allows containers to communicate with each other.

```bash
docker network create student-network
```

### Step 2 — Start the Database Container

```bash
docker run -d \
  --name student-db \
  --network student-network \
  -p 3306:3306 \
  student-db:latest
```

Check the database container:

```bash
docker ps
docker logs student-db
```

### Step 3 — Start the Backend Container

```bash
docker run -d \
  --name student-backend \
  --network student-network \
  -p 8080:8080 \
  student-backend:latest
```

Check backend logs:

```bash
docker logs student-backend
```

### Step 4 — Start the Frontend Container

```bash
docker run -d \
  --name student-frontend \
  --network student-network \
  -p 80:80 \
  student-frontend:latest
```

Check frontend logs:

```bash
docker logs student-frontend
```

*Important: The commands above are a deployment reference. The database credentials, backend database hostname, and startup options must match the Dockerfiles and `application.properties` used in this project.*

## 🌐 Application Access

The frontend is accessible through the EC2 public IP:

```text
http://YOUR_EC2_PUBLIC_IP
```

Backend API:

```text
http://YOUR_EC2_PUBLIC_IP:8080/api/users
```

Health endpoint:

```text
http://YOUR_EC2_PUBLIC_IP:8080/api/health
```

## 🧪 Application Testing

### Check Running Containers

```bash
docker ps
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

### Inspect Docker Network

```bash
docker network inspect student-network
```

## 🔧 Docker Management Commands

### Stop Containers

```bash
docker stop student-frontend student-backend student-db
```

### Start Containers

```bash
docker start student-db student-backend student-frontend
```

### Remove Containers

```bash
docker rm -f student-frontend student-backend student-db
```

### Rebuild Images

```bash
docker build -t student-frontend:latest ./frontend
docker build -t student-backend:latest ./backend
docker build -t student-db:latest ./database
```

### View Docker Images

```bash
docker images
```

### View All Containers

```bash
docker ps -a
```

## 🔐 GitHub Integration Using SSH

GitHub SSH authentication was configured on the EC2 server to push code without using a GitHub password or Personal Access Token.

### Step 1 — Generate an ED25519 SSH Key

```bash
ssh-keygen -t ed25519 -C "mukitshaikh@github.com"
```

The key pair was generated under:

```text
/root/.ssh/id_ed25519
/root/.ssh/id_ed25519.pub
```

- `id_ed25519` — Private key. Never share it.
- `id_ed25519.pub` — Public key. Add it to GitHub.

### Step 2 — Display the Public Key

```bash
cat ~/.ssh/id_ed25519.pub
```

### Step 3 — Add SSH Key to GitHub

1. Open https://github.com/settings/ssh/new
2. Enter the title: `EC2 Ubuntu Project`
3. Select `Authentication Key`.
4. Paste the public key.
5. Click `Add SSH key`.

### Step 4 — Test SSH Authentication

```bash
ssh -T git@github.com
```

Successful output:

```text
Hi mukitshaikh! You've successfully authenticated, but GitHub does not provide shell access.
```

### Step 5 — Configure Git Remote

```bash
git remote set-url origin git@github.com:mukitshaikh/student-registration-aws.git
```

Verify the remote:

```bash
git remote -v
```

Expected output:

```text
origin  git@github.com:mukitshaikh/student-registration-aws.git (fetch)
origin  git@github.com:mukitshaikh/student-registration-aws.git (push)
```

### Step 6 — Push Code to GitHub

```bash
git add .
git commit -m "Update project documentation"
git push origin main
```

The project code is maintained in the following repository:

https://github.com/mukitshaikh/student-registration-aws

## 🔄 Application Deployment Workflow

```text
Developer
   |
   v
Application Source Code
   |
   v
Git / GitHub Repository
   |
   v
AWS EC2 Ubuntu Server
   |
   v
Docker Image Build
   |
   v
Docker Containers
   |
   +---- Frontend (React / Vite)
   |
   +---- Backend (Spring Boot)
   |
   +---- Database (MariaDB)
   |
   v
Application Testing
   |
   v
Access Through EC2 Public IP
```

## 🔒 Security Best Practices

- Never commit database passwords or other secrets to GitHub.
- Do not share the SSH private key.
- Restrict SSH access to trusted IP addresses.
- Do not expose the MariaDB port publicly.
- Prefer using environment variables or AWS Secrets Manager for sensitive configuration.
- Use HTTPS for production deployments.
- Keep Docker images and the Ubuntu system updated.
- Use a dedicated database user rather than a root account for the application.

## 📚 What I Learned

Through this project, I gained practical experience in:

- Deploying a full-stack application on AWS EC2.
- Containerizing React, Spring Boot, and MariaDB applications.
- Building custom Docker images using Dockerfiles.
- Running and managing multiple Docker containers.
- Configuring Docker networking.
- Connecting frontend applications with backend REST APIs.
- Connecting Spring Boot with MariaDB.
- Troubleshooting application and container issues.
- Using Git and GitHub for version control.
- Configuring SSH authentication with GitHub.
- Deploying applications on a cloud-based Ubuntu server.

## 👨‍💻 Author

**Mukit Shaikh**

Cloud & DevOps Learner

GitHub: https://github.com/mukitshaikh

Repository: https://github.com/mukitshaikh/student-registration-aws
