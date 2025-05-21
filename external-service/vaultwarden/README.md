# 🔐 Vaultwarden (Bitwarden RS) Deployment

This setup provides a secure, self-hosted password management service using **Vaultwarden**, reverse proxied by **Traefik** with automatic HTTPS and hardened security settings.

---

## 📦 Features

- 🔐 HTTPS with Let's Encrypt via Traefik
- 🚫 Signups disabled by default
- 🛡️ Admin access protected with token
- 📡 WebSocket support enabled
- 📁 Data persisted in Docker volume
- 🔐 Secure headers and auth middleware supported

---

## 📁 Project Structure

```bash
vaultwarden/
├── docker-compose.yml
├── .env
└── README.md
````

---

## ⚙️ Configuration

### Environment Variables

| Variable      | Description                                    |
| ------------- | ---------------------------------------------- |
| `DOMAIN_ADDR` | Base domain (e.g., `service.harimi.ir`)        |
| `VW_SUB`      | Subdomain used for Vaultwarden (e.g., `vault`) |
| `ADMIN_TOKEN` | Token required to access the admin panel       |

---

## 🔧 Traefik Integration

* Routes:

  * `https://vault.service.harimi.ir` → Vaultwarden UI
* Middleware:

  * Redirects HTTP → HTTPS
  * Adds optional secure headers (via `secure@file`)
  * BasicAuth (if enabled with `web-auth`)

---

## 🚀 Usage

### Start the container

```bash
docker-compose up -d
```

### Access Vaultwarden

* User portal: `https://vault.service.harimi.ir`
* Admin panel: `https://vault.service.harimi.ir/admin?token=your-admin-token`

> 🔐 Change `ADMIN_TOKEN` in `.env` to secure access.

---

## 🧯 Backup & Recovery

* All data is stored in the `vw-data` volume.
* To backup:

  ```bash
  docker run --rm -v vaultwarden_vw-data:/data busybox tar czf - /data > backup.tar.gz
  ```

---

## 🛡️ Security Tips

* Enable middleware to enforce secure headers.
* Disable `/admin` route in Traefik if not used.
* Use monitoring tools to track uptime and login attempts.
* Use `docker secret` or `.env` vaulting to secure credentials.

---

## 📚 Resources

* [Vaultwarden GitHub](https://github.com/dani-garcia/vaultwarden)
* [Traefik Middleware Docs](https://doc.traefik.io/traefik/middlewares/overview/)
* [Traefik TLS Docs](https://doc.traefik.io/traefik/https/tls/)


