# ---------------------------------------------------------
# Examples Git Ignore for Docker Learning Repository
# ---------------------------------------------------------

# -------------------------
# Node.js
# -------------------------
node_modules/
npm-debug.log
yarn-debug.log
yarn-error.log
pnpm-debug.log

# Environment files
.env
.env.local
.env.*.local

# Build output
dist/
build/
.cache/
.next/
out/

# -------------------------
# Python
# -------------------------
__pycache__/
*.pyc
*.pyo
*.pyd
*.pytest_cache/
*.mypy_cache/
*.ruff_cache/
*.coverage
.coverage.*
htmlcov/

# Virtual environments
venv/
env/
.venv/

# -------------------------
# Docker
# -------------------------
# Ignore Docker build context junk
Dockerfile~
docker-compose.override.yml
docker-compose.debug.yml

# Local volumes & bind mounts
data/
db_data/
*.pid

# -------------------------
# Logs
# -------------------------
*.log
logs/
debug.log

# -------------------------
# OS / Editor Files
# -------------------------
.DS_Store
Thumbs.db
desktop.ini

# VSCode
.vscode/
.history/

# JetBrains IDEs
.idea/

# -------------------------
# Git
# -------------------------
.git/
.gitignore
.gitattributes

# -------------------------
# Temporary Files
# -------------------------
*.tmp
*.temp
*.swp
*.swo

# -------------------------
# Project-specific
# -------------------------
# Ignore compiled assets
*.out
*.exe

# Ignore uploads or runtime data
uploads/
runtime