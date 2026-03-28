# Ubuntu Systemd Sandbox

Minimal Ubuntu image with full **systemd** support. `systemctl`, `journalctl`, and service management work out of the box.

## Quick Start

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

## Included Tools

`systemd` · `curl` · `wget` · `vim` · `ping` · `dig` · `ip` · `netstat` · `sudo`

## Build Args

| Arg | Default | Description |
|-----|---------|-------------|
| `VERSION` | `22.04` | Ubuntu base version |

## Required Run Flags

| Flag | Purpose |
|------|---------|
| `--privileged` | System resource access for systemd |
| `--cgroupns=host` | Host cgroup namespace |
| `-v /sys/fs/cgroup:/sys/fs/cgroup:rw` | Cgroup filesystem mount |
| `--dns 8.8.8.8` | DNS resolution |

## Source & Issues

GitHub: [nullruntime-dev/ubuntu-systemd-sandbox](https://github.com/nullruntime-dev/ubuntu-systemd-sandbox)

## Credits

Based on [jrei/systemd-ubuntu](https://github.com/j8r/dockerfiles) by Jean-Rémy Falleri.

## License

MIT