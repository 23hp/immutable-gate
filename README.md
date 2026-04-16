# 🛡️ Immutable HomeLab v2
**Talos Linux • Ansible • ArgoCD • GitOps**

### Built to be Immutable. Designed to be AI-Resistant.
A production-grade, declarative infrastructure designed to self-heal and resist unauthorized tampering. By leveraging a **Zero-Trust** OS and **GitOps** continuous delivery, this lab achieves total security sovereignty and state consistency.

* **OS:** Talos Linux (API-managed, diskless-capable, immutable)
* **Provisioning:** Ansible (Bootstrap & System Lifecycle)
* **Orchestration:** ArgoCD (App-of-Apps pattern, automated drift correction)
* **Focus:** Privacy, Security, and AI-driven threat resilience.

## Prerequisite
- ansible
- talosctl
- kubectl
- [sops](https://github.com/getsops/sops)
- [Bitwarden Secret Manager CLI](https://github.com/bitwarden/sdk-sm)

## Initialization
#### ansible
```bash
    ansible-galaxy install -r requirements.yml
    ansible-playbook operate-system.yml
    ansible-playbook setup-argocd.yml
```

### Secret Management
```bash
    # quickly view/change secrets
    bws run -- sops  secrets.sops.yaml
    # In-place encryption
    bws run -- sops -e -i localhost.sops.yml

```
