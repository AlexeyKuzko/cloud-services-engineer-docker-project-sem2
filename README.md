# Docker Project

This project demonstrates containerization of a full-stack application using Docker and Docker Compose, with a focus on security and best practices. The application is automatically built and deployed using GitHub Actions.

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

- GitHub repository with access to GitHub Actions
- Docker Hub account for storing images
- GitHub repository secrets configured:
  - `DOCKER_USER`: Docker Hub username
  - `DOCKER_PASSWORD`: Docker Hub access token

## Deployment

The application is automatically deployed using GitHub Actions workflow. The workflow:
1. Runs security scanning with Trivy
2. Builds Docker images for frontend and backend
3. Pushes images to Docker Hub
4. Deploys the application using Docker Compose

### GitHub Actions Workflow

The workflow is triggered on push to the main branch and includes:
- Security scanning of Docker images
- Building and pushing images to Docker Hub
- Deployment using Docker Compose

## Image Sizes

- Frontend: ~20MB (nginx:alpine base)
- Backend: ~15MB (alpine base)

## Security Scanning

The project includes automated vulnerability scanning in the CI/CD pipeline using Trivy. The scanning:
- Runs automatically on each push to main branch
- Checks for critical and high severity vulnerabilities
- Fails the build if vulnerabilities are found
- Generates a detailed report of any issues

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

## Monitoring

The deployment status and logs can be monitored through:
1. GitHub Actions tab in your repository
2. Docker Hub repository for image builds
3. Application health endpoints

## Troubleshooting

If the deployment fails:
1. Check GitHub Actions logs for detailed error messages
2. Verify Docker Hub credentials in repository secrets
3. Ensure all required environment variables are set
4. Check application logs for runtime issues
