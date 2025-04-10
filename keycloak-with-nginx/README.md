# Keycloak with Nginx  

This example demonstrates how to run a **single-node Keycloak server** behind **nginx** with a PostgreSQL database and optional custom themes. Nginx acts as a reverse proxy, exposing virtual hosts for public and admin access.

---

## 📁 Folder Structure
```
keycloak-with-nginx/
├── docker-compose.yml
├── nginx/
│   └── conf.d/
│       └── keycloak.conf    # Nginx config for keycloak hosts
├── keycloak/
│   └── themes/              # Optional custom Keycloak themes
├── postgres/
│   └── data/                # Database persistence volume
```

---

## 🧰 Services Overview

| Service            | Description                                      |
|--------------------|--------------------------------------------------|
| `keycloak`         | Single Keycloak instance with PostgreSQL backend |
| `keycloak-database`| PostgreSQL database                              |
| `nginx`            | Reverse proxy with `auth.localhost` & `auth-admin.localhost` vhosts |

---

## 🚀 Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/antoinecrochet/keycloak-with-docker-examples.git
cd keycloak-with-docker-examples/keycloak-with-nginx
```

### 2. Create a `.env` File

```env
POSTGRES_USER=keycloak
POSTGRES_PASSWORD=changeme
POSTGRES_DB=keycloak
```

> ⚠️ Don't forget to change credentials if you're using this beyond local testing.

### 3. Update Your Hosts File

Add these lines to your system’s `/etc/hosts` file:

```bash
127.0.0.1 auth.localhost
127.0.0.1 auth-admin.localhost
```

This allows the nginx reverse proxy to correctly route requests based on the hostname.

### 4. Start the Environment

```bash
docker compose up -d
```

### 5. Access Keycloak

- 🌐 `http://auth.localhost` — public-facing Keycloak  
- 🛠 `http://auth-admin.localhost` — admin console login

---

## 🔐 Admin Credentials

| Username | Password |
|----------|----------|
| `admin`  | `admin` *(you’ll be prompted to change this on first login)* |

---

## 🎨 Custom Themes

Mount your custom Keycloak themes inside the `./keycloak/themes/` folder. They will be available under `/opt/keycloak/themes` inside the container.

---

## 🔍 Notes

- Keycloak runs in `start-dev` mode for ease of local development.
- Nginx performs virtual host-based routing.
- Health and metrics endpoints are enabled.

---

## 🧼 Cleanup

To shut down and remove volumes:

```bash
docker-compose down -v
```