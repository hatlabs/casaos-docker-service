# casaos-docker-service

HaLOS includes CasaOS as a core component for managing Docker containers through a user-friendly web interface. As opposed to the standard CasaOS installation, CasaOS is run in a Docker container itself, allowing for easier updates and isolation from the host system. This repository contains the Docker service configuration and compose files needed to deploy CasaOS on HaLOS as well as Debian packaging to facilitate installation and management of the service.

## Files

- **docker-compose.yml**: Docker Compose configuration for CasaOS container
- **casaos.service**: Systemd service file for managing CasaOS
- **CLAUDE.md**: Documentation for Claude Code

## Installation

The CasaOS service is installed via a Debian package that:
1. Installs the docker-compose.yml file to `/opt/casa/`
2. Installs the systemd service file to `/etc/systemd/system/casaos.service`
3. Creates the `/opt/casa` directory structure
4. Enables and starts the casaos service

## Service Management

```bash
# Start CasaOS
sudo systemctl start casaos

# Stop CasaOS
sudo systemctl stop casaos

# Restart CasaOS
sudo systemctl restart casaos

# Check status
sudo systemctl status casaos

# View logs
sudo journalctl -u casaos -f
```

## Access

Once running, CasaOS is accessible at:
- **Web UI**: http://device-ip:80

## Data Location

CasaOS stores its data in `/opt/casa/`, which is mounted as a bind mount into the container at `/DATA`.

## Container Details

- **Image**: dockurr/casa
- **Container name**: casa
- **Port**: 80 (host) → 8080 (container)
- **Restart policy**: always
- **Stop grace period**: 1 minute

## Docker Socket Access

The CasaOS container has access to the Docker socket (`/var/run/docker.sock`), allowing it to manage other containers on the host system.
