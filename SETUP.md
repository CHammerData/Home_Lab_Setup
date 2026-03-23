# Setup Guide

This guide walks through the full build and deployment from bare hardware to a running stack. Complete each phase in order.

---

## Phase 0: Network / Router Setup

Do this before anything else. The goal is to ensure the NAS and App Server always get the same IP address after a reboot, and that you can reach services by name instead of memorizing IPs.

### Step 1 — DHCP Reservations

Log in to your router and create a DHCP reservation for each server by MAC address. This pins each machine to a fixed IP without requiring a static IP configured on the OS itself (easier to manage and survives reinstalls).

| Machine | Suggested IP | Where to find MAC |
| :--- | :--- | :--- |
| NAS (TrueNAS) | e.g. `192.168.1.10` | TrueNAS web UI → **Network > Interfaces**, or check your router's current lease table |
| App Server (Ubuntu) | e.g. `192.168.1.20` | `ip link show` on the server, or your router's lease table |
| Raspberry Pi (Pi-hole) | e.g. `192.168.1.5` | Shown at the bottom of the Pi-hole install output, or via `hostname -I` after first boot |

Once saved, reboot each machine and confirm they come back on the expected IPs before continuing.

> **Why not static IPs on the OS?** Static IPs set at the OS level bypass DHCP entirely, which means your router's lease table won't show them and you lose the router as a single source of truth. DHCP reservations give you the same stability with less friction.

### Step 2 — Pi-hole Setup (Raspberry Pi 5)

Pi-hole is your network-wide DNS server. It blocks ads and trackers for every device on the network, and — more importantly for your lab — it handles local DNS so every device can resolve hostnames like `truenas.home` without touching each machine's hosts file.

1. **Flash Raspberry Pi OS Lite (64-bit)** to an SD card or USB SSD using [Raspberry Pi Imager](https://www.raspberrypi.com/software/). Before writing, click **Edit Settings** to pre-configure:
    - **Hostname:** `pihole`
    - **Username / Password:** set your credentials
    - **Enable SSH:** checked
    - **Wi-Fi:** leave blank if using wired Ethernet (recommended — a DNS outage takes down your whole network)
2. **Boot and SSH in:**
    ```bash
    ssh <username>@pihole.local
    ```
3. **Update the system:**
    ```bash
    sudo apt update && sudo apt upgrade -y
    ```
4. **Install Pi-hole:**
    ```bash
    curl -sSL https://install.pi-hole.net | bash
    ```
    During the interactive installer:
    - Select your network interface (`eth0` for wired)
    - Select an upstream DNS provider (Cloudflare `1.1.1.1` or Quad9 `9.9.9.9` are solid choices)
    - Enable the web admin interface
    - Enable query logging
    - **Note the admin password** shown at the end — or set your own with `pihole -a -p`
5. **Verify the Pi's IP** matches the DHCP reservation from Step 1:
    ```bash
    hostname -I
    ```
6. **Point your router's DHCP to Pi-hole:** In your Asus router go to **LAN > DHCP Server** and set **DNS Server 1** to the Pi's reserved IP (`192.168.1.5`). Leave DNS Server 2 blank or set `1.1.1.1` as a fallback. Apply and reboot the router.

> **Note:** After pointing DHCP to Pi-hole, new leases will resolve through Pi-hole immediately. Existing devices may retain the old DNS until their lease renews — force a renewal on Windows with `ipconfig /release && ipconfig /renew`.

### Step 3 — Local DNS Records (Pi-hole)

With Pi-hole running, add local DNS entries so every device on the network can reach your lab machines by name.

1. Open Pi-hole admin at `http://pihole.local/admin` (or `http://192.168.1.5/admin`).
2. Go to **Local DNS > DNS Records** and add one entry per machine:

| Domain | IP Address |
| :--- | :--- |
| `truenas.home` | `192.168.1.10` |
| `appserver.home` | `192.168.1.20` |
| `pihole.home` | `192.168.1.5` |

Individual Docker services (Jellyfin, Grafana, Portainer, etc.) are accessed via Nginx Proxy Manager. Once NPM is set up in Phase 4, add one DNS entry per service pointing to `192.168.1.20` and let NPM route by hostname — no need to add every container's port to DNS.

> **Hosts file fallback:** If you need DNS resolution on a machine before Pi-hole is live, temporarily add entries to `C:\Windows\System32\drivers\etc\hosts` (Windows) or `/etc/hosts` (Linux/Mac). Remove them once Pi-hole is resolving correctly — stale hosts file entries override DNS and cause hard-to-debug issues.

### Step 4 — Record Your IPs

Note the reserved IPs here — you'll substitute them for `<NAS_IP>`, `<PI_IP>`, and `<server-ip>` placeholders throughout the rest of this guide:

```
NAS_IP=192.168.1.10
APP_SERVER_IP=192.168.1.20
PI_IP=192.168.1.5
```

---

## Phase 1: NAS Setup (TrueNAS SCALE)

1. **Configure BIOS:** Before installing the OS, enter the ASRock BIOS (Delete at POST) and set the fan headers for the two front case fans — **CHA_FAN1** and **CHA_FAN2** — to **System** fan mode rather than CPU fan mode. Without this the board may throw a fan-stop warning when the front fans spin down at low RPM.
2. **Install OS:** Install TrueNAS SCALE to the dedicated M.2 NVMe boot SSD — **not** to any of the IronWolf HDDs. During the installer, select only the M.2 drive as the boot device. TrueNAS SCALE 24.10+ will generate an active alert if it detects a USB device as the boot pool; a dedicated internal SSD avoids this entirely.
3. **Create Storage Pool:** Create a ZFS pool with RAID-Z1 across your 4 drives.
4. **Create Datasets:** Create datasets for media and downloads (e.g., `/mnt/pool/media`, `/mnt/pool/downloads`).
5. **Enable Shares:** Turn on the NFS service for the App Server to mount. Optionally enable SMB for Windows access.
6. **Configure UPS (NUT Master):** Connect the CyberPower CP1500PFCRM2U USB cable to the NAS. In TrueNAS SCALE go to **System Settings → Services → UPS** and configure:
    - **Identifier:** `cyberpower`
    - **Driver:** `usbhid-ups` (correct HID driver for CyberPower USB models)
    - **Port:** `auto`
    - **Mode:** `Master`
    - **Monitor Password:** set a strong password — you will need this for client machines
    - **Shutdown Timer:** `30` seconds (grace period after low-battery signal before shutdown)
    - **Power Off UPS:** enabled (cuts UPS power after NAS shuts down, preventing restart on power restore)
    - Enable **Remote Monitor** so client machines (App Server, Desktop) can connect
    - Start and enable the UPS service. Verify TrueNAS detects the UPS under **System Settings → Services → UPS → UPS Status**.

---

## Phase 2: App Server Setup (Ubuntu Server)

1. **Configure BIOS:** Before installing the OS, enter the Gigabyte BIOS (Delete at POST) and set the fan headers for the two front case fans — **SYS_FAN1** and **SYS_FAN2** — to **System** fan mode rather than CPU fan mode. Without this the board may throw a fan-stop warning when the front fans spin down at low RPM.
2. **Install OS:** Install Ubuntu Server LTS to **Disk 1 (Samsung 980 PRO 2TB)** with the OpenSSH server enabled for headless management. Disk 2 is left unformatted at install time for use as additional storage or backup staging.
3. **Configure UPS Client (NUT):** Install the NUT client so the App Server shuts down gracefully when the UPS signals low battery:
    ```bash
    sudo apt install nut-client
    ```
    Edit `/etc/nut/upsmon.conf` and add:
    ```
    MONITOR cyberpower@<NAS_IP> 1 upsmon <monitor_password> slave
    SHUTDOWNCMD "/sbin/poweroff"
    ```
    Enable and start the client:
    ```bash
    sudo systemctl enable nut-client && sudo systemctl start nut-client
    ```
4. **Install GPU Drivers:** Install the proprietary Nvidia drivers for the RTX 2070 Super.
5. **Install Docker:** Install Docker Engine and the NVIDIA Container Toolkit to allow containers to access the GPU.
6. **Install Tailscale:**
    ```bash
    curl -fsSL https://tailscale.com/install.sh | sh
    ```
7. **Mount NAS Storage:** Edit `/etc/fstab` to permanently mount the NFS shares:
    ```
    <NAS_IP>:/mnt/pool/media    /mnt/nas/media    nfs auto,nofail,noatime,nolock,intr,tcp,actimeo=1800 0 0
    <NAS_IP>:/mnt/pool/downloads /mnt/nas/downloads nfs auto,nofail,noatime,nolock,intr,tcp,actimeo=1800 0 0
    ```
    Apply and verify:
    ```bash
    sudo mount -a && df -h
    ```

---

## Phase 3: Docker Stack Deployment

1. **Create `.env` file** in the same directory as `docker-compose.yml`:
    ```env
    PUID=1000
    PGID=1000
    TIMEZONE=America/New_York

    # VPN Credentials
    VPN_USER=your_vpn_username
    VPN_PASS=your_vpn_password

    # *Arr API Keys (retrieve from each app's Settings > General after first launch)
    SONARR_API_KEY=your_sonarr_api_key
    RADARR_API_KEY=your_radarr_api_key
    LIDARR_API_KEY=your_lidarr_api_key

    # Grafana credentials
    GRAFANA_USER=admin
    GRAFANA_PASSWORD=your_secure_grafana_password

    # Vaultwarden admin panel token (generate with: openssl rand -base64 48)
    VAULTWARDEN_ADMIN_TOKEN=your_admin_token_here

    # Ntfy base URL — your server's local address, used by other services for push notifications
    NTFY_BASE_URL=http://192.168.1.20:2586
    ```

2. **Update volume paths:** Search `docker-compose.yml` for all `# Update this path` comments and replace placeholders with your NAS mount points:
    - `/path/to/your/nas/downloads` → `/mnt/nas/downloads`
    - `/path/to/your/nas/data` → `/mnt/nas/media`

3. **Verify Prometheus config** exists at `./config/prometheus/prometheus.yml`. The file is included in this repo — no changes needed unless you want to add additional scrape targets.

4. **Deploy the stack:**
    ```bash
    docker compose up -d
    ```

---

## Phase 4: Reverse Proxy & Auth Setup (Nginx Proxy Manager + Authelia)

1. **Initial NPM login:** Open `http://<server-ip>:81`. Default credentials are `admin@example.com` / `changeme`. Change them immediately.
2. **Add Proxy Hosts:** For each service to expose, create a Proxy Host in NPM pointing to `http://<container-name>:<port>` and enable **Force SSL** with a Let's Encrypt certificate.
3. **Configure Authelia:**
    - Create `./config/authelia/configuration.yml` and `./config/authelia/users_database.yml` per the [Authelia documentation](https://www.authelia.com/configuration/prologue/introduction/).
    - In NPM, add the following to the **Advanced** tab of each proxy host you want protected:
        ```nginx
        location /authelia {
            internal;
            set $upstream_authelia http://authelia:9091/api/verify;
            proxy_pass_request_body off;
            proxy_pass $upstream_authelia;
            proxy_set_header Content-Length "";
            proxy_set_header X-Original-URL $scheme://$http_host$request_uri;
        }
        ```
    - Add `auth_request /authelia;` to the relevant location blocks.

---

## Phase 5: Monitoring Setup (Prometheus + Grafana)

1. **Verify metrics collection:** Open Prometheus at `http://<server-ip>:9090` → **Status > Targets**. Confirm `node-exporter` and `prometheus` are both `UP`.
2. **Set up Grafana:**
    - Open `http://<server-ip>:3000` and log in with your `GRAFANA_USER` / `GRAFANA_PASSWORD`.
    - Add a Prometheus data source pointing to `http://prometheus:9090`.
3. **Import dashboards:** Go to **Dashboards > Import** and import by ID:
    - **Node Exporter Full** — ID `1860` (CPU, RAM, disk, network)

---

## Phase 6: Recyclarr Setup

1. **Generate starter config:**
    ```bash
    docker exec -it recyclarr recyclarr config create
    ```
2. **Edit** `./config/recyclarr/recyclarr.yml` with your API keys:
    ```yaml
    sonarr:
      sonarr-main:
        base_url: http://sonarr:8989
        api_key: your_sonarr_api_key

    radarr:
      radarr-main:
        base_url: http://radarr:7878
        api_key: your_radarr_api_key
    ```
3. **Test sync manually:**
    ```bash
    docker exec -it recyclarr recyclarr sync
    ```
4. Recyclarr runs on its internal schedule automatically. Check logs with `docker logs recyclarr`.

---

## Phase 7: Backup Setup (Duplicati)

1. Open Duplicati at `http://<server-ip>:8200`.
2. Create a new backup job:
    - **Source:** `/source` (mapped to `./config` — all container config directories)
    - **Destination:** NAS backup dataset or cloud provider (Backblaze B2, S3, etc.)
    - **Schedule:** Nightly at 2 AM recommended
    - **Encryption:** Enable with a strong passphrase — store it somewhere safe
3. Run a manual backup immediately and verify the restore process works before relying on it.

---

## Phase 8: Post-Deployment Wiring & Validation

### Wiring the *Arr Stack

1. **Prowlarr → Radarr / Sonarr / Lidarr:** In Prowlarr, go to **Settings > Apps** and add each *Arr app using its container name as the hostname (e.g., `http://radarr:7878`). Prowlarr syncs indexers automatically.
2. **qBittorrent → Radarr / Sonarr / Lidarr:** In each *Arr app, go to **Settings > Download Clients** and add qBittorrent pointing to `http://gluetun:8080`.
3. **Seerr → Jellyfin:** Open Seerr at `http://<server-ip>:5055`, follow the setup wizard, sign in with your Jellyfin admin account, then connect Radarr and Sonarr under **Settings > Services**.
4. **Bazarr → Sonarr / Radarr:** In Bazarr go to **Settings > Sonarr** and add Sonarr at `http://sonarr:8989` with your Sonarr API key. Repeat under **Settings > Radarr** at `http://radarr:7878`. Bazarr will then automatically search and download subtitles matching your existing library.
5. **FlareSolverr → Prowlarr:** In Prowlarr go to **Settings > Indexers > Proxies** and add a FlareSolverr entry with URL `http://flaresolverr:8191`. Assign it to any indexer that sits behind Cloudflare — the indexer config page has a **FlareSolverr** dropdown once the proxy is saved.

### Validation Checklist

| Check | Command / Action |
| :--- | :--- |
| **VPN kill-switch** | `docker exec -it gluetun wget -qO- https://ifconfig.me` — must return VPN IP, not home IP |
| **qBittorrent locked to VPN** | `docker stop gluetun` — qBittorrent should immediately lose connectivity |
| **Hardware transcoding** | Play a 4K file in Jellyfin, then run `nvidia-smi` on host — a `jellyfin` process should be using the GPU encoder |
| **HTTPS** | All proxy-exposed services load over HTTPS with a valid certificate |
| **Monitoring** | Grafana dashboards show live data from Node Exporter |
| **Backup** | Trigger a manual Duplicati backup and confirm it completes without errors |
| **Homepage** | Open `http://<server-ip>:3001` — service widgets should populate with live data |
| **Uptime Kuma** | Open `http://<server-ip>:3002` — add monitors and confirm first status checks pass |

---

## Phase 9: Dashboard Setup (Homepage)

Homepage is a static, highly customizable dashboard. It reads YAML config files and displays live stats from your services via their APIs.

1. **Create the config directory and seed files** before starting the container:
    ```bash
    mkdir -p ./config/homepage
    touch ./config/homepage/services.yaml \
          ./config/homepage/widgets.yaml \
          ./config/homepage/bookmarks.yaml \
          ./config/homepage/settings.yaml
    ```
2. **Start the container:**
    ```bash
    docker compose up -d homepage
    ```
3. Open `http://<server-ip>:3001` — an empty dashboard will load.
4. **Configure services** by editing `./config/homepage/services.yaml`. Example starter config:
    ```yaml
    - Media:
        - Jellyfin:
            href: http://jellyfin.home
            icon: jellyfin.png
            description: Media Server
            widget:
              type: jellyfin
              url: http://jellyfin:8096
              key: your_jellyfin_api_key
        - Seerr:
            href: http://seerr.home
            icon: jellyseerr.png
            description: Media Requests
    - Automation:
        - Sonarr:
            href: http://sonarr.home
            icon: sonarr.png
            widget:
              type: sonarr
              url: http://sonarr:8989
              key: your_sonarr_api_key
        - Radarr:
            href: http://radarr.home
            icon: radarr.png
            widget:
              type: radarr
              url: http://radarr:7878
              key: your_radarr_api_key
    - Monitoring:
        - Grafana:
            href: http://grafana.home
            icon: grafana.png
        - Uptime Kuma:
            href: http://uptime.home
            icon: uptime-kuma.png
    ```
5. Changes to YAML files are picked up immediately — no container restart needed. Full documentation at [gethomepage.dev](https://gethomepage.dev/).

---

## Phase 10: Uptime Monitoring (Uptime Kuma)

Uptime Kuma provides a clean status dashboard and push alerts when services go down. It complements Grafana (which shows metrics) by answering the simpler question: is it up right now?

1. Open `http://<server-ip>:3002` and create an admin account on first launch.
2. Add a monitor for each service — use **HTTP(s)** type:

| Monitor | URL | Interval |
| :--- | :--- | :--- |
| Sonarr | `http://sonarr:8989/health` | 60s |
| Radarr | `http://radarr:7878/health` | 60s |
| Lidarr | `http://lidarr:8686/health` | 60s |
| Prowlarr | `http://prowlarr:9696/health` | 60s |
| Jellyfin | `http://jellyfin:8096/health` | 60s |
| Bazarr | `http://bazarr:6767/health` | 60s |
| Grafana | `http://grafana:3000/api/health` | 60s |
| NPM | `http://nginx-proxy-manager:81/api/` | 60s |
| TrueNAS | `http://192.168.1.10/api/v2.0/system/ready` | 120s |

3. Configure a notification channel under **Settings > Notifications**. Connect it to ntfy (Phase 11) once that is running.

---

## Phase 11: Notifications (Ntfy)

Ntfy is a lightweight self-hosted push notification server. Watchtower, Uptime Kuma, and the *arr stack can all route alerts through it, and the ntfy mobile app delivers them to your phone.

1. Confirm ntfy is running: open `http://<server-ip>:2586` — you should see the ntfy web UI.
2. Install the **ntfy mobile app** on your phone (iOS and Android, available at [ntfy.sh](https://ntfy.sh/)).
3. In the app, tap **+** and subscribe to your server. Set the server URL to `http://<server-ip>:2586` (or use your Tailscale IP for access outside the house). Subscribe to topics you'll use — e.g., `watchtower`, `uptime-kuma`, `alerts`.
4. **Connect Uptime Kuma:** In Uptime Kuma go to **Settings > Notifications**, add an ntfy notification with:
    - **Server URL:** `http://ntfy:2586` (internal Docker URL)
    - **Topic:** `uptime-kuma`
    - Click **Test** to confirm delivery to your phone.

---

## Phase 12: Container Update Monitoring (Watchtower)

Watchtower runs in monitor-only mode — it checks for updated container images daily at 4 AM and logs findings but never auto-applies updates. You stay informed without surprise breakage.

1. Watchtower starts automatically with the stack. To trigger an immediate manual scan:
    ```bash
    docker exec watchtower /watchtower --run-once
    ```
2. To review what updates are available:
    ```bash
    docker logs watchtower
    ```
3. **To enable ntfy notifications** (after Phase 11 is running): Add to `.env`:
    ```
    WATCHTOWER_NOTIFICATION_URL=generic+http://ntfy:2586/watchtower
    ```
    Uncomment `WATCHTOWER_NOTIFICATION_URL` in the watchtower `environment:` block in `docker-compose.yml`, then recreate the container:
    ```bash
    docker compose up -d watchtower
    ```
4. **To apply an available update**, pull the new image and recreate only that container:
    ```bash
    docker compose pull <container-name> && docker compose up -d <container-name>
    ```
    Check the project's changelog before updating Jellyfin, Sonarr, Radarr, or the *arr stack — they occasionally ship breaking database migrations.

---

## Phase 13: Vaultwarden (Self-Hosted Password Manager)

Vaultwarden is a lightweight, self-hosted Bitwarden-compatible server. All official Bitwarden apps and browser extensions work with it — this is the migration path off LastPass.

> **HTTPS required:** Bitwarden clients will not connect to Vaultwarden over plain HTTP. Set up an NPM proxy host pointing to `http://vaultwarden:80` with a Let's Encrypt certificate (see Phase 4) before creating your account.

1. **Generate an admin token** and add it to `.env`:
    ```bash
    openssl rand -base64 48
    ```
    ```env
    VAULTWARDEN_ADMIN_TOKEN=<paste generated token here>
    ```
    Recreate the container:
    ```bash
    docker compose up -d vaultwarden
    ```
    > If `VAULTWARDEN_ADMIN_TOKEN` is not set, the `/admin` panel is disabled entirely — the vault still works, you just can't access admin settings.

2. **Register your account:** Navigate to `https://vaultwarden.home/` and sign up. This creates your personal vault — the admin token is separate and does not give vault access.

3. **Disable new signups** to lock the instance to your account:
    - Go to `https://vaultwarden.home/admin` (use the token from step 1)
    - Uncheck **Allow new signups** and save.

4. **Connect Bitwarden clients:** In any Bitwarden-compatible app (browser extension, iOS, Android), go to **Settings > Server URL** and enter your Vaultwarden address:
    - Local network: `http://192.168.1.20:8888`
    - Via NPM: `https://vaultwarden.home`
    - Via Tailscale: `http://<tailscale-ip>:8888`

5. **Migrate from LastPass:**
    - In LastPass export your vault: **Account Options > Advanced > Export**
    - In your Bitwarden web vault go to **Tools > Import Data**, select **LastPass (csv)**, and upload the export
    - Verify all items imported correctly, then delete the export file from your machine
    - Install the Bitwarden browser extension, log in, and disable the LastPass extension
    - Full migration guide: [bitwarden.com/help/import-from-lastpass](https://bitwarden.com/help/import-from-lastpass/)
