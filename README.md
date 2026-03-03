# VPS Platform Automation ( Ansible + GitHub Actions)
This repository contains automation code used to configure manually provisioned VPS nodes using **Ansible**, executed via **GitHub Actions.**

## The design philosophy is:

🔥 Disposable infrastructure<br>
🔁 Rebuild anytime<br>
🧾 Everything defined as code<br>
🚫 No manual configuration drift<br>

## 🏗 Architecture Overview
```
Contabo Web UI → Create VPS
        ↓
Fresh Ubuntu Server
        ↓
GitHub Push
        ↓
GitHub Actions Runner
        ↓
Ansible Playbooks
        ↓
Configured Platform Nodes
```

Infrastructure is manually provisioned (VPS creation),<br>
but configuration is fully automated and reproducible.

## 🎯 Goals
- Keep VPS nodes clean and disposable
- Avoid configuration drift
- Practice Infrastructure as Code principles
- Demonstrate CI/CD-driven configuration management
- Build production-style automation portfolio

## 🖥 Node Layout

| Node	| Role |
| - | - |
| VPS-1	 | GitLab server |
| VPS-2	| Kubernetes control plane |
| VPS-3	| Kubernetes worker |

Each node starts as:
- Fresh Ubuntu install
- SSH key enabled
- No additional software

Everything else is configured by Ansible.

## 📂 Repository Structure
```
.
├── ansible/
│   ├── inventory/
│   │   └── hosts.ini
│   ├── roles/
│   │   ├── common/
│   │   ├── docker/
│   │   ├── gitlab/
│   │   └── k8s/
│   └── site.yml
│
└── .github/
    └── workflows/
        └── deploy.yml
```

## 🚀 How Deployment Works

1. Create VPS manually (Contabo)
- Deploy Ubuntu
- Add SSH public key
- Note the IP address
2. Update Inventory
```
ansible/inventory/hosts.ini
```
Add the new IP addresses.
3. Push to Github
```
git add .
git commit -m "Update VPS IPs"
git push
```
4. GitHub Actions Runs
The workflow:
- Installs Ansible
- Loads SSH private key from GitHub Secrets
- Executes playbooks
- Configures nodes

## 🔐 Secrets

The following secret must be set in GitHub:

| Secret Name | Purpose|
|-|-|
| VPS_SSH_KEY | Private SSH key for connecting to VPS |

## 🔁 Rebuilding The Platform
To refresh infrastructure:
1. Destroy VPS in Contabo
2. Recreate VPS
3. Update IP in inventory
4. Push changes to GitHub

Ansible will reconfigure the nodes from scratch.

This ensures:
- No manual state
- No configuration drift
- Fully reproducible setup

## 🧠 Design Principles
- Idempotent playbooks
- No manual SSH configuration
- Infrastructure treated as disposable
- CI-driven configuration
- Role-based Ansible structure

## 📦 Example Playbook Execution
The main playbook:
```
ansible-playbook -i inventory/hosts.ini site.yml
```

Executed automatically via GitHub Actions on every push to main.

## 🛠 Future Improvements
- Add Terraform for VPS provisioning
- Dynamic inventory
- Self-hosted GitHub runner
- Vault-based secret management
- Monitoring and observability stack

## 🎓 Why This Project
- This project demonstrates:
- Configuration management with Ansible
- CI/CD automation with GitHub Actions
- Secure SSH-based remote orchestration
- Infrastructure lifecycle thinking
- Platform engineering mindset

## 👤 Author
Michael Maldo<br>
Cloud / DevOps / Platform Engineering Focus

## ⭐ Summary

This project follows the principle:<br>
*Servers are cattle, not pets.*

VPS nodes can be destroyed and rebuilt at any time,<br>
with configuration fully managed through version-controlled automation.