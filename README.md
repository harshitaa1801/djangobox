# DjangoBox

A production-ready Django project template with Docker, PostgreSQL, and Nginx.

## Features

- Django 5.2+ with PostgreSQL database
- Docker containerization with separate dev/prod configurations
- Nginx reverse proxy with static/media file serving
- Environment-based configuration
- Production-ready security settings

## Quick Start

### Development Setup

1. Clone the repository:
```bash
git clone <your-repo-url>
cd djangobox
```

2. Create environment file:
```bash
cp .env.dev.example .env.dev
# Edit .env.dev with your values
```

3. Build and run with Docker:
```bash
docker-compose -f docker-compose-dev.yml up --build
```

4. Access the application at `http://localhost`

### Production Setup

1. Create production environment file:
```bash
cp .env.prod.example .env.prod
# Edit .env.prod with secure values
```

2. Update `nginx/nginx.prod.conf` with your domain name

3. Build and run:
```bash
docker-compose -f docker-compose-prod.yml up -d --build
```

## Environment Variables

### Required Variables

- `SECRET_KEY`: Django secret key (generate a secure one for production)
- `DEBUG`: Set to `False` for production
- `ALLOWED_HOSTS`: Comma-separated list of allowed hosts
- `POSTGRES_DB`: Database name
- `POSTGRES_USER`: Database user
- `POSTGRES_PASSWORD`: Database password

## Security Notes

- Always use a secure `SECRET_KEY` in production
- Set `DEBUG=False` in production
- Configure `ALLOWED_HOSTS` properly
- Use strong database passwords
- Consider adding SSL/TLS certificates for HTTPS

## Project Structure

```
djangobox/
├── main/                    # Django project settings
├── nginx/                   # Nginx configuration files
├── docker-compose-dev.yml   # Development Docker setup
├── docker-compose-prod.yml  # Production Docker setup
├── Dockerfile              # Django app container
├── entrypoint.sh           # Container entrypoint script
└── requirements.txt        # Python dependencies
```