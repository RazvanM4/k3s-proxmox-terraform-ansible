# k3s_server Role

Bootstraps the initial k3s embedded-etcd control-plane node and joins additional high-availability server nodes.

## Variables

- `k3s_cluster_token`: Shared cluster join token passed via secret manager / environment variable.

## Example Playbook

```yaml
- hosts: k3s_server
  become: true
  roles:
    - razvanm4.k3s_proxmox.k3s_server
```
