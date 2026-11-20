# argocd_bootstrap Role

Installs declarative Argo CD manifests into the `argocd` namespace on the primary k3s control-plane node for GitOps cluster reconciliation.

## Example Playbook

```yaml
- hosts: k3s_server[0]
  become: true
  roles:
    - razvanm4.k3s_proxmox.argocd_bootstrap
```
