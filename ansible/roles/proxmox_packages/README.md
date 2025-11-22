# Ansible Role: proxmox_packages

Install useful dependencies for Proxmox VE/PBS.

## Description

Installs essential packages commonly needed on Proxmox systems including ifupdown2, git, sudo, libsasl2-modules, lshw, lm-sensors, and rsyslog. Optionally enables non-free-firmware repository and installs AMD microcode.

## Role Variables

```yaml
# List of essential packages to install
proxmox_essential_packages:
  - ifupdown2
  - git
  - sudo
  - libsasl2-modules
  - lshw
  - lm-sensors
  - rsyslog

# Whether to enable non-free-firmware repository
proxmox_enable_nonfree_firmware: false

# Whether to install AMD microcode
proxmox_install_amd_microcode: false
```

## Example Playbook

```yaml
- hosts: proxmox_servers
  become: true
  roles:
    - role: proxmox_packages
      vars:
        proxmox_install_amd_microcode: true
```

## License

GPL-3.0-only
