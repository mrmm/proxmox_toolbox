# Ansible Role: proxmox_storage

Configure SWAP and S.M.A.R.T. settings for Proxmox.

## Description

Manages swap configuration and enables S.M.A.R.T. self-tests for disk health monitoring.

## Role Variables

```yaml
# SWAP configuration
proxmox_configure_swap: false
proxmox_swap_swappiness: 10  # 0-100
proxmox_disable_swap: false

# S.M.A.R.T. configuration
proxmox_enable_smart: false
proxmox_smart_short_test_schedule: "22:00"
proxmox_smart_short_test_day: "Sun"
proxmox_smart_long_test_schedule: "22:00"
proxmox_smart_long_test_day: "01"
```

## Example Playbook

```yaml
- hosts: proxmox_servers
  become: true
  roles:
    - role: proxmox_storage
      vars:
        proxmox_configure_swap: true
        proxmox_swap_swappiness: 1
        proxmox_enable_smart: true
```

## License

GPL-3.0-only
