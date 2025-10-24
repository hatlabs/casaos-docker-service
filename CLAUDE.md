# CasaOS Docker Service

This repository contains the CasaOS containerized deployment configuration for Halos. For the overall project architecture, see [../CLAUDE.md](../CLAUDE.md).

## Overview

CasaOS is a user-friendly web interface for managing Docker containers and applications. In Halos, it provides:
- Container management and monitoring
- App store interface (with marine apps in marine variants)
- Simple web UI accessible on port 80/443
- Easy installation of additional services

## Integration with Halos

CasaOS is installed in all Halos variants via the `stage-halos-base/` stage during image building. See [../halos-pi-gen/CLAUDE.md](../halos-pi-gen/CLAUDE.md) for build system details.

### Installation Location
- Installed during: `stage-halos-base/01-install-casaos/`
- Service configuration: Systemd service files
- Web interface: http://device-ip:80 or https://device-ip:443

### Configuration

CasaOS configuration files are deployed during the image build process. Key configurations include:
- Default port bindings
- App store sources (marine app store for marine variants)
- User permissions and access controls

## Development

### Modifying CasaOS Configuration

To change how CasaOS is configured in Halos images:

1. Navigate to `halos-pi-gen/stage-halos-base/01-install-casaos/`
2. Edit task scripts or configuration files
3. Rebuild the image to test changes

### Testing Changes

CasaOS changes can be tested by:
1. Building a test image with modified configuration
2. Deploying to a Raspberry Pi or VM
3. Accessing the web interface to verify functionality

## App Store Integration

Marine variants of Halos include a custom app store with curated marine applications. See [../casaos-marine-store/CLAUDE.md](../casaos-marine-store/CLAUDE.md) for details on the marine app store.

## Common Tasks

### Adding a New App to CasaOS

Apps can be added to CasaOS in two ways:

1. **Via the app store** (recommended): Add app definition to casaos-marine-store
2. **Manual installation**: Use CasaOS web interface to add custom Docker Compose configurations

### Debugging CasaOS Issues

```bash
# Check CasaOS service status
systemctl status casaos

# View CasaOS logs
journalctl -u casaos -f

# Restart CasaOS
systemctl restart casaos
```

## Related Documentation

- **Parent project**: [../CLAUDE.md](../CLAUDE.md) - Overall architecture
- **Build system**: [../halos-pi-gen/CLAUDE.md](../halos-pi-gen/CLAUDE.md) - How CasaOS is integrated
- **Marine apps**: [../casaos-marine-store/CLAUDE.md](../casaos-marine-store/CLAUDE.md) - App store content
- **CasaOS upstream**: https://github.com/IceWhaleTech/CasaOS

## Notes

This repository is a submodule placeholder for CasaOS-specific configuration and deployment scripts. Most CasaOS integration happens in the `halos-pi-gen` build stages.
