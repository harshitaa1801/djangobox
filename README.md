# DjangoBox

A production-ready Django project template with Docker, PostgreSQL, and Nginx.

## Features

- Django 5.2+ with PostgreSQL database
- Docker containerization with separate dev/prod configurations
- Nginx reverse proxy with static/media file serving
- Environment-based configuration
- Production-ready security settings

## Prerequisites

Before setting up this project, make sure you have the following installed on your system:

- **Docker**: Version 20.0 or higher (includes Docker Compose)
  - [Install Docker](https://docs.docker.com/get-docker/)
  - Docker Compose is included with Docker Desktop and modern Docker installations
- **Python**: Version 3.10 or higher
  - [Download Python](https://www.python.org/downloads/)
- **Git**: For version control
  - [Install Git](https://git-scm.com/downloads/)


### Verify Installation
You can verify your installation by running:
```bash
docker --version
```

## Quick Start

### Development Setup

1. Clone the repository:
```bash
git clone https://github.com/harshitaa1801/djangobox.git
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

## Creating a New Django App

After setting up the project, you can create new Django apps for your features:

### Method 1: Using Docker (Recommended)

1. Create a new app inside the running container:
```bash
# For development environment
docker exec web python manage.py startapp your_app_name

# For production environment
docker exec web python manage.py startapp your_app_name
```

### Method 2: Using Local Django

1. If you have Django installed locally:
```bash
python manage.py startapp your_app_name
```

### After Creating an App

1. Add your new app to `INSTALLED_APPS` in `main/settings.py`:
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'your_app_name',  # Add your app here
]
```

2. Create your models, views, and URLs as needed

3. Run migrations to apply any database changes:
```bash
# Using Docker
docker-compose -f docker-compose-dev.yml exec web python manage.py makemigrations
docker-compose -f docker-compose-dev.yml exec web python manage.py migrate

# Or locally
python manage.py makemigrations
python manage.py migrate
```

4. Include your app's URLs in `main/urls.py`:
```python
from django.contrib import admin
from django.urls import path, include
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('your_app_name.urls')),  # Add this line
]

# Serve static and media files during development
if settings.DEBUG:
    urlpatterns += static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
    urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

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