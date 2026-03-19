# Setup Guide

This guide walks through the full build and deployment from bare hardware to a running stack. Complete each phase in order.

---

## Phase 1: NAS Setup (TrueNAS SCALE)

1. **Install OS:** Install TrueNAS SCALE to the dedicated M.2 NVMe boot SSD — **not** to any of the IronWolf HDDs. During the installer, select only the M.2 drive as the boot device. TrueNAS SCALE 24.10+ will generate an active alert if it detects a USB device as the boot pool; a dedicated internal SSD avoids this entirely.
2. **Create Storage Pool:** Create a ZFS pool with RAID-Z1 across your 4 drives.
3. **Create Datasets:** Create datasets for media and downloads (e.g., `/mnt/pool/media`, `/mnt/pool/downloads`).
4. **Enable Shares:** Turn on the NFS service for the App Server to mount. Optionally enable SMB for Windows access.
5. **Configure UPS (NUT Master):** Connect the CyberPower CP1500PFCRM2U USB cable to the NAS. In TrueNAS SCALE go to **System Settings → Services → UPS** and configure:
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

1. **Install OS:** Install Ubuntu Server LTS to **Disk 1 (Samsung 980 PRO 2TB)** with the OpenSSH server enabled for headless management. Disk 2 is left unformatted at install time for use as additional storage or backup staging.
2. **Configure UPS Client (NUT):** Install the NUT client so the App Server shuts down gracefully when the UPS signals low battery:
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
3. **Install GPU Drivers:** Install the proprietary Nvidia drivers for the RTX 2070 Super.
3. **Install Docker:** Install Docker Engine and the NVIDIA Container Toolkit to allow containers to access the GPU.
4. **Install Tailscale:**
    ```bash
    curl -fsSL https://tailscale.com/install.sh | sh
    ```
5. **Mount NAS Storage:** Edit `/etc/fstab` to permanently mount the NFS shares:
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

### Validation Checklist

| Check | Command / Action |
| :--- | :--- |
| **VPN kill-switch** | `docker exec -it gluetun wget -qO- https://ifconfig.me` — must return VPN IP, not home IP |
| **qBittorrent locked to VPN** | `docker stop gluetun` — qBittorrent should immediately lose connectivity |
| **Hardware transcoding** | Play a 4K file in Jellyfin, then run `nvidia-smi` on host — a `jellyfin` process should be using the GPU encoder |
| **HTTPS** | All proxy-exposed services load over HTTPS with a valid certificate |
| **Monitoring** | Grafana dashboards show live data from Node Exporter |
| **Backup** | Trigger a manual Duplicati backup and confirm it completes without errors |
