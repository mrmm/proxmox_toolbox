# Proxmox Toolbox - Ansible Roles

Comprehensive Ansible roles for managing Proxmox VE and Proxmox Backup Server installations. Supports Proxmox VE 7, 8, 9 and PBS 3, 4.

## Overview

This collection of Ansible roles transforms the functionality of the original `proxmox_toolbox.sh` script into reusable, idempotent, and well-documented Ansible roles. Each role handles a specific aspect of Proxmox configuration and management.

## Available Roles

| Role | Description | Supports |
|------|-------------|----------|
| **proxmox_repository** | Configure no-subscription repositories | PVE 7/8/9, PBS 3/4 |
| **proxmox_updates** | System updates and subscription message removal | PVE 7/8/9, PBS 3/4 |
| **proxmox_packages** | Install essential packages and dependencies | PVE 7/8/9, PBS 3/4 |
| **proxmox_security** | Fail2ban, SSH hardening, user management | PVE 7/8/9, PBS 3/4 |
| **proxmox_storage** | SWAP and S.M.A.R.T. configuration | PVE 7/8/9, PBS 3/4 |
| **proxmox_monitoring** | SNMP v2/v3 monitoring setup | PVE 7/8/9, PBS 3/4 |
| **proxmox_backup** | Backup and restore configurations | PVE 7/8/9, PBS 3/4 |
| **proxmox_email** | Email notifications (legacy, for PVE <8) | PVE 7 |

## Quick Start

### 1. Install Requirements

```bash
# Install Ansible
apt install ansible

# Install required collections
ansible-galaxy collection install -r requirements.yml
```

### 2. Configure Inventory

Copy and customize the inventory file:

```bash
cp inventory/hosts.yml inventory/my-hosts.yml
# Edit inventory/my-hosts.yml with your server details
```

### 3. Run Initial Setup

```bash
# Test connection
ansible -i inventory/my-hosts.yml proxmox -m ping

# Run initial setup playbook
ansible-playbook -i inventory/my-hosts.yml playbooks/initial-setup.yml

# Or run specific roles with tags
ansible-playbook -i inventory/my-hosts.yml site.yml --tags security
```

## Example Playbooks

### Initial Setup (Recommended for New Installations)

```yaml
- name: Initial Proxmox Setup
  hosts: proxmox
  become: true
  vars:
    proxmox_enable_no_subscription: true
    proxmox_enable_fail2ban: true
    proxmox_configure_swap: true
    proxmox_swap_swappiness: 10
    proxmox_enable_smart: true
  roles:
    - proxmox_repository
    - proxmox_updates
    - proxmox_packages
    - proxmox_security
    - proxmox_storage
```

### Security Hardening

```yaml
- name: Harden Proxmox Security
  hosts: proxmox
  become: true
  vars:
    proxmox_enable_fail2ban: true
    proxmox_create_ssh_user: true
    proxmox_ssh_username: "admin"
    proxmox_ssh_deny_root_login: true
  roles:
    - proxmox_security
```

### Monitoring Setup

```yaml
- name: Configure Monitoring
  hosts: proxmox
  become: true
  vars:
    proxmox_enable_snmp: true
    proxmox_snmp_version: 3
    proxmox_snmp_v3_user: "monitor"
    proxmox_snmp_v3_auth_pass: "{{ vault_snmp_auth }}"
    proxmox_snmp_v3_priv_pass: "{{ vault_snmp_priv }}"
  roles:
    - proxmox_monitoring
```

## Version Support Matrix

| Proxmox Version | Debian Version | Repository Format | Support Status |
|----------------|----------------|-------------------|----------------|
| PVE 7.x        | Bullseye       | .list             | ✅ Full        |
| PVE 8.x        | Bookworm       | .list/.sources    | ✅ Full        |
| PVE 9.x        | Bookworm       | .sources          | ✅ Full        |
| PBS 3.x        | Bullseye       | .list             | ✅ Full        |
| PBS 4.x        | Bookworm       | .sources          | ✅ Full        |

## Role Documentation

Each role has its own README with detailed documentation:

- [proxmox_repository](roles/proxmox_repository/README.md)
- [proxmox_updates](roles/proxmox_updates/README.md)
- [proxmox_packages](roles/proxmox_packages/README.md)
- [proxmox_security](roles/proxmox_security/README.md)
- [proxmox_storage](roles/proxmox_storage/README.md)
- [proxmox_monitoring](roles/proxmox_monitoring/README.md)
- [proxmox_backup](roles/proxmox_backup/README.md)
- [proxmox_email](roles/proxmox_email/README.md)

## Common Use Cases

### 1. New Proxmox Installation

```bash
ansible-playbook -i inventory/my-hosts.yml playbooks/initial-setup.yml
```

This will:
- Configure no-subscription repositories
- Update the system
- Install essential packages
- Enable fail2ban
- Configure SWAP (swappiness=10)
- Enable S.M.A.R.T. tests
- Create a backup

### 2. Security Hardening

```bash
ansible-playbook -i inventory/my-hosts.yml playbooks/security-hardening.yml
```

This will:
- Install and configure fail2ban
- Create a new SSH user with sudo access
- Optionally disable root SSH login
- Create PVE admin user (for PVE hosts)

### 3. Regular Backups

```bash
# Create a cron job to run daily backups
ansible-playbook -i inventory/my-hosts.yml playbooks/backup.yml
```

### 4. Update All Servers

```bash
ansible-playbook -i inventory/my-hosts.yml site.yml --tags updates
```

## Advanced Configuration

### Using Ansible Vault for Sensitive Data

```bash
# Create encrypted vault file
ansible-vault create inventory/group_vars/proxmox/vault.yml

# Add sensitive variables
vault_ssh_password: "secure_password"
vault_pve_password: "secure_password"
vault_snmp_auth: "auth_password"
vault_snmp_priv: "priv_password"

# Reference in playbooks
proxmox_ssh_user_password: "{{ vault_ssh_password }}"
```

### Running on Specific Hosts

```bash
# Run on single host
ansible-playbook -i inventory/my-hosts.yml site.yml --limit pve1.example.com

# Run on group
ansible-playbook -i inventory/my-hosts.yml site.yml --limit proxmox_ve

# Dry run (check mode)
ansible-playbook -i inventory/my-hosts.yml site.yml --check --diff
```

### Using Tags

```bash
# Run only repository configuration
ansible-playbook -i inventory/my-hosts.yml site.yml --tags repository

# Run setup tasks only
ansible-playbook -i inventory/my-hosts.yml site.yml --tags setup

# Skip backups
ansible-playbook -i inventory/my-hosts.yml site.yml --skip-tags backup
```

## Directory Structure

```
ansible/
├── README.md                 # This file
├── requirements.yml          # Ansible Galaxy requirements
├── site.yml                  # Main playbook
├── ansible.cfg              # Ansible configuration (optional)
├── inventory/
│   └── hosts.yml            # Inventory example
├── playbooks/
│   ├── initial-setup.yml    # Initial setup playbook
│   ├── security-hardening.yml  # Security playbook
│   └── backup.yml           # Backup playbook
└── roles/
    ├── proxmox_repository/
    ├── proxmox_updates/
    ├── proxmox_packages/
    ├── proxmox_security/
    ├── proxmox_storage/
    ├── proxmox_monitoring/
    ├── proxmox_backup/
    └── proxmox_email/
```

## Best Practices

1. **Test First**: Always run playbooks in check mode (`--check`) first
2. **Use Version Control**: Store your inventory and playbooks in git
3. **Vault Secrets**: Use Ansible Vault for passwords and sensitive data
4. **Backup Before Changes**: Run backup playbook before major changes
5. **Test SSH Access**: When hardening SSH, test new user before disabling root
6. **Document Changes**: Keep notes of customizations to default variables

## Troubleshooting

### Connection Issues

```bash
# Test connectivity
ansible -i inventory/my-hosts.yml proxmox -m ping

# Test with verbose output
ansible-playbook -i inventory/my-hosts.yml site.yml -vvv
```

### Role-Specific Issues

Check the individual role README files for troubleshooting specific to each role.

### Common Issues

1. **Repository configuration fails**: Ensure the server has internet access
2. **Fail2ban not starting**: Check logs with `journalctl -u fail2ban`
3. **SNMP user creation fails**: Ensure passwords are at least 8 characters
4. **Backup fails**: Check disk space and permissions in backup directory

## Migration from Bash Script

If you're currently using `proxmox_toolbox.sh`, migrating to these Ansible roles provides:

- **Idempotency**: Can run multiple times safely
- **Infrastructure as Code**: Version-controlled configuration
- **Scalability**: Manage multiple servers simultaneously
- **Flexibility**: Pick and choose which roles to apply
- **Documentation**: Each role is well-documented
- **Community Support**: Standard Ansible format

### Migration Steps

1. Run a backup with the old script: `bash proxmox_toolbox.sh -b`
2. Document your current configuration
3. Create an Ansible inventory with your servers
4. Run roles gradually, starting with `proxmox_repository`
5. Verify each step before proceeding

## Contributing

Contributions are welcome! Please:

1. Follow Ansible best practices
2. Update documentation
3. Test with multiple Proxmox versions
4. Submit pull requests to the main repository

## License

GPL-3.0-only

## Author

Tonton Jo - https://www.youtube.com/c/tontonjo

Based on the original proxmox_toolbox: https://github.com/Tontonjo/proxmox_toolbox

## Support

- GitHub Issues: https://github.com/Tontonjo/proxmox_toolbox/issues
- YouTube: https://www.youtube.com/c/tontonjo
- Documentation: See individual role README files
