# keycloak-deployment-basic

This configuration provides a Docker Compose configuration to set up a **Keycloak** instance with a **PostgreSQL** database and an **Nginx** reverse proxy.

## Services

The Docker Compose file defines the following services:

### 1. Keycloak
- **Image**: `keycloak/keycloak:26.1`
- **Environment Variables**:
  - `KC_BOOTSTRAP_ADMIN_USERNAME`: Admin username.
  - `KC_BOOTSTRAP_ADMIN_PASSWORD`: Admin password - remember to change this after first login.
  - `KC_DB`: Database vendor
  - `KC_DB_URL`: JDBC url for the PostgreSQL database
  - `KC_DB_USERNAME`: Username for the PostgreSQL database (set via `POSTGRES_USER` environment variable).
  - `KC_DB_PASSWORD`: Password for the PostgreSQL database (set via `POSTGRES_PASSWORD` environment variable).
  - `KC_METRICS_ENABLED`: If the server should expose metrics.
  - `KC_HEALTH_ENABLED`: If the server should expose health check endpoints.
- **Volumes**:
  - Mounts custom Keycloak themes from the `./keycloak/themes` directory to the container.

### 2. Keycloak Database (Postgres)
- **Image**: `postgres:17`
- **Environment Variables**:
  - `POSTGRES_USER`: Username for the PostgreSQL database.
  - `POSTGRES_PASSWORD`: Password for the PostgreSQL database.
  - `POSTGRES_DB`: Name of the PostgreSQL database.
- **Volumes**:
  - Mounts the `./postgres/data` directory for persistent storage of PostgreSQL data.

### 3. Nginx
- **Image**: `nginx`
- **Container Name**: `nginx`
- **Ports**: Exposes port `80` on the host, forwarding to port `80` in the container.

## Requirements

- Docker
- Docker Compose

## Configuration

### Environment Variables

Before running the services, define the following environment variables in a `.env` file or the environment:

- `POSTGRES_USER`: Username for PostgreSQL (e.g., `keycloak_user`).
- `POSTGRES_PASSWORD`: Password for PostgreSQL (e.g., `securepassword`).
- `POSTGRES_DB`: Name of the PostgreSQL database (e.g., `keycloak_db`).

### Keycloak Themes

You can customize the Keycloak themes by placing theme files in the `./keycloak/themes` directory. This will be mounted into the Keycloak container which allows you to add new themes on the fly, you won't need to restart the server to use them.

### Nginx Configuration

You can customize the Nginx configuration by placing your custom Nginx configuration files in the `./nginx/conf.d` directory.

## Running the Setup

1. Clone the repository or download the files.
2. Make sure the environment variables are set by creating a `.env` file or setting them directly in your environment.
3. Run Docker Compose to start the services:

   ```bash
   docker compose up
