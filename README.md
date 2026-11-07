# ECommerce – Dockerized Application

This repository contains the ECommerce project, Dockerized and prepared for the deployment phase.

## Project Components

- **Frontend:** React application served by Nginx.
- **Backend:** Django REST API served by Gunicorn.
- **Database:** PostgreSQL 16 for local development.
- **Demo Data:** A PostgreSQL database backup containing the existing project data.

## Run the Project Locally

### 1. Clone the Repository

```bash
git clone https://github.com/MoatazDiaaAta/automated-ecommerce-deployment-platform.git
cd automated-ecommerce-deployment-platform
```

### 2. Configure Environment Variables

```bash
cp .env.example .env
```

Update `.env` and set a secure `DJANGO_SECRET_KEY`.

### 3. Build and Start the Containers

```bash
docker compose up --build -d
```

### 4. Restore the Demo Database

The repository includes `backups/sportszone.dump`, a PostgreSQL custom-format backup containing the existing demo data.

Restore it into the local PostgreSQL container:

```bash
docker compose exec -T db pg_restore -U postgres -d sportszone --no-owner --no-privileges < backups/sportszone.dump
```

**Important:** Restore the backup into a fresh, empty database. The backup already contains the database schema and data, so do not run migrations before restoring it.

## Application URLs

- **Frontend:** http://localhost:3000
- **Products API:** http://localhost:3000/api/products/
- **Users API:** http://localhost:3000/api/users/
- **Orders API:** http://localhost:3000/api/orders/
- **Django Admin:** http://localhost:3000/admin/

## Database and Deployment

Docker Compose runs PostgreSQL locally for development.

For production, the application will use **Amazon RDS for PostgreSQL** instead of a PostgreSQL container.

The deployment team should:

1. Create an Amazon RDS PostgreSQL instance.
2. Make the RDS instance accessible from the application environment through the appropriate VPC and security group configuration.
3. Restore `backups/sportszone.dump` into an empty RDS database from an EC2 instance or another machine with network access to RDS.
4. Configure the Django database environment variables to use the RDS endpoint and credentials.

Example restore command for RDS:

```bash
pg_restore -h <RDS-ENDPOINT> -p 5432 -U <DB_USER> -d <DB_NAME> --no-owner --no-privileges backups/sportszone.dump
```

Use the appropriate RDS connection and SSL settings.

## Deployment Handover

The application is Dockerized and ready for the next deployment phase.

The deployment team will continue with:

- AWS infrastructure on EC2.
- Amazon RDS for the production database.
- Jenkins CI/CD pipeline.
- Kubernetes deployment.

**Note:** The PostgreSQL container in Docker Compose is intended for local development only. Production deployment must connect to Amazon RDS through environment variables.
