# Homelab Automation & DevSecOps Stack — GitOps, CI/CD & Infrastructure as Code

> ArgoCD • Terraform • Ansible • GitHub Actions • Harbor + Cosign • SBOMs + Trivy • MCP Server

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](#license)
![Focus](https://img.shields.io/badge/Focus-Automation,_CI/CD,_DevSecOps-blue)
![Platform](https://img.shields.io/badge/Platform-Kubernetes,_IaC,_Pipelines-orange)

This repo defines the **automation and DevSecOps layer** of the homelab. It ensures all services are built, scanned, signed, and deployed in a **GitOps-first, security-by-default workflow**.

---

## ✨ Highlights
- **ArgoCD** for GitOps-based Kubernetes deployments
- **Terraform** for infra provisioning (Proxmox, cloud)
- **Ansible** for config management
- **GitHub Actions** pipeline for build → SBOM → scan → sign → attest
- **Harbor + Cosign** for image registry, signing, and vuln scanning
- **Trivy** for vulnerability scanning and compliance
- **MCP Server** for policy-driven automation (Model Context Protocol)

---

## 🧭 High-Level Architecture

```mermaid
flowchart LR
  %% Nodes
  Dev["Developer Commit"];
  GH["GitHub Repo"];
  CI["GitHub Actions CI/CD"];
  Img["Harbor Registry"];
  Argo["ArgoCD"];
  K8s["Kubernetes Cluster"];
  SBOM["SBOM + Trivy Scan"];
  Sign["Cosign Sign & Attest"];
  Verify["Policy Check (Kyverno / Cosign)"];
  TF["Terraform"];
  Infra["Proxmox / Cloud Infra"];
  Ansible["Ansible"];
  Nodes["Nodes"];
  MCP["MCP Server"];
  Policy["Automation Policies"];

  %% Edges
  Dev --> GH;
  GH -->|Build| CI;
  CI --> Img;
  Img --> Argo;
  Argo --> K8s;

  CI --> SBOM;
  CI --> Sign;
  Sign --> Img;
  Img --> Verify;

  TF --> Infra;
  Ansible --> Nodes;
  MCP --> Policy;

```

---

## 📁 Repo Layout
```
automation-devsecops-stack/
├─ README.md
├─ .env.example
├─ .github/
│  └─ workflows/
│     └─ build-scan-sign-attest.yml
├─ argocd/
│  └─ apps/
├─ terraform/
│  └─ README.md
├─ ansible/
│  └─ README.md
├─ mcp-server/
│  └─ README.md
├─ docs/
│  ├─ PIPELINE.md
│  ├─ TERRAFORM.md
│  ├─ ANSIBLE.md
│  ├─ ARGOCD.md
│  └─ SECURITY.md
└─ .gitignore
```

---

## 🚀 Quick Start

### CI/CD
- Modify `.github/workflows/build-scan-sign-attest.yml` to match your repo/registry.
- Push commit → GitHub Actions builds, scans, signs, and attests image.

### ArgoCD
- Place app manifests in `argocd/apps/`.
- ArgoCD syncs workloads automatically into cluster.

### Terraform
- Use `terraform/` modules to provision infra (VMs, storage, network).
- State stored securely (local or remote backend).

### Ansible
- Define playbooks/roles in `ansible/`.
- Automate node configs (Proxmox VMs, Kubernetes nodes).

### MCP Server
- Define automation policies in `mcp-server/`.
- Enforce rules for deployments and IaC usage.

---

## 🔐 Security Controls
- GitHub Actions: build SBOM (Syft), scan (Grype/Trivy).
- Cosign signing + attestation, verified by Kyverno in cluster.
- Terraform + Ansible configs under version control.
- Secrets handled via External Secrets Operator (not plaintext).

---

## 📊 Observability Hooks
- CI pipeline logs in GitHub Actions.
- Harbor vuln scan dashboards.
- ArgoCD app health metrics.
- Terraform plan/apply outputs stored as artifacts.

---

## 📌 Roadmap
- Add Conftest/OPA checks for IaC validation pre-merge.
- Integrate GitLeaks for secret scanning.
- Automate node provisioning fully with Terraform + Ansible.
- Tie MCP Server into SOC for automated remediation.

---

## 🔗 Related Stacks
- [Platform Stack](../homelab-platform-stack)
- [Infrastructure Stack](../homelab-infrastructure-stack)
- [Security SOC Stack](../homelab-security-soc-stack)

---

## 📝 License
MIT — see `LICENSE`
