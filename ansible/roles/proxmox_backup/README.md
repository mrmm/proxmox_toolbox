---
# Ansible Role: proxmox_backup

Backup and restore Proxmox VE/PBS configurations.

## Description

Creates compressed backups of critical Proxmox configuration files and provides restoration capabilities.

## Role Variables

```yaml
# Backup configuration
proxmox_backup_enabled: false
proxmox_backup_dir: "/root/proxmox_config_backups"
proxmox_backup_retention_days: 30

# Restore configuration
proxmox_restore_enabled: false
proxmox_restore_archive: ""  # Path to backup archive
proxmox_restore_network: true
```

## What Gets Backed Up

- SSH configurations
- Fail2ban settings
- Network interfaces
- System configs (sysctl, hosts, hostname)
- Cron jobs and aliases
- SNMP and SMART configurations
- Postfix settings
- Proxmox configurations (/etc/pve/, /etc/proxmox-backup/)
- LVM and storage configs
- Firewall settings
- Cluster configurations

## Example Playbook

### Create backup:
```yaml
- hosts: proxmox_servers
  become: true
  roles:
    - role: proxmox_backup
      vars:
        proxmox_backup_enabled: true
        proxmox_backup_retention_days: 30
```

### Restore from backup:
```yaml
- hosts: proxmox_servers
  become: true
  roles:
    - role: proxmox_backup
      vars:
        proxmox_restore_enabled: true
        proxmox_restore_archive: "/root/proxmox_config_backups/hostname-20250122.tar.gz"
```

## License

GPL-3.0-only
