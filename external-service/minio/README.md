# 🗂️ MinIO Deployment with Traefik

This setup deploys [MinIO](https://min.io/), a high-performance, S3-compatible object storage server, reverse-proxied via **Traefik** with HTTPS and dual endpoints (console and API).

---

## 🚀 Features

- 🔐 **Secure access via HTTPS**
- 🌍 Accessible at two subdomains:
  - **Console**: `https://object.service.harimi.ir`
  - **API (S3 endpoint)**: `https://io.service.harimi.ir`
- 🔄 Automatic Let's Encrypt TLS certificate
- 📊 Health check enabled
- 🧠 Runs on Docker with external networks (`web_net`, `app_net`)

---

## 📁 Project Structure

```

minio/
├── docker-compose.yml
├── .env
└── README.md

````

---

## ⚙️ Configuration

### 🔐 `.env` File

```env
# Domain and subdomains
DOMAIN_ADDRESS=service.harimi.ir
MINIO_SUB1=object          # For console UI
MINIO_SUB2=io              # For S3 API

# Restart policy
RESTART_POLICY=unless-stopped

# Admin credentials
MINIO_ROOT_USER=minioadmin
MINIO_ROOT_PASSWORD=S3cur3P@ssw0rd!

# Optional
HOSTNAME=repo-server
````

> ✅ Use secure, unique credentials in production!

---

## 🌐 URLs

| Purpose  | Subdomain                          | Port | Notes         |
| -------- | ---------------------------------- | ---- | ------------- |
| Console  | `https://object.service.harimi.ir` | 9001 | Admin UI      |
| API (S3) | `https://io.service.harimi.ir`     | 9000 | S3-compatible |

---

## 🛠️ Deployment Instructions

1. Make sure the external Docker networks exist:

```bash
docker network create web_net
docker network create app_net
```

2. Start the container:

```bash
docker-compose up -d
```

3. Access your MinIO console at:

```
https://object.service.harimi.ir
```

Login with the credentials from `.env`.

---

## 🔐 TLS/HTTPS via Traefik

This config uses **Traefik** as a reverse proxy to:

* Redirect all HTTP to HTTPS
* Obtain a TLS certificate from **Let's Encrypt**
* Serve both MinIO console and API securely

Make sure your Traefik instance:

* Has `certresolver` named `mycert`
* Mounts the Docker socket
* Is on the `web_net`

---

## 🧪 Health Check

MinIO is monitored via:

```
http://localhost:9000/minio/health/live
```

This helps Docker automatically restart if MinIO becomes unresponsive.

---

## 🧰 Admin & Tools

* Console UI → for managing buckets, policies, etc.
* API → compatible with AWS CLI, s3cmd, mc, etc.

Example with [MinIO Client (mc)](https://docs.min.io/docs/minio-client-quickstart-guide.html):

```bash
mc alias set myminio https://io.service.harimi.ir minioadmin S3cur3P@ssw0rd!
mc ls myminio
```

---

## 📦 Volumes

| Name         | Purpose            |
| ------------ | ------------------ |
| `minio_data` | Persistent storage |

---

## 🧱 Networking

This service connects to:

* `web_net` → for Traefik routing
* `app_net` → for communication with other internal apps (e.g., backend services)

---

## 🧯 Tips & Security

* Change `MINIO_ROOT_PASSWORD` before exposing to the internet
* Protect Traefik dashboard and metrics with authentication
* Use firewall rules to restrict API usage if necessary
* Set resource limits if running in production environments

---

## 📚 References

* [MinIO Docs](https://docs.min.io/)
* [Traefik Docs](https://doc.traefik.io/)
* [Docker Compose](https://docs.docker.com/compose/)


