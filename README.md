# Ansible Role: SSH Hardening with ssh-audit - Secure and Audit SSH with Ansible

[![CI](https://github.com/HomeSecExplorer/ansible-role-sshaudit/actions/workflows/ci.yml/badge.svg)](https://github.com/HomeSecExplorer/ansible-role-sshaudit/actions/workflows/ci.yml)
![Ansible Galaxy](https://img.shields.io/badge/ansible-galaxy-blue?logo=ansible)
![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)

---

**Author:** HomeSec Explorer  
**License:** MIT  
**Tags:** ssh, ssh-audit, ssh hardening, security, automate ssh audit, ssh server  
Based on hardening recommendations from [ssh-audit](https://github.com/jtesta/ssh-audit)

## Description

This role hardens SSH server and client based on [ssh-audit](https://github.com/jtesta/ssh-audit). It aims to provide safe defaults, repeatable results, and clear verification so you can apply consistent SSH security across your fleet.

This role applies basic SSH security measures. For comprehensive SSH and system hardening, refer to OS guidance such as CIS Benchmarks or DISA STIGs.

---

## Requirements

- Ansible `>= 2.13`
- Root (or sudo) privileges on the target host
- Internet access to install `ssh-audit` (unless pre-installed or not wanted)
- **ssh-audit** utility will be installed as needed for automated audits (can be disabled)
- EPEL repo required on Rocky 9 to install ssh‑audit

## Supported operating systems

- Debian 11 (Bullseye), 12 (Bookworm), 13 (Trixie)
- Ubuntu 22.04 (Jammy), 24.04 (Noble)
- Rocky Linux 9
- Amazon Linux 2023

> Note: the OS compatibility check (`hseph_os_check`) ensures the platforms supported by this role, not official ssh-audit support.

- **SSH client hardening is not supported on Debian 11.**
- **ssh-audit package will not be installed on Amazon Linux 2023**
- To ensure firewall connection rate throttling rules are applied, set `hsesa_connection_rate_throttle` to `true` in `defaults/main.yml`

## Test matrix

**Legend:** :white_check_mark: manual test passed - :repeat: covered in CI - :white_circle: not tested

| Distro | Version | Manually verified | CI | Notes |
|:-------|:--------|:-----------------:|:--:|:-----|
| Debian | 13 | :white_check_mark: | :repeat: |  |
| Debian | 12 | :white_check_mark: | :repeat: |  |
| Debian | 11 | :white_circle: | :repeat: |  |
| Ubuntu | 24.04 | :white_check_mark: | :repeat: |  |
| Ubuntu | 22.04 | :white_circle: | :repeat: |  |
| Rocky  | 9 | :white_check_mark: | :repeat: |  |
| Amazon | 2023 | :white_circle: | :repeat: |  |

---

## Role variables

See `defaults/main.yml` for all configurable variables.

```yaml
# Install or remove the ssh-audit utility
hsesa_ssh_audit_package_state: present   # present | absent

# Run ssh-audit before and after hardening
hsesa_ssh_audit_test: true

# SSH server hardening options
hsesa_ssh_server_hardening: true               # Enable server-side SSH hardening
hsesa_remove_small_dh_moduli: true             # Remove weak DH moduli for better cryptographic security
hsesa_restrict_kcm: true                       # Restrict ssh-agent to secure socket (KCM)
hsesa_connection_rate_throttle: false          # Enable SSH brute-force attack throttling (iptables or firewalld)
hsesa_regenerate_ssh_host_keys: true           # Regenerate SSH host keys with secure algorithms

# SSH client hardening
hsesa_harden_client: true
```

Example inventory:

```yaml
[servers]
hse-debxyz ansible_host=192.168.1.200
```

---

## Available Tags

- `install`: install or remove the `ssh-audit` tool
- `remove`: alias for `install` (set `hsesa_ssh_audit_package_state: absent`)
- `harden`: apply all SSH hardening tasks (server and client)
- `server`: only harden the SSH server
- `client`: only harden the SSH client

---

## Install this role

From Ansible Galaxy (recommended):

```bash
ansible-galaxy install HomeSecExplorer.sshaudit
```

Or manually (via Git):

```bash
git clone https://github.com/HomeSecExplorer/ansible-role-sshaudit.git roles/HomeSecExplorer.sshaudit
```

## Example playbook

```yaml
- name: Harden SSH
  hosts: servers
  become: true
  roles:
    - role: HomeSecExplorer.sshaudit
```

---

## Notes and tips

- Back up `/etc/ssh/sshd_config` and `/etc/ssh/moduli` before first run.
- Test on a lab host and keep an active SSH session while applying changes.
- **If you apply firewall rules, allow your management subnets first.**

---

## License

MIT

## Author Information

HomeSec Explorer  
🔗 [YouTube Channel](https://www.youtube.com/@HomeSecExplorer)

If this role was helpful, drop a ⭐ on GitHub, subscribe on YouTube or Sponsor me!
