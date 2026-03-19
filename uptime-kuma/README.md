![Docker](https://img.shields.io/badge/Docker-29.3+-blue?logo=docker)
![Uptime Kuma](https://img.shields.io/badge/Uptime_Kuma-1.x-green)
![Docker Compose](https://img.shields.io/badge/Docker_Compose-v2-blue?logo=docker)
![License](https://img.shields.io/badge/License-MIT-lightgrey)
![Status](https://img.shields.io/badge/Status-Done-brightgreen)

# 🟢 uptime-kuma — Self-Hosted Availability Monitoring

Uptime Kuma deployed via Docker Compose — a self-hosted monitoring tool that checks
the availability of services (websites, ports, DNS, ping) every 60 seconds and alerts
when something goes down.

>  Goal: Learn Docker Compose, volumes, healthchecks, and understand availability
> monitoring as used in Helpdesk and NOC environments.



---

##  How It Works

```
Uptime Kuma (every 60s)
        ↓
  HTTP  →  Is the site responding with 200?
  TCP   →  Is the port open?
  DNS   →  Is the DNS resolving correctly?
  Ping  →  Is the host reachable?
        ↓
  Store result + response time
        ↓
  Dashboard — Green / Red + history
```

---

##  What Data Is Collected

-  Response time (ms) per check
-  Uptime % over 24h and 30 days
-  Full incident history with timestamps
-  SSL certificate expiration date
-  Timestamped logs of every check





##  Project Structure

```
uptime-kuma/
└── docker-compose.yml    # Stack definition
```

Data is persisted in a Docker-managed volume: `uptime-kuma-data`

---

##  docker-compose.yml explained

```yaml
services:
  uptime-kuma:
    image: louislam/uptime-kuma:1   # pinned version (more stable than :latest)
    container_name: uptime-kuma
    restart: unless-stopped          # restarts automatically UNLESS manually stopped
    ports:
      - "3001:3001"                  # host:container port mapping
    volumes:
      - uptime-kuma-data:/app/data   # persistent storage managed by Docker
    environment:
      - TZ=Europe/Brussels           # timezone for correct alert timestamps
    networks:
      - kuma_network                 # isolated custom network
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:3001"]
      interval: 30s                  # check every 30s
      retries: 3                     # mark unhealthy after 3 failures
      start_period: 10s
      timeout: 5s
    logging:
      driver: "json-file"
      options:
        max-size: "10m"              # max 10MB per log file
        max-file: "3"                # keep last 3 log files

volumes:
  uptime-kuma-data:                  # Docker-managed persistent volume

networks:
  kuma_network:
    driver: bridge
```

---

## 🚀 Usage

### Start the stack
```bash
docker compose up -d
```

### Check status
```bash
docker compose ps
# STATUS should show: Up X minutes (healthy)
```

### View logs
```bash
docker compose logs -f
```

### Access the dashboard
Open your browser: **http://localhost:3001**

### Stop the stack
```bash
docker compose down
```

### Stop without removing data
```bash
docker compose stop
```

---

##  Key concepts learned

- `docker compose up/down` — manage a multi-container stack
- **Named volumes** — persistent data that survives container restarts
- **Healthcheck** — Docker automatically monitors container health
- **Custom networks** — isolate containers from each other
- **Log rotation** — prevent logs from filling up disk
- `restart: unless-stopped` vs `restart: always`
- `TZ` environment variable — correct timestamps in logs and alerts


---

##  Tested On

| OS | Docker version |
|----|---------------|
| Win11 (WSL2) | 29.3.0 |

---

## 👤 Author

**Mickaël Paquet** — Junior Cybersecurity   
[LinkedIn](https://www.linkedin.com/in/mickael-paquet7a0638312) · [GitHub](https://github.com/MickaxL)
