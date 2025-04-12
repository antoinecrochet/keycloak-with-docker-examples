# Keycloak Cluster with TCPPING

> [!WARNING]  
> As of **Keycloak 26**, the **TCPPING** stack for node discovery is **deprecated**. While it is still supported, **Keycloak recommends using `JDBC_PING`** for clustering.
> This example demonstrates **TCPPING** for educational purposes, but **it is advised to use `JDBC_PING`** for new production deployments. **TCP stack support may be removed in a future release of Keycloak.**

This example demonstrates how to set up a simple **Keycloak HA cluster** using **TCPPING** for node discovery and **PostgreSQL** as the shared backend database. A lightweight **nginx** is used as a reverse proxy to load balance requests between two Keycloak nodes.

**TCPPING** for node discovery can be used when multicast is not available, e.g. deployments cross DC, containers cross host. In this example both keycloak server are on the same host but we assume the containers are on different hosts.

---

## 📁 Folder Structure
```
keycloak-cluster-with-tcp-ping/
├── docker-compose.yml
├── nginx/
│   └── conf.d/
│       └── keycloak.conf    # Nginx config for keycloak hosts
├── postgres/
│   └── data/                # Mounted volume for database persistence
```

---

## 🧰 Services Overview

| Service            | Description                                      |
|--------------------|--------------------------------------------------|
| `keycloak1`        | First Keycloak node (with JDBC_PING)             |
| `keycloak2`        | Second Keycloak node (with JDBC_PING)            |
| `keycloak-database`| PostgreSQL database for Keycloak persistence     |
| `nginx`            | Reverse proxy with vhosts `auth.localhost` & `auth-admin.localhost` |

---

## 🚀 Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/antoinecrochet/keycloak-with-docker-examples.git
cd keycloak-with-docker-examples/keycloak-cluster-with-tcp-ping
```

### 2. Create a `.env` File

Create a `.env` file in this folder:

```env
POSTGRES_USER=keycloak
POSTGRES_PASSWORD=changeme
POSTGRES_DB=keycloak
```

> ⚠️ Make sure to change these credentials for anything beyond local development.

### 3. Update Your Hosts File

Add the following lines to your system's `/etc/hosts` file:

```bash
127.0.0.1 auth.localhost
127.0.0.1 auth-admin.localhost
```

This ensures the Nginx virtual hosts will resolve correctly.

### 4. Start the Cluster

```bash
docker compose up -d
```

Once everything is up, open your browser:

- 👉 `http://auth.localhost` — public-facing Keycloak access  
- 🔐 `http://auth-admin.localhost` — admin console

---

## 🔒 Admin Credentials

| Username | Password |
|----------|----------|
| `admin`  | `admin` *(you will be prompted to change this on first login)* |

---

## 🔍 Notes

- Keycloak is running in **`start-dev`** mode for ease of use.
- `tcp` is used for cluster node discovery.
- Both `/metrics` and `/health` endpoints are enabled by default.
- Nginx handles round-robin load balancing across both Keycloak nodes.

---

## 🧼 Cleanup

To stop and remove all containers and volumes:

```bash
docker-compose down -v
```