

---

```markdown
# Docker Assets & Reusable Templates

This document contains reusable assets, templates, diagrams, and snippets that support Docker learning, documentation, and portfolio projects.  
Use these assets in READMEs, architecture diagrams, training material, or GitHub repositories.

---

# 📦 1. Docker Architecture Diagram (ASCII)

```
+---------------------------+
|        Docker CLI         |
+-------------+-------------+
              |
              v
+---------------------------+
|      Docker Engine        |
|  - REST API               |
|  - Docker Daemon (dockerd)|
+-------------+-------------+
              |
              v
+---------------------------+
|        Container          |
|  - Filesystem (Layers)    |
|  - Network Namespace      |
|  - Process Namespace      |
|  - cgroups (limits)       |
+---------------------------+
```

---

# 📦 2. Container Lifecycle Diagram (ASCII)

```
   docker build
        |
        v
   +----------+
   |  Image   |
   +----------+
        |
 docker run
        |
        v
   +----------+
   | Container|
   +----------+
        |
 docker stop
        |
        v
   +----------+
   | Stopped  |
   +----------+
```

---

# 📦 3. Dockerfile Template (Reusable)

```dockerfile
FROM node:20-alpine

WORKDIR /app

COPY package*.json ./
RUN npm install --production

COPY . .

EXPOSE 3000

CMD ["npm", "start"]
```

---

# 📦 4. Python Dockerfile Template

```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD ["python", "app.py"]
```

---

# 📦 5. .dockerignore Template

```
node_modules/
.env
*.log
__pycache__/
.git
.gitignore
Dockerfile
docker-compose.yml
```

---

# 📦 6. Common Docker Commands (Quick Copy)

```bash
docker build -t myapp:latest .
docker run -p 3000:3000 myapp
docker ps
docker logs myapp
docker stop myapp
docker rm myapp
docker rmi myapp:latest
```

---

# 📦 7. Volume & Bind Mount Examples

### Named Volume
```bash
docker run -it --rm -v myvolume:/data alpine sh
```

### Bind Mount
```bash
docker run -p 3000:3000 \
  -v C:\projects\app:/app \
  --rm -d myapp
```

---

# 📦 8. Docker Network Examples

### Create network
```bash
docker network create app_net
```

### Run container on network
```bash
docker run --network app_net myapp
```

---

# 📦 9. Docker Compose Snippet (Service Template)

```yaml
services:
  app:
    build: .
    container_name: app
    ports:
      - "3000:3000"
    environment:
      NODE_ENV: development
    volumes:
      - .:/app
    restart: unless-stopped
```

---

# 📦 10. Multi-Container Architecture Diagram (ASCII)

```
          +-------------+
          |  Frontend   |
          |  (React)    |
          +------+------+
                 |
                 | API Calls
                 v
          +-------------+
          |  Backend    |
          |  (Node/Py)  |
          +------+------+
                 |
                 | DB Connection
                 v
          +-------------+
          |  Database   |
          | (Postgres)  |
          +-------------+
```

---

# 📦 11. Healthcheck Template

```yaml
healthcheck:
  test: ["CMD", "curl", "-f", "http://localhost:3000/health"]
  interval: 30s
  timeout: 10s
  retries: 5
  start_period: 10s
```

---

# 📦 12. Resource Limits Template

```yaml
deploy:
  resources:
    limits:
      cpus: "0.50"
      memory: "512M"
    reservations:
      cpus: "0.25"
      memory: "256M"
```

---

# 📦 13. Logging Configuration Template

```yaml
logging:
  driver: "json-file"
  options:
    max-size: "10m"
    max-file: "3"
```

---

# 📦 14. Environment Variable Examples

```bash
docker run --env APP_ENV=production --env PORT=3000 myapp
```

---

# 📦 15. Docker Hub Push Template

```bash
docker tag myapp:latest mydockerhub/myapp:latest
docker push mydockerhub/myapp:latest
```

---

# ⭐ Suggestions for Using These Assets

- Add diagrams to your README for visual clarity.
- Use templates to standardize Dockerfiles across projects.
- Keep `.dockerignore` updated to reduce build context.
- Use healthchecks and resource limits for production-grade setups.
- Store all diagrams in `/assets/diagrams/` for clean repo organization.

---




