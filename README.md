# ImmutableGate

**ImmutableGate** is a security-first infrastructure framework designed to treat physical hardware as an ephemeral, fully auditable sandbox. It provides a "physical harness" for AI agents and high-risk workloads by enforcing a strict **No-SSH, GitOps-only** operational model.

## The Core Philosophy
Traditional systems rely on human trust and mutable state (SSH). **ImmutableGate** replaces trust with **Verification & Immutability**. 
- **No SSH:** No backdoors, no manual hotfixes, no drift.
- **Git as Truth:** If it isn't in Git, it doesn't exist.
- **Hardware Sandbox:** The entire OS can be wiped and re-provisioned via a single administrative command.

---

## 🏗️ The Technology Stack
* **Host OS:** [Talos Linux](https://www.talos.dev/) — An API-managed, non-SSH, immutable Linux distribution.
* **Orchestration:** [Argo CD](https://argoproj.github.io/cd/) — Managing the "App-of-Apps" pattern for automated state convergence.
* **Provisioning:** [Ansible](https://www.ansible.com/) — Handling initial network bootstrapping and PXE orchestration.
* **Management:** `talosctl` — The sole bridge for administrative hardware resets.

---

## 🌟 Key Features

### 1. The "Red Button" Reset
Administrators retain ultimate authority. With a single `talosctl reset` command, the node wipes its state and returns to a "Zero State" in seconds. This makes it the perfect environment for testing autonomous AI agents that require a clean slate for every execution.

### 2. AI-Native Debugging (ReadOnly, WIP)
ImmutableGate provides a safe observation deck for AI. Using specialized **K8s RBAC**, AI agents can monitor:
* **Structured Logs:** Real-time stdout/stderr analysis.
* **K8s Events:** Automated diagnostics of lifecycle failures.
* *Note:* AI can suggest fixes via Git Pull Requests but lacks the permission to modify the running cluster directly.

### 3. Total Auditability
Since the system is immutable and lacks SSH, every single change—from a kernel parameter to an environment variable—is recorded in the Git history. This creates a 100% transparent audit trail for compliance and safety.

---

## 🔄 Workflow

1.  **Provision:** Ansible bootstraps the bare-metal node with Talos Linux.
2.  **Bootstrap:** The **Root Application** in Argo CD is deployed.
3.  **Converge:** Argo CD pulls the intended state from the Git repository and configures the environment.
4.  **Operate:** AI or applications run within the sandbox. Logs are streamed for observation.
5.  **Re-Initialize:** Administrator triggers a hardware reset, wiping the environment for the next cycle.

---

## 🛠️ Quick Start

### Prerequisites
* A machine capable of PXE booting and HTTP hosting.
* `ansible`, `kubectl`, `talosctl` and `sops` installed locally.
* A Git repository - [exaple](https://github.com/23hp/immutable-gate-apps) containing your Kubernetes manifests.

### Deployment
1. **Set PXE Server IP:** Update `roles/router/defaults/main.yml`.
1. **Define your node:** Update `roles/talos/defaults/main.yml`; Remove `host_vars/localhost.sops.yml` to bypass my secrets settings.
1. **Set the Apps repo:** Update `roles/argocd/defaults/main.yml`

1. **Prepare PXE envoriment:**
   ```bash
   ansible-galaxy install -r requirements.yml
   ansible-playbook pxe-setup.yml
   ```
1. **Apply the Gate:**
Create
   ```bash
   ansible-playbook os-activate.yml
   ```
1. **Reset the Sandbox:**
   ```bash
   # Wipe system
   ansible-playbook reset.yml
   # Or wipe system and apps data
   ansible-playbook destroy.yml
   ```

## 🔒 Security & Privacy (GDPR)
Designed with European privacy standards in mind. By keeping the AI diagnostics local and the infrastructure immutable, **ImmutableGate** ensures that data leakage is minimized and manual intervention (the most common source of security breaches) is eliminated.

---

## 📄 License
Distributed under the MIT License. See `LICENSE` for more information.
