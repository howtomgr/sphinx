# sphinx Installation Guide

sphinx is a free and open-source documentation generator. Sphinx creates intelligent documentation primarily for Python projects

## Table of Contents
1. [Prerequisites](#prerequisites)
2. [Supported Operating Systems](#supported-operating-systems)
3. [Installation](#installation)
4. [Configuration](#configuration)
5. [Service Management](#service-management)
6. [Troubleshooting](#troubleshooting)
7. [Security Considerations](#security-considerations)
8. [Performance Tuning](#performance-tuning)
9. [Backup and Restore](#backup-and-restore)
10. [System Requirements](#system-requirements)
11. [Support](#support)
12. [Contributing](#contributing)
13. [License](#license)
14. [Acknowledgments](#acknowledgments)
15. [Version History](#version-history)
16. [Appendices](#appendices)

## 1. Prerequisites

- **Hardware Requirements**:
  - CPU: 1 core minimum
  - RAM: 256MB minimum
  - Storage: 100MB for docs
  - Network: Build tool only
- **Operating System**: 
  - Linux: Any modern distribution (RHEL, Debian, Ubuntu, CentOS, Fedora, Arch, Alpine, openSUSE)
  - macOS: 10.14+ (Mojave or newer)
  - Windows: Windows Server 2016+ or Windows 10
  - FreeBSD: 11.0+
- **Network Requirements**:
  - Port N/A (default sphinx port)
  - Dev server 8000
- **Dependencies**:
  - See official documentation for specific requirements
- **System Access**: root or sudo privileges required


## 2. Supported Operating Systems

This guide supports installation on:
- RHEL 8/9 and derivatives (CentOS Stream, Rocky Linux, AlmaLinux)
- Debian 11/12
- Ubuntu 20.04/22.04/24.04 LTS
- Arch Linux (rolling release)
- Alpine Linux 3.18+
- openSUSE Leap 15.5+ / Tumbleweed
- SUSE Linux Enterprise Server (SLES) 15+
- macOS 12+ (Monterey and later) 
- FreeBSD 13+
- Windows 10/11/Server 2019+ (where applicable)

## 3. Installation

### RHEL/CentOS/Rocky Linux/AlmaLinux

```bash
# Install EPEL repository if needed
sudo dnf install -y epel-release

# Install sphinx
sudo dnf install -y sphinx

# Enable and start service
sudo systemctl enable --now sphinx

# Configure firewall
sudo firewall-cmd --permanent --add-port=N/A/tcp
sudo firewall-cmd --reload

# Verify installation
sphinx-build --version
```

### Debian/Ubuntu

```bash
# Update package index
sudo apt update

# Install sphinx
sudo apt install -y sphinx

# Enable and start service
sudo systemctl enable --now sphinx

# Configure firewall
sudo ufw allow N/A

# Verify installation
sphinx-build --version
```

### Arch Linux

```bash
# Install sphinx
sudo pacman -S sphinx

# Enable and start service
sudo systemctl enable --now sphinx

# Verify installation
sphinx-build --version
```

### Alpine Linux

```bash
# Install sphinx
apk add --no-cache sphinx

# Enable and start service
rc-update add sphinx default
rc-service sphinx start

# Verify installation
sphinx-build --version
```

### openSUSE/SLES

```bash
# Install sphinx
sudo zypper install -y sphinx

# Enable and start service
sudo systemctl enable --now sphinx

# Configure firewall
sudo firewall-cmd --permanent --add-port=N/A/tcp
sudo firewall-cmd --reload

# Verify installation
sphinx-build --version
```

### macOS

```bash
# Using Homebrew
brew install sphinx

# Start service
brew services start sphinx

# Verify installation
sphinx-build --version
```

### FreeBSD

```bash
# Using pkg
pkg install sphinx

# Enable in rc.conf
echo 'sphinx_enable="YES"' >> /etc/rc.conf

# Start service
service sphinx start

# Verify installation
sphinx-build --version
```

### Windows

```bash
# Using Chocolatey
choco install sphinx

# Or using Scoop
scoop install sphinx

# Verify installation
sphinx-build --version
```

## Initial Configuration

### Basic Configuration

```bash
# Create configuration directory
sudo mkdir -p /etc/sphinx

# Set up basic configuration
# See official documentation for detailed configuration options

# Test configuration
sphinx-build --version
```

## 5. Service Management

### systemd (RHEL, Debian, Ubuntu, Arch, openSUSE)

```bash
# Enable service
sudo systemctl enable sphinx

# Start service
sudo systemctl start sphinx

# Stop service
sudo systemctl stop sphinx

# Restart service
sudo systemctl restart sphinx

# Check status
sudo systemctl status sphinx

# View logs
sudo journalctl -u sphinx -f
```

### OpenRC (Alpine Linux)

```bash
# Enable service
rc-update add sphinx default

# Start service
rc-service sphinx start

# Stop service
rc-service sphinx stop

# Restart service
rc-service sphinx restart

# Check status
rc-service sphinx status
```

### rc.d (FreeBSD)

```bash
# Enable in /etc/rc.conf
echo 'sphinx_enable="YES"' >> /etc/rc.conf

# Start service
service sphinx start

# Stop service
service sphinx stop

# Restart service
service sphinx restart

# Check status
service sphinx status
```

### launchd (macOS)

```bash
# Using Homebrew services
brew services start sphinx
brew services stop sphinx
brew services restart sphinx

# Check status
brew services list | grep sphinx
```

### Windows Service Manager

```powershell
# Start service
net start sphinx

# Stop service
net stop sphinx

# Using PowerShell
Start-Service sphinx
Stop-Service sphinx
Restart-Service sphinx

# Check status
Get-Service sphinx
```

## Advanced Configuration

See the official documentation for advanced configuration options.

## Reverse Proxy Setup

### nginx Configuration

```nginx
upstream sphinx_backend {
    server 127.0.0.1:N/A;
}

server {
    listen 80;
    server_name sphinx.example.com;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name sphinx.example.com;

    ssl_certificate /etc/ssl/certs/sphinx.example.com.crt;
    ssl_certificate_key /etc/ssl/private/sphinx.example.com.key;

    location / {
        proxy_pass http://sphinx_backend;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

### Apache Configuration

```apache
<VirtualHost *:80>
    ServerName sphinx.example.com
    Redirect permanent / https://sphinx.example.com/
</VirtualHost>

<VirtualHost *:443>
    ServerName sphinx.example.com
    
    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/sphinx.example.com.crt
    SSLCertificateKeyFile /etc/ssl/private/sphinx.example.com.key
    
    ProxyRequests Off
    ProxyPreserveHost On
    
    ProxyPass / http://127.0.0.1:N/A/
    ProxyPassReverse / http://127.0.0.1:N/A/
</VirtualHost>
```

### HAProxy Configuration

```haproxy
frontend sphinx_frontend
    bind *:80
    bind *:443 ssl crt /etc/ssl/certs/sphinx.pem
    redirect scheme https if !{ ssl_fc }
    default_backend sphinx_backend

backend sphinx_backend
    balance roundrobin
    server sphinx1 127.0.0.1:N/A check
```

## Security Configuration

### Basic Security Setup

```bash
# Set appropriate permissions
sudo chown -R sphinx:sphinx /etc/sphinx
sudo chmod 750 /etc/sphinx

# Configure firewall
sudo firewall-cmd --permanent --add-port=N/A/tcp
sudo firewall-cmd --reload

# Enable SELinux policies (if applicable)
sudo setsebool -P httpd_can_network_connect on
```

## Database Setup

See official documentation for database configuration requirements.

## Performance Optimization

### System Tuning

```bash
# Basic system tuning
echo 'net.core.somaxconn = 65535' | sudo tee -a /etc/sysctl.conf
echo 'net.ipv4.tcp_max_syn_backlog = 65535' | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

## Monitoring

### Basic Monitoring

```bash
# Check service status
sudo systemctl status sphinx

# View logs
sudo journalctl -u sphinx -f

# Monitor resource usage
top -p $(pgrep sphinx)
```

## 9. Backup and Restore

### Backup Script

```bash
#!/bin/bash
# Basic backup script
BACKUP_DIR="/backup/sphinx"
DATE=$(date +%Y%m%d_%H%M%S)

mkdir -p "$BACKUP_DIR"
tar -czf "$BACKUP_DIR/sphinx-backup-$DATE.tar.gz" /etc/sphinx /var/lib/sphinx

echo "Backup completed: $BACKUP_DIR/sphinx-backup-$DATE.tar.gz"
```

### Restore Procedure

```bash
# Stop service
sudo systemctl stop sphinx

# Restore from backup
tar -xzf /backup/sphinx/sphinx-backup-*.tar.gz -C /

# Start service
sudo systemctl start sphinx
```

## 6. Troubleshooting

### Common Issues

1. **Service won't start**:
```bash
# Check logs
sudo journalctl -u sphinx -n 100
sudo tail -f /var/log/sphinx/sphinx.log

# Check configuration
sphinx-build --version

# Check permissions
ls -la /etc/sphinx
```

2. **Connection issues**:
```bash
# Check if service is listening
sudo ss -tlnp | grep N/A

# Test connectivity
telnet localhost N/A

# Check firewall
sudo firewall-cmd --list-all
```

3. **Performance issues**:
```bash
# Check resource usage
top -p $(pgrep sphinx)

# Check disk I/O
iotop -p $(pgrep sphinx)

# Check connections
ss -an | grep N/A
```

## Integration Examples

### Docker Compose Example

```yaml
version: '3.8'
services:
  sphinx:
    image: sphinx:latest
    ports:
      - "N/A:N/A"
    volumes:
      - ./config:/etc/sphinx
      - ./data:/var/lib/sphinx
    restart: unless-stopped
```

## Maintenance

### Update Procedures

```bash
# RHEL/CentOS/Rocky/AlmaLinux
sudo dnf update sphinx

# Debian/Ubuntu
sudo apt update && sudo apt upgrade sphinx

# Arch Linux
sudo pacman -Syu sphinx

# Alpine Linux
apk update && apk upgrade sphinx

# openSUSE
sudo zypper update sphinx

# FreeBSD
pkg update && pkg upgrade sphinx

# Always backup before updates
tar -czf /backup/sphinx-pre-update-$(date +%Y%m%d).tar.gz /etc/sphinx

# Restart after updates
sudo systemctl restart sphinx
```

### Regular Maintenance

```bash
# Log rotation
sudo logrotate -f /etc/logrotate.d/sphinx

# Clean old logs
find /var/log/sphinx -name "*.log" -mtime +30 -delete

# Check disk usage
du -sh /var/lib/sphinx
```

## Additional Resources

- Official Documentation: https://docs.sphinx.org/
- GitHub Repository: https://github.com/sphinx/sphinx
- Community Forum: https://forum.sphinx.org/
- Best Practices Guide: https://docs.sphinx.org/best-practices

---

**Note:** This guide is part of the [HowToMgr](https://howtomgr.github.io) collection. Always refer to official documentation for the most up-to-date information.
