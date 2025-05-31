# Docker Project

This project demonstrates containerization of a full-stack application using Docker and Docker Compose, with a focus on security and best practices.

## Architecture

The application consists of two main components:
- Frontend: Vue.js application served by Nginx
- Backend: Go application

## Security Features

- Non-root users in all containers
- Read-only filesystem where possible
- Resource limits and reservations
- Health checks for all services
- Docker Secrets for sensitive data
- Regular security scanning with Trivy
- Security headers in Nginx
- Network isolation
- Minimal base images

## Prerequisites

- Docker Engine 20.10.0+
- Docker Compose 2.0.0+

## Setup

1. Create secrets directory and environment files:
```bash
mkdir -p secrets
touch secrets/frontend.env secrets/backend.env
```

2. Add necessary environment variables to the .env files

3. Build and start the services:
```bash
docker-compose up --build
```

## Image Sizes

- Frontend: ~20MB (nginx:alpine base)
- Backend: ~15MB (alpine base)

## Development

For development, you can use the following commands:

```bash
# Build images
docker-compose build

# Start services
docker-compose up

# View logs
docker-compose logs -f

# Stop services
docker-compose down
```

## Security Scanning

The project includes Trivy vulnerability scanning in the CI/CD pipeline. To run it locally:

```bash
# Install Trivy
brew install aquasecurity/trivy/trivy

# Scan images
trivy image frontend:latest
trivy image backend:latest
```

## Resource Limits

- Frontend:
  - CPU: 0.5 cores limit, 0.25 cores reservation
  - Memory: 256MB limit, 128MB reservation

- Backend:
  - CPU: 0.5 cores limit, 0.25 cores reservation
  - Memory: 512MB limit, 256MB reservation

## Health Checks

Both services include health checks that run every 30 seconds:
- Frontend: Checks HTTP availability on port 80
- Backend: Checks health endpoint on port 8080

## Volumes

- `backend_data`: Persistent storage for backend data
- Temporary filesystems for logs and cache

## Networks

- `app-network`: Isolated bridge network for service communication
