# CasaOS Docker Service

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Git Workflow Policy

**IMPORTANT:** Always ask the user before:
- Committing files to git
- Pushing commits to remote repositories
- Creating or modifying git tags
- Running destructive git operations

## Overview

CasaOS provides a user-friendly web interface for Docker container management:
- Container management and monitoring
- App store interface for installing applications
- Web UI accessible on port 80
- Simple orchestration using Docker Compose

## Package Architecture

This Debian package bundles CasaOS as a Docker container with systemd management.

**Components:**
- **docker-compose.yml**: Defines the single-container CasaOS deployment
- **casaos.service**: Systemd service for lifecycle management

**Management approach:**
- systemd controls the container using `docker compose up/down` directly
- CasaOS runs in a container for isolation and easy updates
- Data stored in `/opt/casa/` on the host

**Key characteristics:**
- Single Docker container deployment
- Built-in app store for self-hosted applications
- Active upstream development
- Web UI on port 80

## Package Build Process

1. **debian/rules** processes files during build
2. Files installed to `/opt/casa/`:
   - `docker-compose.yml` - Container definition
3. Systemd service installed to `/etc/systemd/system/casaos.service`
4. Directory structure created at `/opt/casa/`

## Development

### Building the Package

```bash
# Build locally (requires Debian tools)
./run package:deb

# Build using Docker (recommended)
./run package:deb:docker

# Build for CI
./run package:deb:docker:ci
```

### Testing Locally

```bash
# After building, install the package
sudo dpkg -i casaos-docker-service_*.deb

# Check service status
sudo systemctl status casaos

# View logs
sudo journalctl -u casaos -f

# Access web UI
open http://localhost
```

### Updating CasaOS Version

The package uses automated version checking via GitHub Actions:
- Nightly workflow checks Docker Hub for new `dockurr/casa` versions
- Automatically creates PRs with version updates

Manual update:
1. Update `VERSION` file with new tag
2. Update image tag in `docker-compose.yml`
3. Update `debian/changelog`
4. Update `.bumpversion.cfg`
5. Rebuild package

### Debugging Issues

**Local debugging:**
```bash
# Check service status and logs
systemctl status casaos
journalctl -u casaos -f

# Container debugging
docker ps -a
docker logs casa

# Check configuration
cat /opt/casa/docker-compose.yml

# Manual start (for debugging)
cd /opt/casa
sudo docker compose up
```

**Remote debugging:**
```bash
# Check service status
ssh user@host "sudo systemctl status casaos --no-pager"

# Follow logs in real-time
ssh user@host "sudo journalctl -u casaos -f"

# Check container status
ssh user@host "docker ps -a | grep casa"

# Restart service
ssh user@host "sudo systemctl restart casaos"
```

## File Structure

```
casaos-docker-service/
├── VERSION                    # CasaOS/casa image version
├── debian/
│   ├── changelog              # Debian package changelog
│   ├── control                # Package metadata and dependencies
│   ├── rules                  # Build rules
│   ├── install                # File installation mappings
│   ├── postinst               # Post-installation script
│   ├── postrm                 # Post-removal script
│   └── source/format          # Package format
├── casaos.service             # Systemd service definition
├── docker-compose.yml         # CasaOS container definition
├── run                        # Build script
└── docker/                    # Docker build tools
    ├── Dockerfile.debtools
    └── docker-compose.debtools.yml
```

## Systemd Service

The service:
- Runs `docker compose up` directly from `/opt/casa/`
- Stops with `docker compose down`
- Depends on Docker service
- Restarts automatically on failure
- WorkingDirectory is `/opt/casa/`

## Important Notes

- **Version tracking**: VERSION file should match a valid `dockurr/casa` Docker image tag
- **Architecture support**: Multi-arch Docker image (amd64, arm64)
- **Container-in-container**: CasaOS runs in a container but manages other containers via Docker socket
- **Data persistence**: All data stored in `/opt/casa/` on host
- **Automated updates**: GitHub Actions workflow checks for new versions nightly

## Workflow Integration

### GitHub Actions

Workflows included:
1. **build.yml**: Build packages on every push
2. **release.yml**: Create releases with built packages
3. **update-casaos-version.yml**: Check for new CasaOS versions (nightly)

### Automated Version Updates

The nightly workflow checks Docker Hub for new `dockurr/casa` image tags and creates a PR when updates are available.

## Related Documentation

- **CasaOS upstream**: https://github.com/IceWhaleTech/CasaOS
- **CasaOS documentation**: https://casaos.io/
- **Docker image**: https://hub.docker.com/r/dockurr/casa
