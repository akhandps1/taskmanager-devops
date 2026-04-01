# 🚀 Task Manager DevOps Project

A full-stack task management application built with the MERN stack (React + Node.js) featuring a complete DevOps lifecycle. This project demonstrates containerization with Docker, automated CI/CD pipelines using GitHub Actions, and seamless deployment to AWS EC2.

---

## 🌐 Live Demo

**Check out the live application here:** 👉 **[Click here to view the Live App](http://<YOUR_EC2_PUBLIC_IP>:5173)** *(Note: Ensure your AWS EC2 instance is running and inbound rules allow traffic on port 5173).*

---

## 📸 Screenshots

![Task Manager Dashboard](./screenshots/app-screenshot.png)

---

## 🌟 Features

- **Full-Stack Application:** Frontend built with React (Vite) and Tailwind CSS; Backend powered by Node.js, Express, and MongoDB.
- **Containerized:** Both client and server are fully Dockerized using custom `Dockerfile`s and orchestrated via `docker-compose`.
- **Automated CI/CD:** GitHub Actions pipeline configured for automatic linting, testing, and deployment.
- **Cloud Deployment:** Automated Zero-Downtime deployment to AWS EC2 instances.
- **Code Quality:** Configured with ESLint and Prettier for consistent coding standards.

---

## 🛠️ Tech Stack

- **Frontend:** React 19, Vite, TailwindCSS
- **Backend:** Node.js, Express.js, MongoDB (Mongoose)
- **DevOps:** Docker, Docker Compose, GitHub Actions, AWS EC2, SSH/SCP Deployment

---

## 📂 Project Structure

```text
taskmanager-devops/
├── client/                 # React Frontend (Vite)
│   ├── src/                # React components, services, and assets
│   ├── Dockerfile          # Frontend Docker configuration
│   ├── package.json        
│   └── vite.config.js      # Vite build and proxy config
├── server/                 # Node.js Backend API
│   ├── controllers/        # Route controllers
│   ├── models/             # Mongoose DB schemas
│   ├── routes/             # Express API routes
│   ├── Dockerfile          # Backend Docker configuration
│   ├── server.js           # Entry point
│   └── package.json        
├── .github/workflows/      # CI/CD Pipeline Configuration
│   └── deploy.yml          # GitHub Actions deployment script
├── docker-compose.yml      # Multi-container orchestration
└── README.md               # Project documentation
```

## ⚙️ Environment Variables

Create `.env` files in both the `server` and `client` directories (if running locally without Docker).

**`server/.env`**

Code snippet

```
PORT=5000
MONGO_URI=your_mongodb_connection_string
```

_Note: For the frontend Docker build, the `VITE_API_URL` is passed dynamically as a build argument in the `docker-compose.yml`._

---

## 💻 Local Development Setup (Without Docker)

## 1. Backend Setup

Bash

```
cd server
npm install
npm run dev
```

_The server will start on http://localhost:5000_

## 2. Frontend Setup

Open a new terminal tab:

Bash

```
cd client
npm install
npm run dev
```

_The client will start on http://localhost:5173_

---

## 🐳 Docker Setup (Local)

To run the entire application locally using Docker:

1. Ensure Docker and Docker Compose are installed on your machine.
    
2. Run the following command from the root directory:
    

Bash

```
docker-compose up --build
```

- **Frontend:** Accessible at `http://localhost:5173`
    
- **Backend API:** Accessible at `http://localhost:5000`
    

To stop the containers:

Bash

```
docker-compose down
```

---

## 🚀 CI/CD & AWS Deployment Setup

This project uses **GitHub Actions** to automatically deploy to an **AWS EC2 (Ubuntu)** instance whenever code is pushed to the `main` branch.

## Prerequisites on AWS EC2

1. Launch an Ubuntu EC2 instance.
    
2. Open inbound ports in the Security Group: **22 (SSH), 80 (HTTP), 5000 (Backend), 5173 (Frontend)**.
    
3. Install Docker and Docker Compose on the EC2 instance.
    
4. Manually create the `server/.env` file on the EC2 instance to store your MongoDB URI securely.
    

## GitHub Repository Secrets

To enable the automated deployment, add the following secrets to your GitHub Repository (**Settings > Secrets and variables > Actions**):

|**Secret Name**|**Description**|
|---|---|
|`EC2_HOST`|The Public IPv4 address of your AWS EC2 instance|
|`EC2_USER`|The SSH username (default is `ubuntu`)|
|`EC2_KEY`|The entire content of your `.pem` SSH private key|

## Deployment Workflow

1. Developer pushes code to the `main` branch.
    
2. GitHub Actions triggers the `build-and-deploy` workflow.
    
3. Code is linted. If successful, fresh code is securely copied to the EC2 instance via SCP.
    
4. SSH executes `docker-compose down`, rebuilds the images with `docker-compose up -d --build`, and prunes old images.
    
---
