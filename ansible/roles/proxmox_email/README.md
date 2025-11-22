# Ansible Role: proxmox_email

Configure email notifications for Proxmox (Legacy).

## Description

**IMPORTANT:** This role is for legacy email configuration. Proxmox VE 8+ has built-in GUI-based email configuration which is the recommended approach. This role is maintained for:
- Proxmox VE 7 and earlier
- Users who prefer infrastructure-as-code for all configurations
- Automated deployments

For PVE 8+, consider using the GUI configuration instead.

## Requirements

- Proxmox VE or PBS
- SMTP server credentials

## Role Variables

```yaml
proxmox_email_enabled: false

# SMTP server settings
proxmox_email_smtp_host: "smtp.gmail.com"
proxmox_email_smtp_port: 587
proxmox_email_smtp_use_tls: true

# SMTP authentication
proxmox_email_smtp_user: ""
proxmox_email_smtp_password: ""

# Email addresses
proxmox_email_from: "root@{{ ansible_hostname }}"
proxmox_email_root_alias: ""  # Forward root emails
```

## Example Playbook

```yaml
- hosts: proxmox_servers
  become: true
  roles:
    - role: proxmox_email
      vars:
        proxmox_email_enabled: true
        proxmox_email_smtp_host: "smtp.gmail.com"
        proxmox_email_smtp_port: 587
        proxmox_email_smtp_user: "your-email@gmail.com"
        proxmox_email_smtp_password: "your-app-password"
        proxmox_email_root_alias: "admin@example.com"
```

## Gmail App Passwords

For Gmail, you need to:
1. Enable 2FA on your Google account
2. Generate an App Password at https://myaccount.google.com/apppasswords
3. Use the App Password (not your regular password)

## License

GPL-3.0-only
