# Ansible Role: proxmox_updates

Update Proxmox VE/PBS system and remove subscription message.

## Description

This role performs system updates and removes the "No valid subscription" message from Proxmox installations using no-subscription repositories.

## Requirements

- Proxmox VE 7.x, 8.x, or 9.x OR Proxmox Backup Server 3.x or 4.x
- Ansible >= 2.14

## Role Variables

```yaml
# Whether to update apt cache before upgrading
proxmox_update_cache: true

# Whether to perform dist-upgrade
proxmox_dist_upgrade: true

# Whether to remove subscription message
proxmox_remove_subscription_message: true

# Whether to install proxmox-update command wrapper
proxmox_install_update_command: true

# Version of the update command wrapper
proxmox_update_command_version: "1.2"

# Whether to restart proxy service after subscription message removal
proxmox_restart_proxy: true
```

## Example Playbook

```yaml
- hosts: proxmox_servers
  become: true
  roles:
    - role: proxmox_updates
```

## License

GPL-3.0-only

## Author Information

Created by Tonton Jo based on the proxmox_toolbox project.
