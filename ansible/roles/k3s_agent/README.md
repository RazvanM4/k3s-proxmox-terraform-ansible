# k3s_agent Role

Enrolls worker nodes into an existing k3s control plane.

## Variables

- `k3s_cluster_token`: Shared cluster join token passed via secret manager / environment variable.

## Example Playbook

```yaml
- hosts: k3s_agent
  become: true
  roles:
    - razvanm4.k3s_proxmox.k3s_agent
```
