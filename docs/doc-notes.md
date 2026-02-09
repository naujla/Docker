
---

# ✅ **`Docker-Notes.md` (Copy & Paste)**

```markdown
# Docker Notes & Practical Examples

A complete collection of Docker commands, examples, and best practices.  
This guide is designed for learning, interviews, and real-world DevOps/SRE work.

---

# 📌 1. Check Node.js Installation
```bash
node -v
```

---

# 📌 2. Build Docker Images
```bash
docker build .
docker build -t <RepositoryName>:<Tag> .
docker build -t <BuildName>:<ChangeVersion> .
```

**Suggestion:**  
Always tag your images. Untagged images become `<none>:<none>` and clutter your system.

---

# 📌 3. List Docker Images
```bash
docker image ls
```

---

# 📌 4. Run Docker Containers

### Basic run
```bash
docker run <ImageID>
```

### Port mapping
```bash
docker run -p 3000:3000 <ImageID>
```

### Detached mode + port mapping
```bash
docker run -d -p <HostPort>:<DockerPort> <ImageID>
```

### Auto-remove container after stop
```bash
docker run -d --rm -p <HostPort>:<DockerPort> <ImageID>
```

### Assign a container name
```bash
docker run --name "ContainerName" --rm -d -p <HostPort>:<DockerPort> <ImageID>
```

### Interactive mode
```bash
docker run -it <ContainerID>
```

**Suggestion:**  
Use `--rm` for temporary containers to avoid cleanup later.

---

# 📌 5. Manage Containers

### Show running containers
```bash
docker ps
```

### Show all containers (running + stopped)
```bash
docker ps -a
```

### Remove containers
```bash
docker rm <ContainerName>
```

---

# 📌 6. Remove Docker Images
```bash
docker rmi <name>:<tag>
```

---

# 📌 7. Push Images to Docker Hub

### Create repository on DockerHub  
Then tag your image:
```bash
docker tag <ContainerName:tag> <DockerRepositoryName:tag>
```

Remove old image (optional):
```bash
docker rmi <ContainerName:tag>
```

Push:
```bash
docker push <DockerRepositoryName:tag>
```

**Suggestion:**  
Use semantic versioning: `1.0.0`, `1.0.1`, `2.0.0`.

---

# 📌 8. Docker Volumes

### Create and mount a volume
```bash
docker run -it --rm -v <VolumeName>:/myapp/ <ImageID>
```

### Volume commands
```bash
docker volume -h
docker volume ls
docker volume inspect <VolumeName>
```

**Suggestion:**  
Use volumes for persistent data (DBs, logs, uploads).

---

# 📌 9. Bind Mounts (Local ↔ Container)
```bash
docker run -p 3000:3000 \
  -v C:\Users\aujla\project\reactapp\server.txt:/myapp/server.txt \
  --rm -d <containerID>
```

**Suggestion:**  
Use bind mounts for development; use volumes for production.

---

# 📌 10. .gitignore
Create `.gitignore` and list files/folders to exclude.

Example:
```
node_modules/
.env
*.log
```

---

# 📌 11. API Connectivity Examples

### Install Python packages inside Dockerfile
```dockerfile
RUN pip install requests
RUN pip install pymysql
RUN pip install cryptography
```

### DB Hostname Notes
- For physical DB: use `host.docker.internal`
- For container DB: use container name or inspect IP  
  ```bash
  docker inspect <containerID>
  ```

---

# 📌 12. Check Logs
```bash
docker logs <ContainerNameOrID>
```

---

# 📌 13. Environment Variables
```bash
docker run --env MYSQL_ROOT_PASSWORD="root" <ImageID>
```

---

# 📌 14. Docker Networking
```bash
docker network -h
docker network create <NetworkName>
docker run --network <NetworkName> <ContainerID_or_Name>
```

**Suggestion:**  
Use custom networks for multi-container apps.

---

# 📌 15. Docker Compose (Single Container Example)

```yaml
version: "3.9"

services:
  app:
    image: myapp:latest
    build:
      context: .
      dockerfile: Dockerfile
      args:
        APP_ENV: "development"
      target: builder

    container_name: myapp_container
    command: ["npm", "start"]
    entrypoint: ["node"]

    environment:
      NODE_ENV: "production"
      APP_PORT: "3000"
      LOG_LEVEL: "debug"

    env_file:
      - .env

    ports:
      - "3000:3000"

    volumes:
      - ./src:/usr/src/app
      - app_data:/var/lib/appdata

    working_dir: /usr/src/app
    networks:
      - app_network
    restart: unless-stopped

    deploy:
      resources:
        limits:
          cpus: "0.50"
          memory: "512M"
        reservations:
          cpus: "0.25"
          memory: "256M"

    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:3000/health"]
      interval: 30s
      timeout: 10s
      retries: 5
      start_period: 10s

    logging:
      driver: "json-file"
      options:
        max-size: "10m"
        max-file: "3"

    user: "1000:1000"
    privileged: false

    dns:
      - 8.8.8.8
      - 1.1.1.1

    extra_hosts:
      - "host.docker.internal:host-gateway"

    depends_on:
      dummy_service:
        condition: service_started

  dummy_service:
    image: alpine
    command: ["sleep", "3600"]

volumes:
  app_data:

networks:
  app_network:
    driver: bridge
```

---

# 📌 16. Docker Compose Commands
```bash
docker-compose up
docker-compose up -d
docker-compose down
```

---

# 📌 17. Docker Compose (Multi-Container Example)

```yaml
version: "3.9"

services:
  frontend:
    build:
      context: ./frontend
      dockerfile: Dockerfile
    container_name: frontend
    ports:
      - "3000:3000"
    environment:
      API_URL: "http://backend:4000"
    depends_on:
      backend:
        condition: service_healthy
    networks:
      - app_net

  backend:
    build:
      context: ./backend
      dockerfile: Dockerfile
    container_name: backend
    ports:
      - "4000:4000"
    environment:
      DB_HOST: "db"
      DB_USER: "appuser"
      DB_PASS: "apppassword"
      DB_NAME: "appdb"
    depends_on:
      db:
        condition: service_healthy
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:4000/health"]
      interval: 20s
      timeout: 5s
      retries: 5
    networks:
      - app_net

  db:
    image: postgres:15
    container_name: postgres_db
    restart: unless-stopped
    environment:
      POSTGRES_USER: "appuser"
      POSTGRES_PASSWORD: "apppassword"
      POSTGRES_DB: "appdb"
    volumes:
      - db_data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U appuser"]
      interval: 10s
      timeout: 5s
      retries: 5
    networks:
      - app_net

volumes:
  db_data:

networks:
  app_net:
    driver: bridge
```

---

# 📌 18. Docker Compose Network Commands
```bash
docker-compose network ls
```

---


