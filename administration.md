# Bluefin Administrator's Guide

This guide provides comprehensive information for system administrators managing Bluefin deployments. Bluefin is a cloud-native desktop operating system built on Fedora Silverblue that offers reliability comparable to a Chromebook with minimal maintenance requirements.

## Table of Contents

- [System Overview](#system-overview)
- [Installation and Setup](#installation-and-setup)
- [System Management](#system-management)
- [Package Management](#package-management)
- [Security Configuration](#security-configuration)
- [Networking](#networking)
- [Troubleshooting](#troubleshooting)
- [Maintenance and Updates](#maintenance-and-updates)
- [Community and Support](#community-and-support)

## System Overview

Bluefin is designed to provide:
- **Reliability**: A system as dependable as a Chromebook with near-zero maintenance
- **Developer Experience**: A powerful cloud-native developer workflow
- **Modern Desktop**: GNOME desktop environment with carefully curated applications
- **Container-based Architecture**: Built using container technologies for consistency and reliability

### Key Components

- **Base System**: Fedora Silverblue with immutable filesystem
- **Package Management**: Homebrew for command-line tools, Flatpak for applications
- **Desktop Environment**: GNOME with custom configurations
- **Application Store**: Bazaar for enhanced Flathub integration
- **Container Runtime**: Podman for containerized workloads

## Installation and Setup

### System Requirements

- 64-bit x86_64 processor
- 4 GB RAM minimum (8 GB recommended)
- 20 GB storage minimum (40 GB recommended)
- UEFI firmware with Secure Boot support

### Initial Installation

1. Download the latest Bluefin ISO from the official releases
2. Create bootable media using tools like Balena Etcher or Ventoy
3. Boot from the installation media
4. Follow the Anaconda installer prompts
5. Complete the initial user setup

### Post-Installation Configuration

After the first boot, you'll be prompted to enroll the Secure Boot key. When prompted, enter the password:
```
universalblue
```

If you miss this step during initial setup, you can manually enroll the key:
```bash
ujust enroll-secure-boot-key
```

## System Management

### Essential Commands

Bluefin includes the `ujust` command system for common administrative tasks:

```bash
# Display available commands
ujust --choose

# Show system changelogs
ujust changelogs

# Access rebase helper for system recovery
ujust rebase-helper

# Toggle user message of the day
ujust toggle-user-motd

# Enable enhanced terminal features
ujust bluefin-cli
```

### System Updates

Bluefin automatically manages system updates through the bootc (bootable container) system:

```bash
# Check for updates (automatic by default)
bootc upgrade

# Roll back to previous version if needed
sudo bootc rollback
```

### Image Management

View current system information:
```bash
# Check current image version
rpm-ostree status

# View system information
ujust --help
```

## Package Management

Bluefin uses a layered approach to package management:

### Homebrew (Command-line tools)

Homebrew manages command-line packages and development tools:

```bash
# Search for packages
brew search <package-name>

# Install packages
brew install <package-name>

# Update all packages
brew upgrade

# List installed packages
brew list
```

Bluefin automatically handles Homebrew updates, so manual intervention is rarely needed.

### Flatpak (Desktop applications)

Desktop applications are managed through Flatpak:

```bash
# Install applications
flatpak install <app-id>

# Update applications
flatpak update

# List installed applications
flatpak list

# Remove applications
flatpak uninstall <app-id>
```

### Bazaar Application Store

Bazaar provides an enhanced interface for Flathub applications. Support the project through [donations](https://github.com/kolunmi/bazaar).

## Security Configuration

### Secure Boot

Bluefin supports Secure Boot by default with a custom signing key. The public key is available in the [akmods repository](https://github.com/ublue-os/akmods/raw/main/certs/public_key.der).

To enroll the key before installation:
```bash
sudo mokutil --timeout -1
sudo mokutil --import public_key.der
```

### Firewall Management

The system uses standard Fedora firewall configurations:
```bash
# Check firewall status
sudo firewall-cmd --state

# List active zones
sudo firewall-cmd --get-active-zones

# Add services to firewall
sudo firewall-cmd --permanent --add-service=<service-name>
sudo firewall-cmd --reload
```

## Networking

### Tailscale Integration

Bluefin includes Tailscale for secure networking. Refer to the [Tailscale documentation](https://tailscale.com/kb/1017/install) for setup instructions.

### NetworkManager

Standard NetworkManager tools are available:
```bash
# Command-line network management
nmcli

# Text-based user interface
nmtui
```

## Troubleshooting

### System Recovery

If updates cause issues, you can roll back:
```bash
# Roll back to previous system state
sudo bootc rollback

# Reboot to activate the previous version
sudo reboot
```

### Log Analysis

View system logs:
```bash
# System journal
journalctl

# Boot messages
journalctl -b

# Specific service logs
journalctl -u <service-name>
```

### Performance Monitoring

Monitor system performance:
```bash
# System resource usage
top
htop

# Disk usage
df -h
du -sh <directory>
```

### Common Issues

**Boot Problems**: Use the rebase helper to switch between image versions:
```bash
ujust rebase-helper
```

**Application Issues**: Reset Flatpak applications:
```bash
flatpak repair
```

**Network Issues**: Restart NetworkManager:
```bash
sudo systemctl restart NetworkManager
```

## Maintenance and Updates

### Automatic Updates

Bluefin automatically manages:
- System image updates through bootc
- Flatpak application updates
- Homebrew package updates

### Manual Maintenance

Periodic maintenance tasks:

```bash
# Clean package caches
brew cleanup
flatpak uninstall --unused

# Update documentation cache
sudo mandb

# Check system integrity
rpm-ostree status
```

### Monitoring System Health

```bash
# Check systemd services
systemctl --failed

# Monitor disk space
df -h

# Check memory usage
free -h
```

## Community and Support

### Documentation and Resources

- **Primary Documentation**: [docs.projectbluefin.io](https://docs.projectbluefin.io/)
- **Community Discussions**: [Bluefin Discourse](https://universal-blue.discourse.group/c/bluefin/6)
- **Issue Reporting**: [GitHub Issues](https://github.com/ublue-os/bluefin/issues)
- **Project Website**: [projectbluefin.io](https://projectbluefin.io/)

### Getting Help

1. **Community Forum**: Start with the Discourse community for general questions
2. **Documentation**: Check the comprehensive documentation site
3. **GitHub Issues**: Report bugs and feature requests
4. **Release Notes**: Review [release notes](https://github.com/ublue-os/bluefin/releases) for known issues

### Contributing

Support the Bluefin ecosystem:
- **GNOME Foundation**: [Donate to GNOME](https://donate.gnome.org)
- **Bazaar Development**: [Support Bazaar](https://github.com/kolunmi/bazaar)
- **Project Funding**: [Bluefin Donations](https://docs.projectbluefin.io/donations)

### Related Projects

- **Server Solutions**: [ucore](https://github.com/ublue-os/ucore) for server deployments
- **Cloud Native Ecosystem**: [CNCF Landscape](https://landscape.cncf.io)

---

## Quick Reference

### Essential Commands Cheat Sheet

| Command | Description |
|---------|-------------|
| `ujust --choose` | Show available system commands |
| `ujust changelogs` | View recent system changes |
| `ujust rebase-helper` | System recovery and image management |
| `sudo bootc rollback` | Roll back to previous system version |
| `brew search <package>` | Search for command-line packages |
| `brew install <package>` | Install command-line packages |
| `tldr <command>` | Get quick help for commands |
| `Ctrl+Alt+Enter` | Open terminal quickly |

### Emergency Contacts

- **Critical Issues**: Report via GitHub Issues with "urgent" label
- **Security Vulnerabilities**: Follow responsible disclosure guidelines in the repository
- **Community Support**: Post in Discourse forums for assistance

For the most up-to-date information, always refer to the official documentation at [docs.projectbluefin.io](https://docs.projectbluefin.io/).
