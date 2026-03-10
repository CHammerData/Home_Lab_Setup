# Home Lab Media Server Setup

A fully containerized home media server built on two rack-mounted nodes — a TrueNAS SCALE NAS for storage and an Ubuntu App Server for running all Docker services with GPU-accelerated transcoding.

## Documentation

| Document | Description |
| :--- | :--- |
| [ARCHITECTURE.md](ARCHITECTURE.md) | Network diagram, Docker services, and design decisions |
| [SETUP.md](SETUP.md) | Step-by-step build and deployment guide |
| [COMPONENTS.md](COMPONENTS.md) | Hardware inventory and component tracker |

## Quick Reference — Service Ports

| Service | Port | URL |
| :--- | :--- | :--- |
| **Nginx Proxy Manager** | `81` | `http://<server-ip>:81` |
| **Portainer** | `9000` | `http://<server-ip>:9000` |
| **Jellyfin** | `8096` | `http://<server-ip>:8096` |
| **Jellyseerr** | `5055` | `http://<server-ip>:5055` |
| **qBittorrent** | `8080` | `http://<server-ip>:8080` |
| **Prowlarr** | `9696` | `http://<server-ip>:9696` |
| **Radarr** | `7878` | `http://<server-ip>:7878` |
| **Sonarr** | `8989` | `http://<server-ip>:8989` |
| **Lidarr** | `8686` | `http://<server-ip>:8686` |
| **Grafana** | `3000` | `http://<server-ip>:3000` |
| **Prometheus** | `9090` | `http://<server-ip>:9090` |
| **Authelia** | `9091` | `http://<server-ip>:9091` |
| **Duplicati** | `8200` | `http://<server-ip>:8200` |

---

## Changelog

### v2.0 — Major Enhancements
- **[Fix]** Added `depends_on` with `condition: service_healthy` on qBittorrent → Gluetun. qBittorrent will no longer start until the VPN tunnel is confirmed active, closing a potential IP leak window at container startup.
- **[Fix]** Removed deprecated `version: "3.8"` top-level key from `docker-compose.yml`.
- **[Fix]** Added container `healthcheck` definitions for all services that expose an HTTP endpoint.
- **[New]** Added **Nginx Proxy Manager** — single HTTPS entry point with automatic Let's Encrypt certificate management.
- **[New]** Added **Authelia** — SSO and 2FA authentication portal.
- **[New]** Added **Unpackerr** — automatically extracts `.rar`/`.zip` archives after download completes.
- **[New]** Added **Recyclarr** — automatically syncs TRaSH Guides quality profiles to Radarr and Sonarr.
- **[New]** Added **Jellyseerr** — media request portal for Jellyfin users.
- **[New]** Added **Prometheus + Grafana + Node Exporter** — full-stack monitoring.
- **[New]** Added **Duplicati** — encrypted, scheduled backup of all container config directories.
- **[New]** Split documentation into README, ARCHITECTURE, SETUP, and COMPONENTS.

### v1.0 — Initial Release
- Docker Compose stack with Portainer, Tailscale, Gluetun, qBittorrent, Prowlarr, Radarr, Sonarr, Lidarr, and Jellyfin.
- NAS integration via NFS mounts.
- Nvidia GPU passthrough for Jellyfin hardware transcoding.
