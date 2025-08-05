# SPOON-AI Docker Project Scripts
`This content is AI-generated.`

This directory contains helpful scripts for managing the SPOON-AI Docker environment.

## Available Scripts

### `init.sh` - Complete Project Initialization

This script handles the complete setup of the SPOON-AI project:

```bash
# Make executable and run
chmod +x init.sh
./init.sh
```

**What it does:**
- ✅ Checks prerequisites (Docker, Docker Compose, Git)
- ✅ Initializes and updates Git submodules
- ✅ Creates environment configuration from template
- ✅ Builds all Docker images
- ✅ Starts all services
- ✅ Performs health checks on all services
- ✅ Provides service URLs and next steps

### Quick Commands

```bash
# Full setup (recommended for first time)
./init.sh

# Development mode
docker-compose up -d --build

# Production mode  
docker-compose -f docker-compose.yml -f docker-compose.prod.yml up -d --build

# View logs
docker-compose logs -f

# Stop all services
docker-compose down

# Clean restart
docker-compose down && docker-compose up -d --build

# Health check
docker-compose ps
curl http://localhost:8000  # Frontend
curl http://localhost:3000  # Backend
curl http://localhost:7860  # AI Module
```

## Service Management

### Individual Service Control

```bash
# Restart specific service
docker-compose restart backend

# View logs for specific service
docker-compose logs -f frontend

# Rebuild specific service
docker-compose up -d --build ai-module

# Access service container
docker-compose exec backend bash
```

### Database Management

```bash
# Access PostgreSQL
docker-compose exec db psql -U spoon_db -d spoon_db

# Database backup
docker-compose exec db pg_dump -U spoon_db spoon_db > backup.sql

# Check database status
docker-compose exec db pg_isready -U spoon_db
```

## Troubleshooting

### Common Issues

1. **Port conflicts**: Check if ports 3000, 8000, 7860, or 5432 are in use
2. **Submodule issues**: Run `git submodule update --init --recursive`
3. **Memory issues**: AI module needs sufficient RAM for model loading
4. **Build failures**: Try `docker system prune -a` to clean cache

### Debug Commands

```bash
# Check resource usage
docker stats

# View service status
docker-compose ps

# Check network connectivity
docker-compose exec backend ping ai-module
docker-compose exec frontend ping backend
```

## Development Workflow

1. **Initial Setup**: Run `./init.sh`
2. **Code Changes**: Services auto-reload in development mode
3. **Add Dependencies**: Use `docker-compose exec <service> npm install <package>`
4. **Testing**: Access services via browser or API calls
5. **Logs**: Monitor with `docker-compose logs -f`

For detailed information, see the main README.md file.
