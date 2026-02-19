```markdown
# Cuculus India – GitHub Enterprise Governance & Structure

**Owner:** Avnish Singh  
**Scope:** Enterprise-level GitHub structure, organization model, repository strategy, access governance, and CI/CD management  
**Audience:** Management, Engineering, DevOps, Technical Writers  
**Status:** Initial Version (v1.0)

---

# 1. Purpose

This repository documents how Cuculus India uses **GitHub Enterprise** to:

- Manage product source code
- Store customer-specific deployment configuration
- Operate Kubernetes-based delivery models
- Control CI/CD pipelines
- Maintain technical documentation with version control
- Enforce governance, security, and auditability

This document serves as the **authoritative internal reference** for GitHub structure and management decisions.

---

# 2. GitHub Enterprise Structure

GitHub Enterprise is organized in three layers:

```

Enterprise (Cuculus India)
│
├── Organizations (per customer or program)
│     ├── Teams
│     └── Repositories

```

## 2.1 Enterprise Account
The top-level administrative boundary.
- Centralized governance
- License management
- Policy enforcement
- Security controls

---

# 3. Organization Strategy

## Guiding Principle

> One organization per customer or major program.  
> One shared organization for product source code.

This ensures:
- Strong access isolation
- Clear audit boundaries
- No accidental cross-customer visibility
- Scalable expansion

---

# 4. Current Approved Organizations

## 4.1 `cuculus-products`

**Purpose:**  
All core product source code and shared components.

**Contains:**
- reporting-service
- prepayment-service
- jms-repush
- hes-connector
- shared-libraries
- ci-templates

**Access:**  
Developers + System Engineering

---

## 4.2 `cuculus-customer-pvvnl`

**Purpose:**  
PVVNL-specific deployment configuration and GitOps.

**Contains:**
- pvvnl-deployment-config
- pvvnl-gitops
- environment overlays
- runbooks

---

## 4.3 `cuculus-customer-dgvcl`

**Purpose:**  
DGVCL deployment and configuration.

**Contains:**
- dgvcl-deployment-config
- dgvcl-gitops
- pipeline definitions

---

## 4.4 `cuculus-customer-bypl`

**Purpose:**  
BYPL Kubernetes-based deployment management.

**Contains:**
- bypl-gitops
- bypl-config
- release documentation

---

## 4.5 `cuculus-program-ped-satnam`

**Purpose:**  
Program-level Kubernetes and deployment management.

**Contains:**
- ped-satnam-gitops
- platform configuration
- CD workflows
- deployment documentation

---

# 5. Repository Categories

Repositories fall into the following types:

## 5.1 Product Code Repositories
Application source code.
- Backend services
- Connectors
- Workers
- APIs

## 5.2 Customer Configuration Repositories
- Kubernetes manifests
- Helm values
- Environment overlays
- Deployment instructions

## 5.3 GitOps Repositories
- Desired cluster state
- Namespace definitions
- Ingress/service definitions

## 5.4 CI/CD Repositories
- Workflow templates
- Shared automation scripts

## 5.5 Documentation Repositories
- Product documentation
- Release notes
- Deployment guides
- Operational runbooks

---

# 6. Access Governance

## 6.1 Teams

Each organization should include:

- `system-engineering`
- `developers`
- `technical-writers` (if required)
- `release-devops` (optional but recommended)

---

## 6.2 Role Model

| Role | Access Level | Responsibility |
|------|--------------|---------------|
| Developers | Write | Code changes via PR |
| System Engineering | Maintain/Admin | Repo governance |
| Technical Writers | Write (docs only) | Documentation updates |
| Enterprise Owner | Admin | Org & policy control |

---

## 6.3 Principles

- Least privilege access
- No direct pushes to `main`
- Mandatory pull request reviews
- Enforced branch protection
- 2FA mandatory for all users

---

# 7. Standard Development Workflow

1. Create Issue
2. Create Feature Branch
3. Commit Changes
4. Open Pull Request
5. Code Review
6. CI Validation
7. Merge to Main
8. CD Deployment

No direct commits to protected branches.

---

# 8. CI/CD & Kubernetes Deployment Model

## 8.1 CI (Continuous Integration)
- Automated build & testing
- Runs on every Pull Request

## 8.2 CD (Continuous Deployment)
- Automated deployment via GitHub Actions
- Dev/UAT/Prod separation
- Approval gates for production

## 8.3 Runners

Two possible models:

- GitHub-hosted runners
- Self-hosted runners (for internal/VPN/customer deployments)

Self-hosted runners must:
- Be secured
- Be isolated per environment when required
- Not expose secrets

---

# 9. Security Policies

## 9.1 Secrets Management

The following must NEVER be stored in repositories:
- Passwords
- API keys
- Certificates
- Private keys
- Tokens

Secrets must be stored in:
- GitHub encrypted secrets
- External secret manager (preferred)

---

## 9.2 Branch Protection

For all production repositories:
- Require Pull Request before merge
- Require at least 1 review
- Require CI checks to pass
- Restrict force pushes

---

## 9.3 Auditability

All changes are:
- Traceable
- Time-stamped
- Linked to user identity
- Recoverable

---

# 10. License Allocation (Initial 10 Seats)

### System Engineering
- Kukkadotti Leelaja
- Chandan Haldar
- Avnish Singh
- Kalyan Panathula
- Muthuprakash

### Technical Writer
- Nilesh Gunjal

### Developers
- Indranil Sarkar
- Emran Khan
- Probal
- Teja

At least two Enterprise Owners should be defined to avoid operational dependency on a single administrator.

---

# 11. Naming Conventions

## Organizations
- `cuculus-products`
- `cuculus-customer-<name>`
- `cuculus-program-<name>`

## Repositories
- `<service-name>`
- `<customer>-deployment-config`
- `<program>-gitops`
- `<product>-docs`

## Branches
- `feature/<ticket>-description`
- `bugfix/<ticket>-description`
- `release/<version>`

---

# 12. Change Management

Any modification to:
- Organization structure
- Access model
- Security policy
- Deployment architecture

Must be documented via Pull Request in this repository.

---

# 13. Future Expansion

When adding:
- New customers → Create new customer organization
- New products → Add to `cuculus-products`
- New programs → Create program organization

This structure is designed to scale beyond the initial 10 licenses.

---

# 14. Governance Ownership

**Enterprise Administrator:** Avnish Singh  
Responsible for:
- Organization creation
- Access approvals
- Policy enforcement
- CI/CD governance
- Security oversight

---
