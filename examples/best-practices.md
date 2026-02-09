

```markdown
# Dockerfile Best Practices

This guide covers the most important best practices for writing secure, efficient, and production‑ready Dockerfiles.  
Use these patterns in real projects, interviews, and your GitHub portfolio.

---

# 📌 1. Use Small, Secure Base Images

### Prefer slim or alpine images:
```dockerfile
FROM node:20-alpine
FROM python:3.11-slim
```

### Avoid large images:
❌ `FROM ubuntu:latest`  
❌ `FROM node:latest`

**Why:** Smaller images reduce attack surface, build time, and deployment size.

---

# 📌 2. Always Use a .dockerignore File

Create a `.dockerignore` to reduce build context:

```
node_modules/
.env
.git
*.log
__pycache__/
```

**Why:** Prevents unnecessary files from being copied into the image.

---

# 📌 3. Use Multi‑Stage Builds (Highly Recommended)

### Example:
```dockerfile
# Build stage
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .

# Production stage
FROM node:20-alpine
WORKDIR /app
COPY --from=builder /app .
CMD ["npm", "start"]
```

**Why:** Keeps final image small and secure.

---

# 📌 4. Pin Versions (Never Use "latest")

```dockerfile
FROM python:3.11-slim
RUN pip install flask==3.0.0
```

**Why:** Ensures consistent builds and avoids breaking changes.

---

# 📌 5. Set a Working Directory

```dockerfile
WORKDIR /app
```

**Why:** Avoids long paths and improves readability.

---

# 📌 6. Copy Only What You Need

```dockerfile
COPY package*.json ./
RUN npm install
COPY . .
```

**Why:** Reduces rebuild time by leveraging Docker layer caching.

---

# 📌 7. Combine Commands to Reduce Layers

```dockerfile
RUN apk update && apk add --no-cache curl bash
```

**Why:** Fewer layers = smaller images.

---

# 📌 8. Avoid Running as Root

```dockerfile
RUN adduser -D appuser
USER appuser
```

**Why:** Improves container security.

---

# 📌 9. Expose Only Required Ports

```dockerfile
EXPOSE 3000
```

**Why:** Documents container behavior for developers.

---

# 📌 10. Use ENTRYPOINT for Main Process

```dockerfile
ENTRYPOINT ["npm", "start"]
```

**Why:** ENTRYPOINT defines the main executable; CMD provides arguments.

---

# 📌 11. Clean Up Package Caches

### Alpine:
```dockerfile
RUN apk add --no-cache python3
```

### Debian/Ubuntu:
```dockerfile
RUN apt-get update && apt-get install -y curl \
    && rm -rf /var/lib/apt/lists/*
```

**Why:** Reduces image size.

---

# 📌 12. Use Labels for Metadata

```dockerfile
LABEL maintainer="narender@example.com"
LABEL version="1.0"
LABEL description="My Node.js App"
```

**Why:** Helps with automation and documentation.

---

# 📌 13. Keep Secrets Out of Dockerfile

❌ Never do this:
```dockerfile
ENV DB_PASSWORD=supersecret
```

**Use instead:**
- `.env` files  
- Docker secrets  
- Environment variables at runtime  

---

# 📌 14. Use Healthchecks (Optional but Recommended)

```dockerfile
HEALTHCHECK --interval=30s --timeout=5s --retries=3 \
  CMD curl -f http://localhost:3000/health || exit 1
```

**Why:** Helps orchestrators detect unhealthy containers.

---

# 📌 15. Example: Production‑Ready Node.js Dockerfile

```dockerfile
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .

FROM node:20-alpine
WORKDIR /app
COPY --from=builder /app .
EXPOSE 3000
USER node
CMD ["npm", "start"]
```

---

# 📌 16. Example: Production‑Ready Python Dockerfile

```dockerfile
FROM python:3.11-slim AS builder
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .

FROM python:3.11-slim
WORKDIR /app
COPY --from=builder /app .
EXPOSE 5000
CMD ["python", "app.py"]
```

---

# ⭐ Summary of Best Practices

- Use **small base images** (`alpine`, `slim`)
- Use **multi-stage builds**
- Always include a **.dockerignore**
- Pin versions (avoid `latest`)
- Use **WORKDIR**
- Copy only what you need
- Avoid running as root
- Clean up caches
- Use labels
- Keep secrets out of Dockerfile
- Use healthchecks for production

---
