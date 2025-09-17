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

C:\Users\Pranvi Kankane\OneDrive\Desktop\Devops\Docker-dockercomposed-Assignment\Microservervice-Checkout\Microservices-Task-170925\Microservices>docker version
Client:
 Version:           28.3.2
 API version:       1.51
 Go version:        go1.24.5
 Git commit:        578ccf6
 Built:             Wed Jul  9 16:12:31 2025
 OS/Arch:           windows/amd64
 Context:           desktop-linux

Server: Docker Desktop 4.43.2 (199162)
 Engine:
  Version:          28.3.2
  API version:      1.51 (minimum version 1.24)
  Go version:       go1.24.5
  Git commit:       e77ff99
  Built:            Wed Jul  9 16:13:55 2025
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.7.27
  GitCommit:        05044ec0a9a75232cad458027ca83437aae3f4da
 runc:
  Version:          1.2.5
  GitCommit:        v1.2.5-0-g59923ef
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0

docker-compose --version

C:\Users\Pranvi Kankane\OneDrive\Desktop\Devops\Docker-dockercomposed-Assignment\Microservervice-Checkout\Microservices-Task-170925\Microservices>docker-compose --version
Docker Compose version v2.38.2-desktop.1


# Check Docker daemon status
docker info
Project Structure
microservices-task/
├── user-service/
│   ├── Dockerfile
│   ├── package.json
│   └── app.js
├── product-service/
│   ├── Dockerfile
│   ├── package.json
│   └── app.js
├── gateway-service/
│   ├── Dockerfile
│   ├── package.json
│   └── app.js
├── docker-compose.yml
└── README.md
https://github.com/vijaykankane/Microservices-Task-170925


Setup Instructions
1. Clone Repository
bash
git clone https://github.com/vijaykankane/Microservices-Task-170925.git
cd Microservices-Task
2. Build and Start Services
bash
# Build all services and start in detached mode
docker-compose up -d --build

[+] Running 6/6
 ✔ user-service                             Built                                                                                     0.0s 
 ✔ gateway-service                          Built                                                                                     0.0s 
 ✔ product-service                          Built                                                                                     0.0s 
 ✔ Container microservices-user-service     Healthy                                                                                   7.2s 
 ✔ Container microservices-product-service  Healthy                                                                                   7.2s 
 ✔ Container microservices-gateway-service  Started                                                                                   6.3s

# View running containers
docker-compose ps


C:\Users\Pranvi Kankane\OneDrive\Desktop\Devops\Docker-dockercomposed-Assignment\Microservervice-Checkout\Microservices-Task-170925\Microservices>docker compose ps
time="2025-09-17T22:15:52+05:30" level=warning msg="C:\\Users\\Pranvi Kankane\\OneDrive\\Desktop\\Devops\\Docker-dockercomposed-Assignment\\Microservervice-Checkout\\Microservices-Task-170925\\Microservices\\docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion"
NAME                            IMAGE                           COMMAND                  SERVICE           CREATED       STATUS                 PORTS
microservices-gateway-service   microservices-gateway-service   "dumb-init -- node a…"   gateway-service   3 hours ago   Up 3 hours (healthy)   0.0.0.0:3003->3003/tcp, [::]:3003->3003/tcp
microservices-product-service   microservices-product-service   "dumb-init -- node a…"   product-service   3 hours ago   Up 3 hours (healthy)   0.0.0.0:3001->3001/tcp, [::]:3001->3001/tcp
microservices-user-service      microservices-user-service      "dumb-init -- node a…"   user-service      3 hours ago   Up 3 hours (healthy)   0.0.0.0:3000->3000/tcp, [::]:3000->3000/tcp


docker-compose logs -f
3. Verify Service Health
# Check health status

C:\>curl http://localhost:3000/health
{"status":"User Service is healthy"}
C:\>curl http://localhost:3001/health
{"status":"Product Service is healthy"}
C:\>curl http://localhost:3003/health
{"status":"Gateway Service is healthy"}
C:\>
bash




C:\>curl -X GET http://localhost:3000/users
[{"id":1,"name":"John Doe"},{"id":2,"name":"Jane Smith"}]
C:\>


C:\>curl -X GET http://localhost:3003/api/users
[{"id":1,"name":"John Doe"},{"id":2,"name":"Jane Smith"}]
C:\>

Product Service (Port 3001)
bash


# Test available endpoints (check service documentation for actual endpoints)
# Examples based on typical product service endpoints:

C:\>curl -X GET http://localhost:3001/products
[{"id":1,"name":"Laptop","price":999},{"id":2,"name":"Phone","price":699}]
C:\>

C:\>curl -X GET http://localhost:3003/api/products
[{"id":1,"name":"Laptop","price":999},{"id":2,"name":"Phone","price":699}]
C:\>

Gateway Service (Port 3003)
bash

# Test gateway routing (check service documentation for actual endpoints)
# Examples based on typical gateway patterns:

C:\>curl -X GET http://localhost:3003/api/users
[{"id":1,"name":"John Doe"},{"id":2,"name":"Jane Smith"}]


C:\>curl -X GET http://localhost:3003/api/products
[{"id":1,"name":"Laptop","price":999},{"id":2,"name":"Phone","price":699}]
C:\>


For Service Communication
Services communicate through the internal Docker network microservices-network. The gateway service uses these internal URLs:

User Service: http://user-service:3000
Product Service: http://product-service:3001

# Inspect network configuration

C:\>docker network ls
NETWORK ID     NAME                    DRIVER    SCOPE
33460339a37c   bridge                  bridge    local
53b7b87cb328   host                    host      local
9af0d8522c1d   microservices-network   bridge    local
fa7d77a44c69   none                    null      local
d17aab256688   vkdon2                  bridge    local


C:\>docker network inspect microservices-network
[
    {
        "Name": "microservices-network",
        "Id": "9af0d8522c1d5f7a27c4f7d25c511c96232ee4ff475ff11b75bb88a7c3f6ac36",
        "Created": "2025-09-17T12:54:23.467837935Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv4": true,
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.20.0.0/16"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "332eae68ce561d0d53574efae6b0c94e7e14e9b652cd4bfef351e1d4f7ed2871": {
                "Name": "microservices-user-service",
                "EndpointID": "49d08e693e0799ec06357948f823a3146c1d0007376be025c975e2197b89dbe0",
                "MacAddress": "96:20:e3:40:8d:10",
                "IPv4Address": "172.20.0.2/16",
                "IPv6Address": ""
            },
            "e0e61e72fcb2e89f39f79c269675d81e839246aef4d1111b0816e555f848a543": {
                "Name": "microservices-gateway-service",
                "EndpointID": "291c8b4393d13f748b0ca4ec43eb2c3d3854fc4cb881799e696142be36efc5ac",
                "MacAddress": "26:4d:f1:00:a5:2d",
                "IPv4Address": "172.20.0.4/16",
                "IPv6Address": ""
            },
            "fddca603808382ffafe1cd38736d626b2c149e157718d45d97ddae599f10a53f": {
                "Name": "microservices-product-service",
                "EndpointID": "42b78fd5fd3959dde0e0163fcd52afb7fc5e60ed12723f8e1799ba21b5b48978",
                "MacAddress": "9a:a7:3a:5a:09:ea",
                "IPv4Address": "172.20.0.3/16",
                "IPv6Address": ""
            }
        },
        "Options": {
            "com.docker.network.enable_ipv4": "true",
            "com.docker.network.enable_ipv6": "false"
        },
        "Labels": {
            "com.docker.compose.config-hash": "e9c13263cc37dfce1a2f2ef93f401a5eb261bec86ac4639e8253ae63cb295257",
            "com.docker.compose.network": "microservices-network",
            "com.docker.compose.project": "microservices",
            "com.docker.compose.version": "2.38.2"
        }
    }
]
