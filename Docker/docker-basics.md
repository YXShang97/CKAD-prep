# Docker Basics for CKAD

## Essential Docker Commands

### Build Images

```bash
docker build -t app:v1.0 .                    # Build image from Dockerfile
docker build -t app:latest --no-cache .        # Build without cache
docker build -f custom.dockerfile -t app .     # Use specific Dockerfile
```

### Run Containers

```bash
docker run -d --name myapp -p 8080:80 nginx   # Run detached with port mapping
docker run -it ubuntu:20.04 /bin/bash         # Interactive terminal
docker run --rm alpine echo "hello"            # Remove container after exit
docker run -e ENV_VAR=value app:latest        # Set environment variable
```

### Manage Images

```bash
docker images                                  # List images
docker rmi image:tag                          # Remove image
docker tag source:tag target:tag              # Tag image
docker push registry/image:tag                 # Push to registry
docker pull nginx:alpine                       # Pull image
```

### Manage Containers

```bash
docker ps                                      # List running containers
docker ps -a                                   # List all containers
docker stop container_name                     # Stop container
docker start container_name                    # Start container
docker rm container_name                       # Remove container
docker logs container_name                     # View logs
docker exec -it container_name /bin/bash      # Execute command in running container
```

### Registry Operations

```bash
docker login                                   # Login to registry
docker push myregistry.com/app:v1.0          # Push image
docker pull myregistry.com/app:v1.0          # Pull image
```

## Key Concepts for CKAD

### Dockerfile Best Practices

- Use specific base image tags (not `latest`)
- Minimize layers by combining RUN commands
- Use `.dockerignore` to exclude unnecessary files
- Run as non-root user for security
- Use multi-stage builds for smaller images

### Container vs Pod

- Container: Single process in isolated environment
- Pod: Group of containers sharing network and storage
- In Kubernetes, you deploy Pods, not containers directly

### Image Naming

```
registry.com/namespace/repository:tag
├── registry.com (optional, defaults to docker.io)
├── namespace (optional for Docker Hub)
├── repository (required)
└── tag (optional, defaults to 'latest')
```

## Common CKAD Scenarios

```bash
# Build and push image for Kubernetes
docker build -t myregistry.com/app:v1.0 .
docker push myregistry.com/app:v1.0

# Test image locally before Kubernetes deployment
docker run -d -p 8080:8080 --name test-app myregistry.com/app:v1.0
docker logs test-app
docker exec -it test-app /bin/sh

# Clean up
docker stop test-app && docker rm test-app
```
