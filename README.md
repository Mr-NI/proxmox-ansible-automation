# Automated Infrastructure-as-Code (IaC) Deployment for Proxmox

## 📌 Project Overview
This project focuses on automating the lifecycle of Virtual Machines within a **Proxmox Virtual Environment (PVE)**. By leveraging **Ansible** and the **Proxmox API**, this solution replaces manual GUI-based configurations with a "Single Source of Truth" codebase, ensuring rapid, consistent, and scalable infrastructure deployment.

---

## 🛠️ Technology Stack
* **Automation Engine:** Ansible (v2.15+)
* **Control Node:** Ubuntu 24.04 (Mac Mini)
* **Target Hypervisor:** Proxmox VE 8.x
* **Primary Collection:** `community.proxmox` (Native API integration)

---

## 📝 Problem Statement
### Current Manual Process
Deploying a VM currently requires a series of manual steps via the Proxmox Web GUI, including resource allocation, ISO mounting, and manual OS installation. This "bespoke" method is:
* **Time-Intensive:** ~20 minutes per VM.
* **Error-Prone:** High risk of configuration drift.
* **Non-Scalable:** Manual effort scales linearly with the number of nodes.

### The Solution
By treating infrastructure as code, we reduce deployment time to **under 90 seconds** and ensure that every node is identical and hardened by default.

---

## 🏗️ Technical Approach & Workflow
The **Ubuntu Control Node** sends instructions via the **Proxmox API**. Instead of a standard ISO install, we utilize **Cloud-Init** templates for near-instant cloning and configuration.

### Implementation Steps:
1. **Template Creation:** Build a "Gold Master" Ubuntu Cloud-Init image.
2. **API Authentication:** Use PVE API Tokens (stored securely via `ansible-vault`).
3. **Playbook Execution:** Define VM specs (CPU, RAM, Network) in YAML.
4. **Validation:** Automated SSH "Ping" and service verification.

---

## ⚠️ Risk Analysis
| Risk | Severity | Mitigation |
| :--- | :--- | :--- |
| API Token Exposure | High | Utilize `ansible-vault` for encryption. |
| Resource Over-provisioning | Medium | Pre-deployment hardware checks via `proxmox_query`. |
| Network Conflicts | Low | Static DHCP reservation or Ansible-managed IPAM. |

---

## 🧪 Testing Strategy
* **Syntax Validation:** `ansible-lint` for code quality.
* **Dry Run:** `ansible-playbook --check` to simulate changes.
* **Functional Test:** Post-deployment health checks to verify service uptime (e.g., Nginx/Docker).

---

## 🚀 Future Enhancements
* **CI/CD Integration:** Trigger deployments via GitHub Actions.
* **Self-Healing:** Auto-recreation of failed nodes detected by monitoring tools.
* **Multi-Cloud:** Extending playbooks to support hybrid-cloud (AWS/Azure) environments.

---
