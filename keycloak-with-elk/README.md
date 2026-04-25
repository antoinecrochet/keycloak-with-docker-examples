# 🔐 Keycloak with ELK Stack and Filebeat

This example demonstrates how to run a **Keycloak server with PostgreSQL** and ship its logs to the **Elastic Stack (Elasticsearch + Logstash + Kibana)** using **Filebeat**.

> ⚠️ **Note**: `xpack.security.enabled` is set to `false` for all Elastic components in this setup to simplify local development. **Do not use this configuration in production environments.**

---

## 📁 Folder Structure

```
keycloak-with-elk/
├── docker-compose.yml
├── .env
├── keycloak/
│   └── log/                    # Keycloak log volume
├── postgres/
│   └── data/                   # PostgreSQL data volume
├── elasticsearch/
│   └── data/                   # Elasticsearch data volume
├── logstash/
│   ├── logstash.conf           # Logstash pipeline config
│   ├── logstash.yml            # Logstash system config
│   └── data/                   # Logstash persistent storage
├── filebeat/
│   └── filebeat.yml            # Filebeat config (filestream input)
```

---

## 🧰 Services Overview

| Service             | Description                                       |
| ------------------- | ------------------------------------------------- |
| `keycloak`          | Keycloak 26.6 instance with PostgreSQL backend    |
| `keycloak-database` | PostgreSQL 17 for Keycloak                        |
| `filebeat`          | Collects logs from Keycloak and ships to Logstash |
| `logstash`          | Parses and forwards logs to Elasticsearch         |
| `elasticsearch`     | Stores logs and supports full-text search         |
| `kibana`            | Web UI for exploring and visualizing logs         |

---

## 🛠️ Setup Instructions

### 1. Clone the Repository

```bash
git clone https://github.com/your-repo/keycloak-with-elk.git
cd keycloak-with-elk
```

### 2. Create a `.env` File

```env
POSTGRES_USER=keycloak
POSTGRES_PASSWORD=changeme
POSTGRES_DB=keycloak

STACK_VERSION=9.0.0
MEM_LIMIT=1g

KEYCLOAK_PORT=8080
ES_PORT=9200
KIBANA_PORT=5601
```

> 🔐 You should change these defaults for production environments.

---

### 3. Start the Stack

```bash
docker compose up -d
```

> Use `docker compose logs -f` to monitor logs for any startup issues.

---

## 🌍 Access Points

| URL                     | Description       |
| ----------------------- | ----------------- |
| `http://localhost:8080` | Keycloak login    |
| `http://localhost:5601` | Kibana dashboard  |
| `http://localhost:9200` | Elasticsearch API |

---

## 🧾 Admin Credentials

| App      | Username | Password                                    |
| -------- | -------- | ------------------------------------------- |
| Keycloak | admin    | admin *(prompted to change on first login)* |

---

## 📊 Viewing Keycloak Logs in Kibana

1. Open **Kibana** at [http://localhost:5601](http://localhost:5601)
2. Go to **Discover**
3. Select the keycloak index (`keycloak-*`)

---

## 🧪 Testing Log Ingestion

You can check that logs are flowing by:

```bash
curl http://localhost:9200/_cat/indices?v
```

And tailing logs:

```bash
docker compose logs -f filebeat logstash
```

---

## 🧼 Cleanup

```bash
docker compose down -v
```

This stops all containers and removes named volumes.