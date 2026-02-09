
---

# ✅ **`example-notes.md` (Copy & Paste)**

```markdown
# Docker Example Notes

A collection of clean, ready-to-copy examples for Docker commands, Dockerfiles, volumes, networks, and Docker Compose.  
Use these examples in your projects, labs, or GitHub portfolio.

---

# 📌 1. Basic Dockerfile Example
```dockerfile
FROM node:20-alpine

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "start"]
```

---

# 📌 2. Python Dockerfile Example
```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

CMD ["python", "app.py"]
```

---

# 📌 3. Build Image Examples
```bash
docker build .
docker build -t myapp:latest .
docker build -t myapp:v2 .
```

---

# 📌 4. Run Container Examples
```bash
docker run myapp
docker run -p 3000:3000 myapp
docker run -d -p 3000:3000 myapp
docker run --rm -p 3000:3000 myapp
docker run --name mycontainer -d myapp
docker run -it myapp sh
```

---

# 📌 5. Volume Examples
### Create & mount a volume
```bash
docker run -it --rm -v myvolume:/data alpine sh
```

### List volumes
```bash
docker volume ls
```

### Inspect volume
```bash
docker volume inspect myvolume
```

---

# 📌 6. Bind Mount Examples
### Mount local folder into container
```bash
docker run -p 3000:3000 \
  -v C:\projects\app:/app \
  --rm -d myapp
```

---

# 📌 7. Networking Examples
### Create network
```bash
docker network create mynetwork
```

### Run container on network
```bash
docker run --network mynetwork myapp
```

---

# 📌 8. Docker Compose — Single Container
```yaml
version: "3.9"

services:
  app:
    build: .
    container_name: myapp
    ports:
      - "3000:3000"
    volumes:
      - .:/app
    environment:
      NODE_ENV: development
    restart: unless-stopped
```

---

# 📌 9. Docker Compose — Multi-Container Example
```yaml
version: "3.9"

services:
  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
    depends_on:
      backend:
        condition: service_healthy
    networks:
      - app_net

  backend:
    build: ./backend
    ports:
      - "4000:4000"
    environment:
      DB_HOST: db
      DB_USER: appuser
      DB_PASS: apppassword
      DB_NAME: appdb
    depends_on:
      db:
        condition: service_healthy
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:4000/health"]
      interval: 10s
      retries: 5
    networks:
      - app_net

  db:
    image: postgres:15
    environment:
      POSTGRES_USER: appuser
      POSTGRES_PASSWORD: apppassword
      POSTGRES_DB: appdb
    volumes:
      - db_data:/var/lib/postgresql/data
    networks:
      - app_net

volumes:
  db_data:

networks:
  app_net:
    driver: bridge
```

---

# 📌 10. Docker Compose Commands
```bash
docker-compose up
docker-compose up -d
docker-compose down
docker-compose logs -f
```

---

# 📌 11. Push Image to Docker Hub
```bash
docker tag myapp:latest mydockerhub/myapp:latest
docker push mydockerhub/myapp:latest
```

---

# 📌 12. Useful Cleanup Commands
```bash
docker system prune
docker system prune -a
docker volume prune
docker network prune
```

---




