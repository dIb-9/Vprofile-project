# Vprofile Project — Multi-Tier Containerized Web Application

A fully containerized, multi-service Java web application demonstrating a production-style architecture using Docker and Docker Compose. Built as part of a hands-on DevOps learning path to understand service orchestration, caching strategies, and message queuing in a real application stack.

---

## Architecture Overview

The application is composed of 5 independent services, each running in its own container:

| Service | Technology | Role |
|---|---|---|
| **vproweb** | Nginx | Reverse proxy — routes incoming traffic to the app layer |
| **vproapp** | Tomcat (Java) | Application server — handles business logic and user requests |
| **vprodb** | MySQL 8 | Persistent relational database — stores user profile data |
| **vprocache01** | Memcached | In-memory caching layer — serves repeated requests without hitting the DB |
| **vpromq01** | RabbitMQ | Message broker — handles async operations and background task queuing |

### Request Flow
```
User → Nginx (port 80) → Tomcat App → Check Memcache
                                         ├── Cache HIT  → Return data from cache
                                         └── Cache MISS → Query MySQL → Store in cache → Return data
```

---

## Key Features & What This Demonstrates

- **Multi-container orchestration** with Docker Compose — all 5 services defined, networked, and started with a single command
- **Caching layer in action** — first request fetches from MySQL and inserts into Memcached; subsequent requests are served directly from cache (visible in UI)
- **Persistent data volumes** — MySQL and Tomcat use named volumes to survive container restarts
- **Custom Dockerfiles** per service — app, db, and web layers each have tailored build configurations
- **Message queuing** — RabbitMQ configured for async task handling between services
- **Reverse proxy setup** — Nginx configured to forward requests to the Tomcat backend

---

## Tech Stack

- **Java / Maven** — Application build
- **Tomcat** — Java application server
- **MySQL 8** — Relational database
- **Memcached** — Distributed memory caching
- **RabbitMQ** — Message broker (AMQP)
- **Nginx** — Web server / reverse proxy
- **Docker & Docker Compose** — Containerization and orchestration

---

## Project Structure

```
Vprofile-project/
├── Docker-files/
│   ├── app/Dockerfile        # Tomcat app container
│   ├── db/Dockerfile         # MySQL container with schema
│   │   └── db_backup.sql     # Initial database schema & seed data
│   └── web/Dockerfile        # Nginx reverse proxy container
│       └── nginvproapp.conf  # Nginx configuration
├── src/                      # Java application source code
│   └── main/
│   └── test/
├── compose.yaml              # Docker Compose — full stack definition
├── pom.xml                   # Maven build configuration
└── README.md
```

---

## Prerequisites

- Docker & Docker Compose installed
- JDK 11
- Maven 3
- MySQL 8 (only needed if running outside Docker)

---

## How to Run

**Clone the repository:**
```bash
git clone https://github.com/dIb-9/Vprofile-project.git
cd Vprofile-project
```

**Build and start all services:**
```bash
docker compose up -d --build
```

**Access the application:**
```
http://localhost:80
```

**Check RabbitMQ status:**
```
http://localhost/user/rabbit
```

**Stop all services:**
```bash
docker compose down
```

---

## Caching Behavior

One of the key learning outcomes of this project is observing the caching layer in action:

1. First visit to a user profile → banner shows **"Data is From DB and Data Inserted In Cache"**
2. Subsequent visits → banner shows **"Data is From Cache"**

This demonstrates how Memcached reduces database load by serving repeated reads from memory.

---

## Challenges & Learnings

- Configuring inter-container communication and service dependencies in Docker Compose
- Understanding how a Java app connects to MySQL, Memcached, and RabbitMQ simultaneously
- Writing custom Dockerfiles for different service types (compiled Java app vs. config-only containers)
- Managing persistent data with named volumes across container restarts
- Debugging connection issues between services using container logs and network inspection

---

## Author

**Muhammad Ibrahim Rashad**  
[GitHub: dIb-9](https://github.com/dIb-9) | [LinkedIn](https://www.linkedin.com/in/mohamed-i-rashad)

*Part of an ongoing DevOps Engineering learning path covering Linux, Docker, Kubernetes, CI/CD, and Cloud (AWS/Azure).*
