Microservices Docker Containerization Project
Project Overview
This project demonstrates containerization of a microservices-based application using Docker and Docker Compose. The application consists of three Node.js microservices orchestrated to work together in a distributed architecture.

Architecture Components
User Service (Port 3000): Handles user management operations
Product Service (Port 3001): Manages product catalog and inventory
Gateway Service (Port 3003): API gateway for routing and aggregation
Prerequisites
Required Software
Docker Engine 20.10+
Docker Compose 2.0+
Git
Node.js 18+ (for local development)
System Requirements
Memory: Minimum 2GB RAM available for Docker
Storage: 2GB free disk space
Network: Internet connection for pulling base images
Verification Commands
bash
# Verify Docker installation
docker --version
docker-compose --version

# Check Docker daemon status
docker info
Project Structure
microservices-task/
├── user-service/
│   ├── Dockerfile
│   ├── package.json
│   └── server.js
├── product-service/
│   ├── Dockerfile
│   ├── package.json
│   └── server.js
├── gateway-service/
│   ├── Dockerfile
│   ├── package.json
│   └── server.js
├── docker-compose.yml
└── README.md
Setup Instructions
1. Clone Repository
bash
git clone https://github.com/mohanDevOps-arch/Microservices-Task.git
cd Microservices-Task
2. Build and Start Services
bash
# Build all services and start in detached mode
docker-compose up -d --build

# View running containers
docker-compose ps

# View logs from all services
docker-compose logs -f
3. Verify Service Health
bash
# Check health status
docker-compose ps

# Individual service connectivity checks
curl http://localhost:3000/  # User Service
curl http://localhost:3001/  # Product Service
curl http://localhost:3003/  # Gateway Service

# Or use wget for testing
wget --spider http://localhost:3000/
wget --spider http://localhost:3001/
wget --spider http://localhost:3003/
Testing Each Service
User Service (Port 3000)
bash
# Basic connectivity check
curl -X GET http://localhost:3000/

# Test available endpoints (check service documentation for actual endpoints)
# Examples based on typical user service endpoints:
curl -X GET http://localhost:3000/users
curl -X GET http://localhost:3000/api/users
Product Service (Port 3001)
bash
# Basic connectivity check
curl -X GET http://localhost:3001/

# Test available endpoints (check service documentation for actual endpoints)
# Examples based on typical product service endpoints:
curl -X GET http://localhost:3001/products
curl -X GET http://localhost:3001/api/products
Gateway Service (Port 3003)
bash
# Basic connectivity check
curl -X GET http://localhost:3003/

# Test gateway routing (check service documentation for actual endpoints)
# Examples based on typical gateway patterns:
curl -X GET http://localhost:3003/api/users
curl -X GET http://localhost:3003/api/products
Load Testing (Optional)
bash
# Install apache bench for load testing
# Ubuntu/Debian: sudo apt install apache2-utils
# macOS: brew install apache2-utils

# Test gateway under load (use root endpoint)
ab -n 1000 -c 10 http://localhost:3003/
Service Communication
Services communicate through the internal Docker network microservices-network. The gateway service uses these internal URLs:

User Service: http://user-service:3000
Product Service: http://product-service:3001
Management Commands
Development Operations
bash
# View real-time logs
docker-compose logs -f [service-name]

# Restart specific service
docker-compose restart user-service

# Rebuild specific service
docker-compose up -d --build user-service

# Scale services (if stateless)
docker-compose up -d --scale user-service=2

# Execute commands in running container
docker-compose exec user-service sh
Production Operations
bash
# Start services with production settings
docker-compose -f docker-compose.yml up -d

# Monitor resource usage
docker stats

# Create backup of volumes
docker run --rm -v microservices_user-data:/data -v $(pwd):/backup alpine tar czf /backup/user-data-backup.tar.gz -C /data .
Troubleshooting Guide
Common Issues
Services Not Starting
bash
# Check container logs
docker-compose logs [service-name]

# Verify Docker resources
docker system df
docker system prune  # Clean up if needed

# Check port conflicts
netstat -tulpn | grep :3000
Network Connectivity Issues
bash
# Inspect network configuration
docker network ls
docker network inspect microservices-network

# Test service-to-service connectivity
docker-compose exec gateway-service ping user-service
docker-compose exec gateway-service nslookup user-service
Health Check Failures
bash
# Manual connectivity check
docker-compose exec user-service curl localhost:3000/

# Check if service is running and listening on correct port
docker-compose exec user-service ps aux
docker-compose exec user-service netstat -tulpn
Resource Constraints
bash
# Monitor resource usage
docker stats --no-stream

# Adjust resource limits in docker-compose.yml
# Increase memory limits if containers are being killed
Performance Tuning
Memory Optimization
Adjust deploy.resources.limits in docker-compose.yml
Monitor memory usage with docker stats
Consider using Alpine-based images for smaller footprint
Network Optimization
Use internal service names for inter-service communication
Implement connection pooling in applications
Consider using traefik or nginx for load balancing
Production Considerations
Security Hardening
Services run as non-root users (nodeuser:1001)
Regular security updates in Alpine base images
Network isolation with custom bridge network
Consider implementing service mesh for advanced security
Monitoring & Observability
bash
# Add monitoring stack
# Prometheus metrics endpoint: /metrics
# Health checks: /health
# Consider adding ELK stack or Grafana

# Log aggregation
docker-compose logs --since="1h" > app-logs.txt
Scaling Strategies
Use Docker Swarm or Kubernetes for production
Implement horizontal pod autoscaling
Database connection pooling and caching
CDN for static assets
Backup & Recovery
bash
# Database backup (if applicable)
docker-compose exec database-service mysqldump -u root -p database_name > backup.sql

# Volume backup
docker run --rm -v microservices_user-data:/data -v $(pwd):/backup alpine tar czf /backup/backup.tar.gz -C /data .
Environment Variables
Available Configuration
NODE_ENV: Application environment (development/production)
SERVICE_NAME: Service identifier for logging
SERVICE_PORT: Port configuration
USER_SERVICE_URL: Gateway → User service URL
PRODUCT_SERVICE_URL: Gateway → Product service URL
Custom Configuration
bash
# Create .env file for environment-specific settings
echo "NODE_ENV=development" > .env
echo "DEBUG=true" >> .env

# Use with docker-compose
docker-compose --env-file .env up -d
Screenshots Reference
When documenting your deployment, include screenshots of:

Terminal showing successful build:
bash
   docker-compose up -d --build
Docker Desktop showing running containers:
All three services in "Running" state
Port mappings visible
Service connectivity checks:
bash
   curl localhost:3000/
   curl localhost:3001/  
   curl localhost:3003/
Docker Compose PS output:
bash
   docker-compose ps
Contributing
Development Workflow
Create feature branch
Make changes to service code
Test locally with Docker Compose
Update documentation if needed
Submit pull request
Code Quality
Follow Node.js best practices
Implement proper error handling
Add comprehensive logging
Write unit tests for business logic
Support & Resources
Official Documentation
Docker Documentation
Docker Compose Reference
Node.js Docker Best Practices
Performance Monitoring
Docker Stats API
Node.js Performance Monitoring
Project Status: Production Ready
Last Updated: September 2025
Docker Version: 20.10+
Node.js Version: 18+

