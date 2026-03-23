# Home Lab Media Server Setup

A fully containerized home media server built on two rack-mounted nodes — a TrueNAS SCALE NAS for storage and an Ubuntu App Server for running all Docker services with GPU-accelerated transcoding.

## Documentation

| Document | Description |
| :--- | :--- |
| [ARCHITECTURE.md](ARCHITECTURE.md) | Network diagram, Docker services, and design decisions |
| [SETUP.md](SETUP.md) | Step-by-step build and deployment guide |
| [COMPONENTS.md](COMPONENTS.md) | Hardware inventory and component tracker |

## Quick Reference — Service Ports

| Service | Port | Local URL | Docs |
| :--- | :--- | :--- | :--- |
| **Nginx Proxy Manager** | `81` | `http://<server-ip>:81` | [nginxproxymanager.com](https://nginxproxymanager.com/guide/) |
| **Portainer** | `9000` | `http://<server-ip>:9000` | [docs.portainer.io](https://docs.portainer.io/) |
| **Homepage** | `3001` | `http://<server-ip>:3001` | [gethomepage.dev](https://gethomepage.dev/) |
| **Uptime Kuma** | `3002` | `http://<server-ip>:3002` | [github.com/louislam/uptime-kuma](https://github.com/louislam/uptime-kuma/wiki) |
| **Jellyfin** | `8096` | `http://<server-ip>:8096` | [jellyfin.org/docs](https://jellyfin.org/docs/) |
| **Seerr** | `5055` | `http://<server-ip>:5055` | [github.com/seerr-team/seerr](https://github.com/seerr-team/seerr) |
| **Bazarr** | `6767` | `http://<server-ip>:6767` | [wiki.bazarr.media](https://wiki.bazarr.media/) |
| **qBittorrent** | `8080` | `http://<server-ip>:8080` | [github.com/qbittorrent/qBittorrent/wiki](https://github.com/qbittorrent/qBittorrent/wiki) |
| **Prowlarr** | `9696` | `http://<server-ip>:9696` | [wiki.servarr.com/prowlarr](https://wiki.servarr.com/prowlarr) |
| **Radarr** | `7878` | `http://<server-ip>:7878` | [wiki.servarr.com/radarr](https://wiki.servarr.com/radarr) |
| **Sonarr** | `8989` | `http://<server-ip>:8989` | [wiki.servarr.com/sonarr](https://wiki.servarr.com/sonarr) |
| **Lidarr** | `8686` | `http://<server-ip>:8686` | [wiki.servarr.com/lidarr](https://wiki.servarr.com/lidarr) |
| **FlareSolverr** | `8191` | `http://<server-ip>:8191` | [github.com/FlareSolverr/FlareSolverr](https://github.com/FlareSolverr/FlareSolverr) |
| **Vaultwarden** | `8888` | `http://<server-ip>:8888` | [github.com/dani-garcia/vaultwarden/wiki](https://github.com/dani-garcia/vaultwarden/wiki) |
| **Ntfy** | `2586` | `http://<server-ip>:2586` | [docs.ntfy.sh](https://docs.ntfy.sh/) |
| **Grafana** | `3000` | `http://<server-ip>:3000` | [grafana.com/docs](https://grafana.com/docs/grafana/latest/) |
| **Prometheus** | `9090` | `http://<server-ip>:9090` | [prometheus.io/docs](https://prometheus.io/docs/introduction/overview/) |
| **Authelia** | `9091` | `http://<server-ip>:9091` | [authelia.com/docs](https://www.authelia.com/configuration/prologue/introduction/) |
| **Duplicati** | `8200` | `http://<server-ip>:8200` | [duplicati.readthedocs.io](https://duplicati.readthedocs.io/) |
| **Pi-hole Admin** | `80` | `http://<pi-ip>/admin` | [docs.pi-hole.net](https://docs.pi-hole.net/) |

---

## Changelog

### v2.2 — Expanded Service Stack & Upgrade Planning
- **[New]** Added **Homepage** — customizable dashboard with live widgets for all services.
- **[New]** Added **Uptime Kuma** — service uptime monitoring and alerting.
- **[New]** Added **Watchtower** — daily container image scan in monitor-only mode (no auto-updates).
- **[New]** Added **Bazarr** — automatic subtitle management, integrated with Sonarr and Radarr.
- **[New]** Added **FlareSolverr** — Cloudflare bypass proxy for Prowlarr indexers.
- **[New]** Added **Vaultwarden** — self-hosted Bitwarden-compatible password manager.
- **[New]** Added **Ntfy** — self-hosted push notification server; receives alerts from Uptime Kuma and Watchtower.
- **[New]** Added Pi-hole setup guide (Raspberry Pi 5) to SETUP.md Phase 0 — replaces router DNS as local name resolution.
- **[New]** Added Upgrade Path section to COMPONENTS.md — documents planned switch, gaming desktop mobo/RAM, and router upgrades.
- **[Update]** SETUP.md Phase 0 restructured from 3 steps to 4, with Pi-hole as the DNS layer.
- **[Update]** SETUP.md Phase 8 wiring expanded with Bazarr and FlareSolverr connection steps.
- **[Update]** SETUP.md Phases 9–13 added covering all new services.
- **[Update]** `.env` template updated with `VAULTWARDEN_ADMIN_TOKEN` and `NTFY_BASE_URL`.

### v2.1 — Seerr Migration
- **[Update]** Migrated from **Jellyseerr** to **Seerr** (`ghcr.io/seerr-team/seerr:latest`). Jellyseerr was deprecated 2026-02-16; Seerr is the unified successor to both Jellyseerr and Overseerr. Port (5055) and config path are unchanged — existing data migrates automatically on first run.

### v2.0 — Major Enhancements
- **[Fix]** Added `depends_on` with `condition: service_healthy` on qBittorrent → Gluetun. qBittorrent will no longer start until the VPN tunnel is confirmed active, closing a potential IP leak window at container startup.
- **[Fix]** Removed deprecated `version: "3.8"` top-level key from `docker-compose.yml`.
- **[Fix]** Added container `healthcheck` definitions for all services that expose an HTTP endpoint.
- **[New]** Added **Nginx Proxy Manager** — single HTTPS entry point with automatic Let's Encrypt certificate management.
- **[New]** Added **Authelia** — SSO and 2FA authentication portal.
- **[New]** Added **Unpackerr** — automatically extracts `.rar`/`.zip` archives after download completes.
- **[New]** Added **Recyclarr** — automatically syncs TRaSH Guides quality profiles to Radarr and Sonarr.
- **[New]** Added **Jellyseerr** — media request portal for Jellyfin users. (Migrated to **Seerr** in v2.1)
- **[New]** Added **Prometheus + Grafana + Node Exporter** — full-stack monitoring.
- **[New]** Added **Duplicati** — encrypted, scheduled backup of all container config directories.
- **[New]** Split documentation into README, ARCHITECTURE, SETUP, and COMPONENTS.

### v1.0 — Initial Release
- Docker Compose stack with Portainer, Tailscale, Gluetun, qBittorrent, Prowlarr, Radarr, Sonarr, Lidarr, and Jellyfin.
- NAS integration via NFS mounts.
- Nvidia GPU passthrough for Jellyfin hardware transcoding.
