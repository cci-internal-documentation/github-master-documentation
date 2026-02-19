```markdown
# Cuculus India – GitHub Enterprise Governance

**Owner:** Avnish Singh  
**Scope:** Enterprise structure, organization model, repositories, access control, CI/CD governance  
**Status:** v1.0  

---

# 1. Purpose

GitHub Enterprise is the centralized platform for managing:

- Product source code
- Customer-specific deployment configuration
- Kubernetes (GitOps) repositories
- CI/CD automation
- Technical documentation versioning
- Change tracking and audit compliance

All engineering and deployment work must be governed through this structure.

---

# 2. Enterprise Structure

GitHub is organized in three levels:

```

Cuculus India (Enterprise Account)
│
├── Organizations
│     ├── Teams
│     └── Repositories

```

- **Enterprise Account** → Company-level governance and license management  
- **Organization** → Project, customer, or program boundary  
- **Repository** → Version-controlled unit (code, config, docs, pipelines)

---

# 3. Organization Model

## 3.1 Core Product Organization

### `cuculus-products`

Contains all reusable product code:

- reporting-service  
- prepayment-service  
- jms-repush  
- hes-connector  
- shared-libraries  
- ci-templates  

This organization stores only product source code and shared components.

---

## 3.2 Customer Organizations

One organization per customer to ensure isolation.

### `cuculus-customer-pvvnl`
- PVVNL deployment config
- GitOps repositories
- Environment overlays
- Runbooks

### `cuculus-customer-dgvcl`
- DGVCL deployment config
- GitOps repositories
- Pipeline definitions

### `cuculus-customer-bypl`
- BYPL Kubernetes deployment
- GitOps config
- Release documentation

---

## 3.3 Program Organization

### `cuculus-program-ped-satnam`

Contains:
- Kubernetes platform configuration
- CD workflows
- Platform-level GitOps repositories
- Deployment documentation

---

# 4. Repository Categories

Repositories will fall into these types:

- **Product Code Repositories**
- **Customer Deployment Configuration**
- **GitOps Cluster State**
- **CI/CD Workflow Definitions**
- **Documentation Repositories**

---

# 5. Access Governance

## 5.1 Teams per Organization

Each organization should define:

- `system-engineering`
- `developers`
- `technical-writers` (if required)
- `release-devops` (optional)

---

## 5.2 Role Principles

- Least privilege access
- No direct pushes to protected branches
- Pull Request review mandatory
- Branch protection enabled
- Two-factor authentication required

---

# 6. Development Workflow

1. Create Issue  
2. Create Feature Branch  
3. Commit Changes  
4. Open Pull Request  
5. Review + CI Validation  
6. Merge to Main  
7. Automated Deployment  

All production changes must go through Pull Request approval.

---

# 7. CI/CD & Deployment Model

## Continuous Integration (CI)
- Automated build and tests on every Pull Request

## Continuous Deployment (CD)
- Automated deployment to Dev/UAT/Prod
- Production requires approval gate

## Runners
- GitHub-hosted runners (default)
- Self-hosted runners for customer/VPN environments

Self-hosted runners must be secured and isolated appropriately.

---

# 8. Security Policies

## 8.1 Secrets

The following must NOT be stored in repositories:

- Passwords
- API keys
- Certificates
- Private keys
- Tokens

Use encrypted secrets or an external secret management system.

---

## 8.2 Branch Protection

For critical repositories:

- Require Pull Request before merge
- Require at least one reviewer
- Require CI checks to pass
- Restrict force pushes

---

# 9. License Allocation (Initial 10 Seats)

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

At least two Enterprise Owners should be designated.

---

# 10. Naming Standards

## Organizations
- `cuculus-products`
- `cuculus-customer-<name>`
- `cuculus-program-<name>`

## Branch Naming
- `feature/<ticket>-description`
- `bugfix/<ticket>-description`
- `release/<version>`

---

# 11. Governance Ownership

Enterprise Administrator: **Avnish Singh**

Responsibilities:
- Organization creation
- Access management
- Policy enforcement
- CI/CD governance
- Security oversight

---

# 12. Scalability Model

When adding:

- New customer → Create new customer organization  
- New product → Add repository in `cuculus-products`  
- New program → Create program organization  

This structure ensures:

- Customer isolation  
- Controlled deployment pipelines  
- Audit-ready change management  
- Secure and scalable growth  
```
