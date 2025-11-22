# Ansible Role: proxmox_security

Configure security settings for Proxmox VE/PBS including fail2ban, SSH hardening, and user management.

## Description

This role provides comprehensive security configuration for Proxmox installations:
- Install and configure fail2ban with Proxmox-specific jails
- Create and configure SSH users with sudo access
- SSH hardening (disable root login, allow sudo group)
- PVE admin user creation and group management (PVE only)

## Requirements

- Proxmox VE 7.x, 8.x, or 9.x OR Proxmox Backup Server 3.x or 4.x
- Ansible >= 2.14

## Role Variables

```yaml
# Fail2ban
proxmox_enable_fail2ban: true

# SSH user configuration
proxmox_create_ssh_user: false
proxmox_ssh_username: ""
proxmox_ssh_user_password: ""
proxmox_ssh_allow_sudo_group: true
proxmox_ssh_deny_root_login: false
proxmox_ssh_generate_keys: true

# PVE user management (PVE only)
proxmox_create_pve_admin: false
proxmox_pve_admin_username: ""
proxmox_pve_admin_password: ""
proxmox_pve_admin_group: "administrators"
proxmox_pve_disable_root_pam: false
```

## Example Playbook

```yaml
- hosts: proxmox_servers
  become: true
  roles:
    - role: proxmox_security
      vars:
        proxmox_enable_fail2ban: true
        proxmox_create_ssh_user: true
        proxmox_ssh_username: "admin"
        proxmox_ssh_user_password: "changeme"
        proxmox_ssh_deny_root_login: true
        proxmox_create_pve_admin: true
        proxmox_pve_admin_username: "pvadmin"
        proxmox_pve_admin_password: "changeme"
```

## License

GPL-3.0-only
