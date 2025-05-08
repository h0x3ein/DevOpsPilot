# 🚦 Traefik Reverse Proxy Setup

This project configures a **Traefik v2.10.4** reverse proxy using Docker Compose, enabling secure routing and metrics collection for containerized services with Let's Encrypt TLS, Prometheus metrics, and automatic service discovery.

---

## 📦 Project Structure

- `docker-compose.yml` – Defines the Traefik service with volumes, networks, and labels.
- `.env` – Stores environment variables used in the compose file.
- `web_net` – External Docker network used for service communication.

---

## 🌐 Environment Variables

Defined in `.env` file:

| Variable       | Description                            | Example                |
|----------------|----------------------------------------|------------------------|
| `DOMAIN_ADDR`  | Base domain used across routes         | `service.harimi.ir`    |
| `SUB`          | Subdomain for Traefik dashboard        | `tr`                   |
| `TRA_METRICS`  | Subdomain for metrics endpoint         | `metrics`              |

---

## 🛠️ Configuration Details

### 🔐 TLS and Let's Encrypt

- ACME (Let's Encrypt) is used for automatic HTTPS certificate generation.
- Certificates are stored in a persistent volume `traefik-acme`.

### 📊 Metrics

- Prometheus metrics are exposed via a custom entry point on port `8082`.
- Accessible through a secure route:
```

[https://metrics.service.harimi.ir](https://metrics.service.harimi.ir)

```

### 📋 Dashboard Access

- Traefik dashboard is available at:
```

[https://tr.service.harimi.ir](https://tr.service.harimi.ir)

````

> Dashboard access should be protected. For production use, consider enabling authentication middleware (e.g., `basicAuth`).

---

## 🚀 Running the Project

1. **Ensure the external network exists**:
 ```bash
 docker network create web_net
````

2. **Start Traefik**:

   ```bash
   docker-compose up -d
   ```

3. **Verify health**:

   ```bash
   docker inspect --format='{{json .State.Health}}' traefik
   ```

---

## 📁 Volumes and Logs

* Traefik stores Let's Encrypt data in `traefik-acme` volume.
* Access logs and error logs are written to `/log-file.log` in JSON format.

---

## 🔧 Service Exposure Example

To expose a container via Traefik, attach labels like the following:

```yaml
labels:
  - traefik.enable=true
  - traefik.http.routers.myapp.rule=Host(`app.service.harimi.ir`)
  - traefik.http.routers.myapp.entrypoints=https
  - traefik.http.routers.myapp.tls.certresolver=mycert
  - traefik.docker.network=web_net
```

---

## 📈 Monitoring

Prometheus can scrape metrics at:

```
https://metrics.service.harimi.ir/metrics
```

Make sure your Prometheus is configured with proper TLS settings.

---

## ✅ Healthcheck

The container includes a health check using Traefik's `/ping` endpoint for robust monitoring:

```bash
wget --quiet --tries=1 --spider http://localhost:8080/ping
```

---

## 🔒 Security Recommendations

* Disable `--api.insecure=true` in production.
* Replace insecure dashboard access with:

  * BasicAuth
  * OAuth2 proxy (e.g., via Keycloak)

---

## 📚 Resources

* [Traefik Documentation](https://doc.traefik.io/traefik/)
* [Traefik Docker Provider](https://doc.traefik.io/traefik/providers/docker/)
* [Traefik TLS Configuration](https://doc.traefik.io/traefik/https/tls/)
* [Traefik Prometheus Metrics](https://doc.traefik.io/traefik/observability/metrics/prometheus/)

---

## 🧑‍💻 Author

This configuration is maintained by the DevOps team at **harimi.ir**.
