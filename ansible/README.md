# k3s-proxmox Ansible Collection

Collection namespace: `razvanm4.k3s_proxmox`.

Included roles:
- `k3s_prereqs`: OS hardening, sysctl parameters, swap disable, kernel modules (`br_netfilter`, `overlay`).
- `k3s_server`: Embedded etcd high-availability control-plane bootstrap.
- `k3s_agent`: Worker node enrollment via dedicated cluster token.
- `argocd_bootstrap`: Declarative Argo CD installation for GitOps cluster reconciliation.
