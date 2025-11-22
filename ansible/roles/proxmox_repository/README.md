# Ansible Role: proxmox_repository

Configure Proxmox VE and PBS no-subscription repositories. Supports Proxmox VE 7, 8, 9 and PBS 3, 4.

## Description

This role automatically detects and configures the appropriate repositories for your Proxmox installation:
- Enables no-subscription repositories
- Disables/comments enterprise repositories
- Handles both legacy (.list) and modern (.sources) repository formats
- Supports seamless upgrades between Proxmox versions

## Requirements

- Proxmox VE 7.x, 8.x, or 9.x OR Proxmox Backup Server 3.x or 4.x
- Ansible >= 2.14
- Collections:
  - community.general
  - ansible.posix

## Role Variables

Available variables with their default values (see `defaults/main.yml`):

```yaml
# Auto-detect Proxmox type (pve or pbs) - can be overridden
# proxmox_type: auto  # auto, pve, or pbs

# Auto-detect Proxmox version - can be overridden
# proxmox_version: auto  # auto, 7, 8, or 9

# Auto-detect distribution codename - can be overridden
# proxmox_distribution: auto  # auto, bullseye, or bookworm

# Whether to disable enterprise repositories
proxmox_disable_enterprise: true

# Whether to enable no-subscription repositories
proxmox_enable_no_subscription: true

# Whether to clean up old .list files after migration to .sources
proxmox_cleanup_old_sources: true

# Whether to suggest running apt modernize-sources on upgrades
proxmox_suggest_modernize: true

# Update apt cache after configuration
proxmox_update_cache: true
```

## Dependencies

None.

## Example Playbook

### Basic usage (auto-detection):

```yaml
- hosts: proxmox_servers
  become: true
  roles:
    - role: proxmox_repository
```

### Override detected values:

```yaml
- hosts: proxmox_servers
  become: true
  roles:
    - role: proxmox_repository
      vars:
        proxmox_type: pve
        proxmox_version: 8
        proxmox_distribution: bookworm
```

### Custom configuration:

```yaml
- hosts: proxmox_servers
  become: true
  roles:
    - role: proxmox_repository
      vars:
        proxmox_disable_enterprise: true
        proxmox_enable_no_subscription: true
        proxmox_cleanup_old_sources: false
        proxmox_update_cache: true
```

## Supported Versions

| Proxmox Version | Debian Version | Repository Format | Status |
|----------------|----------------|-------------------|--------|
| PVE 7.x        | Bullseye       | .list             | ✓      |
| PVE 8.x        | Bookworm       | .list             | ✓      |
| PVE 9.x        | Bookworm       | .sources          | ✓      |
| PBS 3.x        | Bullseye       | .list             | ✓      |
| PBS 4.x        | Bookworm       | .sources          | ✓      |

## What This Role Does

1. **Auto-Detection**: Automatically detects:
   - Proxmox type (PVE or PBS)
   - Proxmox version
   - Distribution codename

2. **Repository Configuration**:
   - Adds no-subscription repositories
   - Disables/comments enterprise repositories
   - Handles version-specific repository formats

3. **Migration Support**:
   - Detects and comments old repository entries
   - Suggests modernizing sources after upgrades
   - Cleans up deprecated .list files

4. **Idempotency**: All tasks are idempotent and can be run multiple times safely

## License

GPL-3.0-only

## Author Information

This role was created by Tonton Jo based on the [proxmox_toolbox](https://github.com/Tontonjo/proxmox_toolbox) project.

YouTube: https://www.youtube.com/c/tontonjo
