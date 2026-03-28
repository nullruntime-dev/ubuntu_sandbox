# Ubuntu Systemd Sandbox

A minimal Ubuntu Docker image with full **systemd** support — `systemctl`, `journalctl`, and service management work out of the box. Built for local testing and development of systemd-based tools and services.

## Quick Start

```bash
docker pull nullruntimedev/ubuntu-systemd-sandbox
```

```bash
docker run -d --name ubuntu-sandbox \
  --privileged \
  --cgroupns=host \
  -v /sys/fs/cgroup:/sys/fs/cgroup:rw \
  --dns 8.8.8.8 \
  --dns 1.1.1.1 \
  -p 8090:8090 -p 3000:3000 \
  nullruntimedev/ubuntu-systemd-sandbox
```

```bash
docker exec -it ubuntu-sandbox bash
```

## What's Included

- **Base**: Ubuntu 22.04
- **Systemd**: Full init system running as PID 1
- **Networking**: `curl`, `wget`, `ping`, `dig`, `ip`, `netstat`
- **Utilities**: `vim`, `sudo`, `ca-certificates`

## Usage Examples

### Managing Services

```bash
# List running services
systemctl list-units --type=service

# Check service status
systemctl status systemd-resolved

# View logs
journalctl -xe
```

### Creating a Custom Service

```bash
cat <<EOF > /etc/systemd/system/my-app.service
[Unit]
Description=My Application
After=network.target

[Service]
Type=simple
ExecStart=/usr/bin/my-app
Restart=always

[Install]
WantedBy=multi-user.target
EOF

systemctl daemon-reload
systemctl enable --now my-app.service
```

### Extending with a Dockerfile

```dockerfile
FROM nullruntimedev/ubuntu-systemd-sandbox

RUN apt-get update && apt-get install -y nginx \
    && rm -rf /var/lib/apt/lists/*

RUN systemctl enable nginx

EXPOSE 80
```

## Build from Source

```bash
git clone https://github.com/nullruntime-dev/ubuntu-systemd-sandbox.git
cd ubuntu-systemd-sandbox

# Default (Ubuntu 22.04)
docker build -t ubuntu-systemd-sandbox .

# Specific Ubuntu version
docker build -t ubuntu-systemd-sandbox --build-arg VERSION=24.04 .
```

## Run Options

| Flag | Required | Purpose |
|------|----------|---------|
| `--privileged` | Yes | Grants systemd access to system resources |
| `--cgroupns=host` | Yes | Shares host cgroup namespace for systemd |
| `-v /sys/fs/cgroup:/sys/fs/cgroup:rw` | Yes | Mounts cgroup filesystem |
| `--dns 8.8.8.8` | Recommended | Ensures DNS resolution works inside the container |

### Persistent Data

Mount volumes to persist data across container restarts:

```bash
docker run -d --name ubuntu-sandbox \
  --privileged \
  --cgroupns=host \
  -v /sys/fs/cgroup:/sys/fs/cgroup:rw \
  -v ./data:/opt/data \
  nullruntimedev/ubuntu-systemd-sandbox
```

## Cleanup

```bash
docker stop ubuntu-sandbox && docker rm ubuntu-sandbox
```

## Security Note

This container runs in `--privileged` mode, which disables most Docker security restrictions. It is intended for **local development and testing only** — do not use in production.

## Credits

Dockerfile structure based on [jrei/systemd-ubuntu](https://github.com/j8r/dockerfiles) by **Jean-Rémy Falleri**.

## License

[MIT](LICENSE)