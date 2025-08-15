# 🚀 Arxoft Dockerized EC2 

**Docker-Based Production Deployment Framework**

This repository serves as a **production-ready deployment framework** for any platform using **Docker Compose**. It is designed for deployment on cloud servers such as **Amazon EC2** and supports scalable orchestration of multiple services, including frontend, backend, and system components.

## 📦 Included Services

This monorepo integrates the following core services:

- **Redis**  
  In-memory cache used for backend coordination and job queuing. Multiple services can use it across the Docker Network.

- **Nginx**  
  Reverse proxy for routing domains HTTP(S) traffic to services, with optional TLS support.

All other services mentioned in `docker-compose.yml` are different applications' repos served on this framework.

## 🖥️ System Requirements

Before deploying to EC2 or any Linux server, ensure the following are installed:

- **Docker** ≥ 20.10  
  Install via: https://docs.docker.com/get-docker/

- **Docker Compose** ≥ 1.29  
  Install via: https://docs.docker.com/compose/install/

- **Git**
- **Bash Shell**  
  Required to run management scripts (`sh.restart`)

## 📂 Folder Structure Overview

```text
.
├── docker-compose.yml
├── sh.restart                       # Service management script
├── nginx/
│   ├── hosts.conf                   # Nginx reverse proxy configuration
│   └── certs/                       # TLS certificates
├── my-project
├── another-project
├── yet-another-project
├── ...
```

# ⚙️ Installation & Deployment

1. Clone the Monorepo

```
git clone https://github.com/your-org/commodity-price-tracker-monorepo.git
cd commodity-price-tracker-monorepo
```

2. Ensure Dockerfiles Are Production-Optimized

Each service’s `Dockerfile` must:

- Use multi-stage builds
- Exclude dev dependencies
- Set the correct `CMD` or `ENTRYPOINT`

3. Start All Services

```
./sh.restart all
```

This command builds and starts all services as defined in `docker-compose.yml`.


## 🔁 Service Management

To restart individual containers:

```
./sh.restart <service-name>
```

Examples:

```
./sh.restart fastapi-app
./sh.restart nextjs-app
```

To restart everything:

```
./sh.restart all
```

## 🧪 Local Development Note

This monorepo is intended for production deployment only.

For local development:

- Clone each service’s repo individually.
- Use per-project development setups (e.g., hot reload, bind mounts, .env files).
- Run with local docker-compose.yml or virtualenv.

## 🌐 Network & Security Notes

- Nginx handles HTTPS traffic termination via mounted TLS certs (`nginx/certs/`).
- Redis is only exposed internally and not publicly accessible.
- Consider adding a firewall, VPC rules, and environment variable secrets for production security.

### Generating self-signed SSL Certificate for a domain

```
openssl req -x509 -nodes -days 365 \
  -newkey rsa:2048 \
  -keyout nginx/certs/example.key \
  -out nginx/certs/example.crt \
  -subj "/CN=example.com"
```

## 🚀 Deploying on EC2 (Quick Start)

- Launch an Ubuntu EC2 instance
- SSH into the server
- Install Docker and Docker Compose
- Clone this repo and run:

```
sudo ./sh.restart all
```

You’ll have your full stack running and exposed via Nginx on ports 80/443.

## 📌 Future Improvements

- CI/CD pipeline for auto-deploy
- Monitoring & alerting integration
- Docker image registry support (ECR/Docker Hub)
- Horizontal scaling via Docker Swarm or Kubernetes
