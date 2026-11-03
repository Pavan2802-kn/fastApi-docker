# Docker Health Monitoring

# FastAPI Application – Docker Deployment

## Project Overview

This project demonstrates containerization of a Python FastAPI application using Docker. It uses Python 3.12 Slim as the base image, installs application dependencies, configures a Docker health check, and runs the application using Uvicorn.

## Tech Stack

- Python 3.12
- FastAPI
- Uvicorn (ASGI Server)
- Docker
- Linux (Debian Slim)

## Project Structure

```text
fastapi-project/
│
├── Dockerfile
├── .dockerignore
├── requirements.txt
└── main.py
```

## Dockerfile Explanation

| Instruction | Description |
|---|---|
| FROM | Uses Python 3.12 Slim base image |
| WORKDIR | Sets `/app` as working directory |
| COPY | Copies application dependencies and source code |
| RUN | Installs Python packages and curl |
| EXPOSE | Documents application port 8000 |
| HEALTHCHECK | Monitors application availability |
| CMD | Starts FastAPI using Uvicorn |

## Application Setup

### requirements.txt

```text
fastapi
uvicorn
```

### main.py

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def home():
    return {"message": "Docker application is running"}
```

## Docker Commands

### 1. Build Docker Image

```bash
docker build -t myapp:v3 .
```

### 2. List Docker Images

```bash
docker images
```

### 3. Run Container

```bash
docker run -d \
  -p 8000:8000 \
  --name mycontainer3 \
  myapp:v3
```

### 4. Check Running Containers

```bash
docker ps
```

### 5. View Container Logs

```bash
docker logs mycontainer3
```

### 6. Access Application

Open in browser:

```text
http://localhost:8000/
```

FastAPI Swagger documentation:

```text
http://localhost:8000/docs
```

## Docker Health Check

The Dockerfile implements a health check using curl.

```dockerfile
HEALTHCHECK --interval=30s \
            --timeout=5s \
            --start-period=10s \
            --retries=3 \
            CMD curl -f http://localhost:8000/ || exit 1
```

### Health Check Parameters

| Parameter | Description |
|---|---|
| interval | Runs health check every 30 seconds |
| timeout | Maximum execution time of 5 seconds |
| start-period | Initial 10-second startup grace period |
| retries | Three consecutive failures mark container unhealthy |

### Check Container Health

```bash
docker ps
```

Expected status:

```text
Up 1 minute (healthy)
```

Detailed health status:

```bash
docker inspect --format='{{json .State.Health}}' mycontainer3
```

### Check Application Health Inside Container

Enter the container:

```bash
docker exec -it mycontainer3 /bin/sh
```

Check curl:

```bash
curl --version
```

Test application:

```bash
curl -f http://localhost:8000/
```

Expected response:

```json
{"message": "Docker application is running"}
```

Check HTTP status:

```bash
curl -o /dev/null -s -w "%{http_code}\n" http://localhost:8000/
```

Expected output:

```text
200
```

## Docker Container Management

Stop container:

```bash
docker stop mycontainer3
```

Start container:

```bash
docker start mycontainer3
```

Remove container:

```bash
docker rm -f mycontainer3
```

Remove image:

```bash
docker rmi myapp:v3
```

## Troubleshooting

| Issue | Troubleshooting |
|---|---|
| Container exits | Check `docker logs` |
| Application unavailable | Verify port mapping |
| Health check unhealthy | Test endpoint using curl |
| Dependency installation failure | Verify requirements.txt |
| Port already allocated | Use another host port |
| Container cannot start | Inspect container logs |

## Key Learning Outcomes

- Docker image creation and container deployment.
- Understanding Dockerfile instructions.
- Dependency management and Docker layer caching.
- Port mapping and container networking.
- Docker health monitoring.
- Container troubleshooting using Docker CLI.
- Running FastAPI applications using Uvicorn.

## Author

Pavan Gowda K N

**Project:** FastAPI Application Containerization using Docker
