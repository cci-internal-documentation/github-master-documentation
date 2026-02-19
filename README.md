# Cuculus India -- GitHub Enterprise Governance (Summary)

**Owner:** Avnish Singh\
**Scope:** Enterprise structure, repositories, access control, CI/CD,
security

------------------------------------------------------------------------

## 1. Purpose

GitHub Enterprise is the centralized platform for:

-   Product source code\
-   Customer-specific deployment configuration\
-   Kubernetes (GitOps) repositories\
-   CI/CD automation\
-   Version-controlled documentation

All engineering and deployment activities must follow this structure.

------------------------------------------------------------------------

## 2. Enterprise Structure

    Enterprise (Cuculus India)
    │
    ├── Organizations
    │     ├── Teams
    │     └── Repositories

-   **Enterprise** → Governance & license control\
-   **Organization** → Product, customer, or program boundary\
-   **Repository** → Code, configuration, or documentation unit

------------------------------------------------------------------------

## 3. Organization Model

### Core Products

`cuculus-products`\
Reusable product code and shared components.

### Customer Organizations

`cuculus-customer-<name>`\
Customer-specific deployment config, GitOps, and release artifacts.

### Program Organizations

`cuculus-program-<name>`\
Platform-level configuration and workflows.

------------------------------------------------------------------------

## 4. Access Governance

Each organization defines teams:

-   `system-engineering`\
-   `developers`\
-   `technical-writers`\
-   `release-devops`

Principles:

-   Least privilege access\
-   Branch protection enabled\
-   Pull Request review mandatory\
-   No direct pushes to protected branches\
-   Two-factor authentication required

------------------------------------------------------------------------

## 5. Development Workflow

1.  Create Issue\
2.  Create Feature Branch\
3.  Commit Changes\
4.  Open Pull Request\
5.  Review + CI Validation\
6.  Merge to `main`\
7.  Automated Deployment

All production changes require PR approval.

------------------------------------------------------------------------

## 6. Security Controls

-   No secrets in repositories\
-   Use encrypted secrets or approved secret manager\
-   CI checks required before merge\
-   Force pushes restricted

------------------------------------------------------------------------

## 7. Scalability Model

-   New customer → New customer organization\
-   New product → Repository in `cuculus-products`\
-   New program → New program organization

This ensures customer isolation, secure pipelines, and audit-ready
change management.
