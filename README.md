# casaos-docker-service

Debian packaging for CasaOS, a user-friendly web interface for managing Docker containers. Unlike the standard CasaOS installation, this deployment runs CasaOS in a Docker container itself, allowing for easier updates and isolation from the host system. This repository contains the Docker service configuration and compose files needed to deploy CasaOS, as well as Debian packaging to facilitate installation and management of the service.

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

## Automated Version Updates

This repository includes a nightly workflow that checks for new versions of the `dockurr/casa` Docker image on Docker Hub. When a new version is detected, the workflow automatically:

1. Updates the `VERSION` file
2. Updates the image tag in `docker-compose.yml`
3. Adds a new entry to `debian/changelog`
4. Updates `.bumpversion.cfg`
5. Creates a Pull Request with all changes

The workflow runs:
- **Nightly** at 2 AM UTC
- **Manually** via workflow dispatch in GitHub Actions

This ensures the package stays in sync with upstream releases.

## License

This packaging repository (build scripts, Debian packaging files, systemd service, documentation) is licensed under the **MIT License**. See the [LICENSE](LICENSE) file for details.

**Important:** CasaOS itself is licensed under the **Apache-2.0 License**. When you install this package, you receive:
- The packaging infrastructure (MIT) - created by Hat Labs
- CasaOS software (Apache-2.0) - created by the CasaOS/IceWhale project

Users of the installed software are bound by CasaOS's Apache-2.0 license for the CasaOS application itself.

## Links

- **CasaOS Project**: https://github.com/IceWhaleTech/CasaOS
- **CasaOS Documentation**: https://casaos.io/
- **Docker Image**: https://hub.docker.com/r/dockurr/casa
- **This Packaging Repository**: https://github.com/hatlabs/casaos-docker-service
