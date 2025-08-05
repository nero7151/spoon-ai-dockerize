# SPOON-AI Dockerize Project
`This content is AI-generated.`

A comprehensive multi-service application featuring AI text generation capabilities, built with FastAPI, NestJS backend, Next.js frontend, and PostgreSQL database, all orchestrated with Docker Compose.

## Table of Contents

- [Project Overview](#project-overview)
- [Architecture](#architecture)
- [Prerequisites](#prerequisites)
- [Quick Start](#quick-start)
- [Detailed Setup Guide](#detailed-setup-guide)
- [Service Details](#service-details)
- [Development Workflow](#development-workflow)
- [Troubleshooting](#troubleshooting)
- [Production Deployment](#production-deployment)

## Project Overview

This project consists of four main services:

- **AI Module**: FastAPI service with GPT-2 text generation capabilities
- **Backend**: NestJS REST API server
- **Frontend**: Next.js React application with TypeScript
- **Database**: PostgreSQL database for data persistence

## Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Frontend      │    │    Backend      │    │   AI Module     │
│   (Next.js)     │◄──►│   (NestJS)      │◄──►│   (FastAPI)     │
│   Port: 8000    │    │   Port: 3000    │    │   Port: 7860    │
└─────────────────┘    └─────────────────┘    └─────────────────┘
                                │
                                ▼
                       ┌─────────────────┐
                       │   Database      │
                       │  (PostgreSQL)   │
                       │   Port: 5432    │
                       └─────────────────┘
```

## Prerequisites

Before starting, ensure you have the following installed on your system:

- **Docker**: Version 20.10 or higher
- **Docker Compose**: Version 2.0 or higher
- **Git**: For cloning submodules
- **SSH Keys**: Configured for GitHub and Hugging Face access

### System Requirements

- **RAM**: Minimum 8GB (AI module requires significant memory for model loading)
- **Storage**: At least 5GB free space
- **CPU**: Multi-core processor recommended

## Quick Start

### 1. Clone the Repository with Submodules

```bash
# Clone the main repository
git clone --recursive git@github.com:nero7151/spoon-ai-dockerize.git spoon-ai-dockerize
cd spoon-ai-dockerize

# If you already cloned without --recursive, initialize submodules
git submodule update --init --recursive
```

### 2. Start All Services

```bash
# Build and start all services in detached mode
docker-compose up -d --build

# View logs from all services
docker-compose logs -f

# Check service status
docker-compose ps
```

### 3. Access the Application

- **Frontend**: http://localhost:8000
- **Backend API**: http://localhost:3000
- **AI Module**: http://localhost:7860
- **Database**: localhost:5432

## Detailed Setup Guide

### Step 1: Initialize Git Submodules

This project uses Git submodules to manage separate repositories:

```bash
# Initialize all submodules
git submodule init

# Update submodules to latest commits
git submodule update --recursive

# Alternative: Clone with submodules in one command
git clone --recursive <repository-url>
```

**Submodule Details:**
- `backend/` → spoon-ai-backend (NestJS API)
- `frontend/` → spoon-ai-frontend (Next.js UI)  
- `ai-module/` → SPOON-AI Hugging Face Space (FastAPI + GPT-2)

### Step 2: Environment Configuration

Create environment files for each service:

#### Backend Environment (.env in backend/)
```bash
# Database configuration
DATABASE_URL=postgresql://spoon_db:password@db:5432/spoon_db
NODE_ENV=development
PORT=3000

# AI Module integration
AI_MODULE_URL=http://ai-module:7860
```

#### Frontend Environment (.env.local in frontend/)
```bash
# API endpoints
NEXT_PUBLIC_API_URL=http://localhost:3000
NEXT_PUBLIC_AI_URL=http://localhost:7860
```

### Step 3: Build and Deploy Services

#### Option A: Development Mode (Recommended)

```bash
# Build all services
docker-compose build

# Start services with hot-reload
docker-compose up -d

# Monitor logs
docker-compose logs -f backend frontend ai-module
```

#### Option B: Production Mode

```bash
# Use production target for build
docker-compose -f docker-compose.yml -f docker-compose.prod.yml up -d --build
```

### Step 4: Initialize Database

```bash
# Access the database container
docker-compose exec db psql -U spoon_db -d spoon_db

# Run any initialization scripts (if available)
docker-compose exec backend npm run migration:run
```

## Service Details

### AI Module (FastAPI + GPT-2)

- **Framework**: FastAPI with Uvicorn
- **Model**: GPT-2 text generation
- **Port**: 7860 (internal) / 7860 (external)
- **Health Check**: GET http://localhost:7860/

**Key Features:**
- Text generation endpoint: POST `/generate`
- Model preloading on startup
- Configurable generation parameters

**API Usage:**
```bash
curl -X POST http://localhost:7860/generate \
  -H "Content-Type: application/json" \
  -d '{"prompt": "Hello world"}'
```

### Backend (NestJS)

- **Framework**: NestJS with TypeScript
- **Port**: 3000 (internal) / 3000 (external)
- **Database**: PostgreSQL connection
- **Hot Reload**: Enabled in development mode

**Key Features:**
- RESTful API endpoints
- Database integration
- AI module communication
- Swagger documentation (when configured)

### Frontend (Next.js)

- **Framework**: Next.js 15 with React 19
- **Language**: TypeScript
- **Styling**: Tailwind CSS
- **Port**: 3000 (internal) / 8000 (external)

**Key Features:**
- Server-side rendering
- Hot module replacement
- Modern React features
- Responsive design

### Database (PostgreSQL)

- **Image**: PostgreSQL Latest
- **Port**: 5432
- **Credentials**: 
  - User: `spoon_db`
  - Password: `password`
  - Database: `spoon_db`

## Development Workflow

### Starting Development

```bash
# Start all services
docker-compose up -d

# View logs for specific service
docker-compose logs -f frontend

# Restart a specific service
docker-compose restart backend

# Rebuild a service after code changes
docker-compose up -d --build frontend
```

### Code Changes and Hot Reload

- **Frontend**: Changes in `frontend/src/` trigger automatic reload
- **Backend**: Changes in `backend/src/` trigger automatic restart
- **AI Module**: Changes require container rebuild

### Accessing Service Containers

```bash
# Access backend container
docker-compose exec backend bash

# Access frontend container
docker-compose exec frontend sh

# Access database
docker-compose exec db psql -U spoon_db -d spoon_db
```

### Managing Dependencies

#### Backend (Node.js packages)
```bash
# Install new package
docker-compose exec backend npm install <package-name>

# Update packages
docker-compose exec backend npm update
```

#### Frontend (Node.js packages)
```bash
# Install new package
docker-compose exec frontend npm install <package-name>

# Update packages
docker-compose exec frontend npm update
```

#### AI Module (Python packages)
```bash
# Access container and install
docker-compose exec ai-module pip install <package-name>

# Or modify requirements.txt and rebuild
docker-compose up -d --build ai-module
```

## Troubleshooting

### Common Issues

#### 1. Port Conflicts
```bash
# Check if ports are in use
lsof -i :3000 -i :8000 -i :7860 -i :5432

# Stop conflicting services
sudo systemctl stop postgresql  # If local PostgreSQL is running
```

#### 2. Submodule Issues
```bash
# Reset submodules
git submodule deinit -f .
git submodule update --init --recursive

# Update to latest commits
git submodule update --recursive --remote
```

#### 3. Docker Build Failures
```bash
# Clean Docker cache
docker system prune -a

# Force rebuild without cache
docker-compose build --no-cache

# Check Docker logs
docker-compose logs <service-name>
```

#### 4. Memory Issues (AI Module)
```bash
# Check memory usage
docker stats

# Increase Docker memory limit (Docker Desktop)
# Settings → Resources → Advanced → Memory
```

#### 5. Database Connection Issues
```bash
# Check database logs
docker-compose logs db

# Verify database is running
docker-compose exec db pg_isready -U spoon_db

# Reset database
docker-compose down -v
docker-compose up -d db
```

### Debugging Tips

1. **Check service health:**
   ```bash
   docker-compose ps
   curl http://localhost:3000/health  # Backend
   curl http://localhost:7860/       # AI Module
   ```

2. **Monitor resource usage:**
   ```bash
   docker stats --format "table {{.Container}}\t{{.CPUPerc}}\t{{.MemUsage}}"
   ```

3. **View container details:**
   ```bash
   docker-compose exec <service> env  # Check environment variables
   docker-compose exec <service> ps aux  # Check running processes
   ```

## Production Deployment

### Environment Preparation

1. **Create production environment files:**
   ```bash
   # Copy development configs
   cp backend/.env backend/.env.prod
   cp frontend/.env.local frontend/.env.prod
   
   # Update with production values
   # - Change passwords
   # - Update URLs
   # - Set NODE_ENV=production
   ```

2. **Create production docker-compose override:**
   ```yaml
   # docker-compose.prod.yml
   version: '3.8'
   services:
     backend:
       build:
         target: production
     frontend:
       build:
         target: production
     db:
       volumes:
         - postgres_data:/var/lib/postgresql/data
   volumes:
     postgres_data:
   ```

### Deployment Commands

```bash
# Production build and deploy
docker-compose -f docker-compose.yml -f docker-compose.prod.yml up -d --build

# Health check
docker-compose ps
curl -f http://localhost:8000 && echo "Frontend OK"
curl -f http://localhost:3000 && echo "Backend OK"
curl -f http://localhost:7860 && echo "AI Module OK"
```

### Security Considerations

1. **Change default passwords**
2. **Use environment variables for secrets**
3. **Configure reverse proxy (nginx/traefik)**
4. **Enable HTTPS/SSL certificates**
5. **Set up monitoring and logging**

### Backup and Maintenance

```bash
# Database backup
docker-compose exec db pg_dump -U spoon_db spoon_db > backup.sql

# Log rotation
docker-compose logs --tail=1000 > app.log

# Update services
git submodule update --recursive --remote
docker-compose pull
docker-compose up -d --build
```

## Support

For issues and questions:

1. Check the [Troubleshooting](#troubleshooting) section
2. Review service logs: `docker-compose logs <service-name>`
3. Verify system requirements are met
4. Ensure all submodules are properly initialized
