# k3s_prereqs Role

Prepares Ubuntu/Debian nodes for k3s by installing storage/transport prerequisites, disabling runtime swap, and configuring kernel modules and `sysctl` forwarding parameters.

## Requirements

- Debian 12 (Bookworm) or Ubuntu 22.04 / 24.04 LTS

## Example Playbook

```yaml
- hosts: k3s_cluster
  become: true
  roles:
    - razvanm4.k3s_proxmox.k3s_prereqs
```
