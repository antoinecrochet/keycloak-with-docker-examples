# 🛡️ Keycloak with Docker Examples

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](./LICENSE)
[![Dockerized](https://img.shields.io/badge/dockerized-yes-blue.svg)](https://www.docker.com/)
![Status](https://img.shields.io/badge/status-active-brightgreen)
![Keycloak Version](https://img.shields.io/badge/keycloak-26.1-blueviolet)

This repository provides a collection of **ready-to-run Keycloak setups using Docker and Docker Compose**, covering single-node, cluster, and proxy-based configurations. Each subfolder is a self-contained example with its own `docker-compose.yml`.

---

## 📦 Available Setups

| Folder | Description |
|--------|-------------|
| [`keycloak-with-nginx`](./keycloak-with-nginx) | A single Keycloak instance behind an nginx reverse proxy with virtual hosts. Supports custom themes. |
| [`keycloak-cluster-with-jdbc-ping`](./keycloak-cluster-with-jdbc-ping) | A basic Keycloak HA setup with two nodes using JDBC_PING for cluster discovery and nginx as a load balancer. |
| [`keycloak-cluster-with-tcp-ping`](./keycloak-cluster-with-tcp-ping) | A basic Keycloak HA setup with two nodes using TCPPING for cluster discovery and nginx as a load balancer. Deprecated since Keycloak 26.  |
---

## 🧰 Prerequisites

- [Docker](https://www.docker.com/)
- [Docker Compose](https://docs.docker.com/compose/)
- Optional: Custom themes for Keycloak (`/keycloak/themes`)

---

## 🚀 Getting Started

Each subfolder includes its own `README.md` and can be run independently.

```bash
cd keycloak-with-nginx        # or keycloak-cluster-with-jdbc-ping
cp .env.example .env          # if provided
docker-compose up -d
```

For domain-based routing, be sure to update your `/etc/hosts` file as described in the individual setups.

---

## 🧪 What's Included

✅ Keycloak  
✅ PostgreSQL  
✅ Reverse proxies (nginx)  
✅ Custom themes (optional)  
✅ Cluster discovery (via JDBC_PING)

---

## 📚 Resources

- 🔗 [Keycloak Documentation](https://www.keycloak.org/documentation.html)
- 🔗 [Keycloak Docker Hub](https://hub.docker.com/r/keycloak/keycloak)

---

## ⭐ Support the Project

If this project saves you time or helps you learn something new, **consider giving it a star** ⭐  
Your support helps others discover it and encourages future improvements!