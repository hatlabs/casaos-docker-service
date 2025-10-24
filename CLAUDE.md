# CasaOS Docker Service

This repository contains the CasaOS containerized deployment configuration for Halos.

## Git Workflow Policy

**IMPORTANT:** Always ask the user before:
- Committing files to git
- Pushing commits to remote repositories
- Creating or modifying git tags
- Running destructive git operations

## Overview

CasaOS provides a user-friendly web interface for Docker container management in Halos:
- Container management and monitoring
- App store interface (with marine apps in marine variants)
- Web UI accessible on port 80/443

## Integration with Halos

- Installed during: `stage-halos-base/01-install-casaos/` (in hatlabs/halos-pi-gen repository)
- Available in: All Halos variants
- Configuration: App store sources vary by variant (marine apps for marine variants)

## Development

### Modifying CasaOS Configuration

1. Edit files in `stage-halos-base/01-install-casaos/` in the hatlabs/halos-pi-gen repository
2. Rebuild the image to test changes

### Debugging CasaOS Issues

```bash
# Check service status and logs
systemctl status casaos
journalctl -u casaos -f

# Container debugging
docker ps -a
docker logs <container-name>
```

## Related Documentation

- **CasaOS upstream**: https://github.com/IceWhaleTech/CasaOS

## Notes

This repository is primarily a submodule placeholder. Most CasaOS integration happens in the hatlabs/halos-pi-gen repository build stages.
