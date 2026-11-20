# k3s-on-Proxmox Blueprint (Terraform + Ansible)

Repeatable infrastructure-as-code blueprint provisioning a high-availability **k3s Kubernetes cluster** on **Proxmox VE** using **Terraform** (`bpg/proxmox`) followed by **Ansible** configuration management and **Argo CD** GitOps bootstrap.

Companion repository to:
- [Provision k3s with Terraform and Ansible: A Repeatable Cluster Blueprint](https://razvanmares.ro/en/blog/provision-k3s-with-terraform-and-ansible-a-repeatable-cluster-blueprint)
- [The Control Plane Relay: Provisioning k3s with Terraform and Ansible](https://razvanmares.ro/en/blog/the-control-plane-relay-provisioning-k3s-with-terraform-and-ansible)
- [Inside My k3s-on-Proxmox Homelab: A Small Platform With Real Constraints](https://razvanmares.ro/en/blog/inside-my-k3s-on-proxmox-homelab-a-small-platform-with-real-constraints)

---

## Architecture Flow

```text
Proxmox VE Cloud-Init Template (VMID 9000)
   └─▶ Terraform (./terraform)
         ├─ Provisions k3s-cp-01..03 (embedded etcd) + k3s-wk-01..02
         ├─ Injects static IPs (RFC 5737 defaults), DNS & SSH public key
         └─ Emits generated-inventory.ini
               └─▶ Ansible Collection razvanapps.k3s_proxmox (./ansible)
                     ├─ Role: k3s_prereqs (swapoff, sysctl, modules)
                     ├─ Role: k3s_server (HA cluster-init + join)
                     ├─ Role: k3s_agent (worker node join)
                     └─ Role: argocd_bootstrap (GitOps control plane)
```

> **Terraform Registry Module**: The standalone root Terraform module is also mirrored at [`RazvanApps/terraform-proxmox-k3s`](https://github.com/RazvanApps/terraform-proxmox-k3s) to satisfy the Terraform Registry `terraform-<PROVIDER>-<NAME>` naming convention.

---

## Quick Start

### 1. Provision VMs with Terraform
```bash
cd terraform
terraform init
terraform plan -var="ssh_public_key=$(cat ~/.ssh/id_ed25519.pub)"
terraform apply -var="ssh_public_key=$(cat ~/.ssh/id_ed25519.pub)"
```

### 2. Configure k3s & Argo CD with Ansible
```bash
# Inject K3S_TOKEN from your secret manager into the shell environment first
ansible-playbook -i terraform/generated-inventory.ini ansible/playbooks/site.yml \
  -e "k3s_cluster_token=${K3S_TOKEN}"
```

### 3. Build Ansible Galaxy Collection Locally
```bash
ansible-galaxy collection build ansible/ --output-path ./artifacts
```

## Security & Defaults

All default IPv4 addresses in `terraform/variables.tf` use documentation CIDR `192.0.2.0/24` (RFC 5737). Supply your own LAN/VLAN subnet, Proxmox API token, SSH public key, and cluster token via `.tfvars` / environment variables outside version control.

## License

MIT — [Răzvan Mareș (RazvanApps)](https://razvanmares.ro)
