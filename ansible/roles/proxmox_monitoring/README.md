# Ansible Role: proxmox_monitoring

Configure SNMP monitoring for Proxmox VE/PBS.

## Description

Installs and configures SNMP daemon for monitoring Proxmox servers. Supports both SNMPv2 and SNMPv3.

## Requirements

- pexpect Python library for SNMPv3 configuration (installed automatically by Ansible)

## Role Variables

```yaml
# SNMP configuration
proxmox_enable_snmp: false
proxmox_snmp_version: 3  # 2 or 3

# SNMPv2 configuration
proxmox_snmp_ro_community: "public"
proxmox_snmp_allowed_subnet: ""  # e.g., "192.168.1.0/24"

# SNMPv3 configuration (MD5/DES)
proxmox_snmp_v3_user: "monitor"
proxmox_snmp_v3_auth_pass: "changeme123"
proxmox_snmp_v3_priv_pass: "changeme456"
```

## Example Playbook

### SNMPv3 (recommended):
```yaml
- hosts: proxmox_servers
  become: true
  roles:
    - role: proxmox_monitoring
      vars:
        proxmox_enable_snmp: true
        proxmox_snmp_version: 3
        proxmox_snmp_v3_user: "monitoring"
        proxmox_snmp_v3_auth_pass: "authpassword123"
        proxmox_snmp_v3_priv_pass: "privpassword456"
```

### SNMPv2:
```yaml
- hosts: proxmox_servers
  become: true
  roles:
    - role: proxmox_monitoring
      vars:
        proxmox_enable_snmp: true
        proxmox_snmp_version: 2
        proxmox_snmp_ro_community: "my_community"
        proxmox_snmp_allowed_subnet: "10.0.0.0/24"
```

## License

GPL-3.0-only
