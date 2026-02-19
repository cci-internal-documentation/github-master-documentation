**Document owner:** Avnish Singh\
**Audience:** Management & project leadership\
**Scope:** How Cuculus India will use GitHub Enterprise for source code,
customer deployment configuration, CI/CD, Kubernetes-based projects, and
technical documentation versioning.\
**Status:** Initial (v1.0)

- [1) Why we purchased GitHub Enterprise](#1-why-we-purchased-github-enterprise)
  - [Business purpose](#business-purpose)
- [What GitHub Enterprise is (in simple terms)](#what-github-enterprise-is-in-simple-terms)
- [2) Core concepts](#2-core-concepts)
  - [2.1 Enterprise account (Cuculus India)](#21-enterprise-account-cuculus-india)
  - [2.2 Organizations (project/customer containers)](#22-organizations-projectcustomer-containers)
  - [2.3 Teams (role-based access groups)](#23-teams-role-based-access-groups)
  - [2.4 Repositories ("repos")](#24-repositories-repos)
  - [2.5 Permissions and roles (how we control access)](#25-permissions-and-roles-how-we-control-access)
- [3) Cuculus India initial license allocation (10 seats)](#3-cuculus-india-initial-license-allocation-10-seats)
  - [3.1 Initial users (as proposed)](#31-initial-users-as-proposed)
    - [System Engineering (5)](#system-engineering-5)
      - [Technical Writers (1)](#technical-writers-1)
      - [Developers Team (4)](#developers-team-4)
  - [3.2 Recommended admin coverage (risk control)](#32-recommended-admin-coverage-risk-control)
- [4) Proposed structure for Cuculus India in GitHub Enterprise](#4-proposed-structure-for-cuculus-india-in-github-enterprise)
  - [4.1 The hierarchy (how it fits together)](#41-the-hierarchy-how-it-fits-together)
  - [4.2 Organization strategy options (choose and standardize)](#42-organization-strategy-options-choose-and-standardize)
    - [Option A --- "Org per Project/Customer" (your current plan)](#option-a-----org-per-projectcustomer-your-current-plan)
    - [Option B --- "Few Orgs + Many Repos" (alternative, not mandatory)](#option-b-----few-orgs--many-repos-alternative-not-mandatory)
- [5) Repository types we will maintain](#5-repository-types-we-will-maintain)
  - [5.1 Product/service code repositories](#51-productservice-code-repositories)
  - [5.2 Customer deployment configuration repositories](#52-customer-deployment-configuration-repositories)
  - [5.3 Kubernetes platform repos (GitOps style)](#53-kubernetes-platform-repos-gitops-style)
  - [5.4 CI/CD pipeline repos (if separated)](#54-cicd-pipeline-repos-if-separated)
  - [5.5 Documentation repositories (technical writers)](#55-documentation-repositories-technical-writers)
- [6) Standard working model (how changes flow)](#6-standard-working-model-how-changes-flow)
  - [6.1 What "version control" means](#61-what-version-control-means)
  - [6.2 Standard change workflow (recommended)](#62-standard-change-workflow-recommended)
- [7) Governance policies (what we will enforce)](#7-governance-policies-what-we-will-enforce)
  - [7.1 Repository visibility](#71-repository-visibility)
  - [7.2 Branch protection (quality \& stability)](#72-branch-protection-quality--stability)
  - [7.3 CODEOWNERS (ownership clarity)](#73-codeowners-ownership-clarity)
  - [7.4 Access control using roles](#74-access-control-using-roles)
  - [7.5 Auditability](#75-auditability)
- [8) CI/CD and Kubernetes deployments (management explanation)](#8-cicd-and-kubernetes-deployments-management-explanation)
  - [8.1 What CI/CD is](#81-what-cicd-is)
  - [8.2 GitHub Actions (automation engine)](#82-github-actions-automation-engine)
  - [8.3 Runners: GitHub-hosted vs self-hosted](#83-runners-github-hosted-vs-self-hosted)
  - [8.4 Environments (Dev/UAT/Prod controls)](#84-environments-devuatprod-controls)
- [9) Customer configuration storage rules (DGVCL, PVVNL, etc.)](#9-customer-configuration-storage-rules-dgvcl-pvvnl-etc)
  - [9.1 Separation principle](#91-separation-principle)
  - [9.2 What can be stored vs what must not be stored](#92-what-can-be-stored-vs-what-must-not-be-stored)
- [10) Documentation versioning for technical writers](#10-documentation-versioning-for-technical-writers)
  - [10.1 Why GitHub for documentation](#101-why-github-for-documentation)
  - [10.2 Proposed doc workflow](#102-proposed-doc-workflow)
- [11) Operating procedures (SOP) --- what I will manage](#11-operating-procedures-sop-----what-i-will-manage)
  - [11.1 Creating a new Organization (per project/customer)](#111-creating-a-new-organization-per-projectcustomer)
  - [11.2 Standard teams inside each org](#112-standard-teams-inside-each-org)
  - [11.3 Onboarding/offboarding users](#113-onboardingoffboarding-users)
- [12) What management should expect (visibility \& outcomes)](#12-what-management-should-expect-visibility--outcomes)
  - [12.1 What improves immediately](#121-what-improves-immediately)
  - [12.2 What management can measure](#122-what-management-can-measure)
- [13) Risks and mitigations](#13-risks-and-mitigations)
  - [Risk 1 --- Over-segmentation (too many orgs)](#risk-1-----over-segmentation-too-many-orgs)
  - [Risk 2 --- Secrets leakage](#risk-2-----secrets-leakage)
  - [Risk 3 --- Single point of failure (one admin)](#risk-3-----single-point-of-failure-one-admin)
  - [Risk 4 --- Unsafe self-hosted runner posture](#risk-4-----unsafe-self-hosted-runner-posture)
- [14) Glossary (quick reference)](#14-glossary-quick-reference)
- [Appendix A --- Suggested default permissions (baseline)](#appendix-a-----suggested-default-permissions-baseline)
  - [Inside each organization](#inside-each-organization)
- [Appendix B --- Proposed naming conventions (optional but recommended)](#appendix-b-----proposed-naming-conventions-optional-but-recommended)
  - [Organizations:](#organizations)
  - [Repositories:](#repositories)
  - [Branches:](#branches)

# 1) Why we purchased GitHub Enterprise

## Business purpose

GitHub Enterprise will be Cuculus India's **central system of record**
for:

1.  []{#_Toc222325993 .anchor}Product source code repositories\
    Examples: reporting, prepayment, JMS queue repush, HES Connector,
    and similar product modules.

2.  []{#_Toc222325994 .anchor}Customer environment configuration
    repositories\
    Examples: DGVCL, PVVNL (and other customer deployments).\
    This includes Kubernetes manifests / Helm charts / deployment
    parameters **without embedding secrets** (see security section).

3.  []{#_Toc222325995 .anchor}Kubernetes-based delivery programs + CD
    pipelines\
    Examples: PED-Satnam, CCI-SAS (includes DVC & IPCL), BYPL.\
    Includes **deployment automation (CD)** and operational runbooks as
    code.

4.  []{#_Toc222325996 .anchor}Technical documentation version control\
    Technical writers will store documentation in repos to ensure:

    - history of changes

    - approvals via review workflow

    - ability to restore older versions

# What GitHub Enterprise is (in simple terms)

GitHub is like a **high-control shared drive** for code and documents,
with:

- full change history ("who changed what, when, and why")

- structured review/approval before changes are accepted

- automation for build/test/deploy (CI/CD)

- access control per project/customer

GitHub Enterprise adds **centralized administration** and policy control
at company level ("enterprise account"), and lets us manage many
separate project spaces ("organizations") under one umbrella. ([GitHub
Docs](https://docs.github.com/en/enterprise-cloud%40latest/admin/concepts/enterprise-fundamentals/enterprise-accounts?utm_source=chatgpt.com))

# 2) Core concepts 

## 2.1 Enterprise account (Cuculus India)

The **Enterprise account** is the top-level container for the company.
It is the **central point of administration** for policies, access
governance, and billing across multiple organizations. ([GitHub
Docs](https://docs.github.com/en/enterprise-cloud%40latest/admin/concepts/enterprise-fundamentals/enterprise-accounts?utm_source=chatgpt.com))

**Key management takeaway:**\
Enterprise = company-wide governance layer.

## 2.2 Organizations (project/customer containers)

An **Organization** is a controlled workspace where we host repositories
and manage people via teams. GitHub explicitly positions organizations
as a way to collaborate across many people/projects while controlling
access and settings. ([GitHub
Docs](https://docs.github.com/en/enterprise-cloud%40latest/organizations?utm_source=chatgpt.com))

**Key management takeaway:**\
Organization = project/customer/program boundary.

**Your stated plan:** "For every project we will create organizations in
the Cuculus Enterprise and then add members to the relevant
organization."

This is a valid model, especially when **customer segregation** and
permission boundaries are important.

## 2.3 Teams (role-based access groups)

A **Team** is a named group of users inside an organization (e.g.,
Developers, System Engineering, Technical Writers). Teams are used to
grant access to multiple repos consistently.

**Key management takeaway:**\
Teams = scalable access control (avoid giving permissions repo-by-repo
per person).

## 2.4 Repositories ("repos")

A **Repository** is a secure folder with history, typically containing:

- application code (backend/frontend/services)

- configuration (deployment manifests, Helm charts)

- documentation (Markdown/Asciidoc/diagrams)

- pipeline definitions (GitHub Actions workflow YAML)

**Key takeaway:**\
Repo = the unit of ownership, change control, and audit trail.

## 2.5 Permissions and roles (how we control access)

Access is controlled through **permissions grouped into roles**. GitHub
defines permissions as the ability to perform actions, and roles as sets
of permissions. ([GitHub
Docs](https://docs.github.com/en/organizations/managing-peoples-access-to-your-organization-with-roles/roles-in-an-organization?utm_source=chatgpt.com))

At repository level, GitHub provides standard roles (from least to most
access) such as **Read, Triage, Write, Maintain, Admin**. ([GitHub
Docs](https://docs.github.com/en/organizations/managing-user-access-to-your-organizations-repositories/managing-repository-roles/repository-roles-for-an-organization?utm_source=chatgpt.com))

**Key takeaway:**\
We can ensure "least privilege": people only get the access they need.

# 3) Cuculus India initial license allocation (10 seats)

## 3.1 Initial users (as proposed)

### System Engineering (5)

1.  Kukkadotti Leelaja

2.  Chandan Haldar

3.  Avnish Singh

4.  Kalyan Panathula

5.  Muthuprakash

#### Technical Writers (1)

1.  Nilesh Gunjal

#### Developers Team (4)

1.  Indranil Sarkar

2.  Emran Khan

3.  Probal

4.  Teja

## 3.2 Recommended admin coverage (risk control)

To avoid "single-admin dependency," define:

- **Enterprise Owners (2 people recommended):**

  - Primary: Avnish Singh

  - Backup: cci-ita

This is a governance/safety requirement: if the only admin is
unavailable, operations stop (onboarding, permissions, emergency fixes).

# 4) Proposed structure for Cuculus India in GitHub Enterprise

## 4.1 The hierarchy (how it fits together)

Cuculus India (Enterprise Account)

└── Organization (per project/customer/program)

├── Teams (System Engineering / Developers / Tech Writers / DevOps)

└── Repositories (code / config / docs / pipelines)

This aligns with how GitHub Enterprise accounts centrally manage
multiple organizations. ([GitHub
Docs](https://docs.github.com/en/enterprise-cloud%40latest/admin/concepts/enterprise-fundamentals/enterprise-accounts?utm_source=chatgpt.com))

## 4.2 Organization strategy options (choose and standardize)

### Option A --- "Org per Project/Customer" (your current plan)

Examples:

- Org: cci-internal-project-reporting

- Org: cci-internal-project-prepayment

- Org: cci-internal-project-jms-queue-repush

- Org: cci-customer-dgvcl

- Org: cci-customer-pvvnl

- Org: cci-customer-ped

- Org: cci-customer-zonos-sas

- Org: cci-customer-bypl

**Pros**

- strong access separation (especially customer config)

- easier to prevent accidental cross-customer visibility

- simpler audit boundary per engagement

**Cons**

- more admin overhead (more org settings to maintain)

- cross-project shared libraries need careful handling (shared org or
  package repo)

### Option B --- "Few Orgs + Many Repos" (alternative, not mandatory)

Examples:

- Org: products (all product code repos)

- Org: customer-config (customer environment config repos)

- Org: platform-devops (shared deployment tooling/pipelines)

- Org: docs (technical writing repos)

**Pros**

- easier to administer

- simpler to enforce consistent policy

**Cons**

- higher risk of accidentally granting broader access if not careful

*Recommendation for Cuculus right now (pragmatic):*\
Use **Option A** for customer-specific config and high-sensitivity
projects, and optionally use a **single "shared" org** for common
libraries/tools that must be reused across projects.

# 5) Repository types we will maintain

To keep things predictable, define standard repository categories:

## 5.1 Product/service code repositories

Purpose: application code for reporting, prepayment, JMS repush,
connectors, etc.

**Example repo names**

- reporting-service

- prepayment-service

- jms-repush-worker

- hes-connector

## 5.2 Customer deployment configuration repositories

Purpose: store Kubernetes manifests/Helm values, environment overlays,
deployment notes.

**Example repo names**

- dgvcl-deployment-config

- pvvnl-deployment-config

**Critical rule:** Do **not** store customer passwords, tokens, private
keys in GitHub. Store only non-secret configuration and references to
secret stores.

## 5.3 Kubernetes platform repos (GitOps style)

Purpose: represent the desired cluster state (namespaces, deployments,
services, ingress, config maps, etc.)

**Example repo names**

- ped-satnam-gitops

- cci-sas-gitops

## 5.4 CI/CD pipeline repos (if separated)

Purpose: reusable workflows, shared actions, pipeline templates.

**Example repo names**

- ci-templates

- cd-templates

*(Alternatively, pipelines live inside each application repo, which is
common.)*

## 5.5 Documentation repositories (technical writers)

Purpose: product documentation, release notes, deployment guides,
runbooks.

**Example repo names**

- reporting-docs

- deployment-runbooks

- api-docs

# 6) Standard working model (how changes flow)

## 6.1 What "version control" means

Every change is stored as a **commit**, which includes:

- author identity

- timestamp

- exact before/after difference

- message describing the change

This gives traceability and rollback capability.

## 6.2 Standard change workflow (recommended)

1.  **Create an Issue** (track the work request)

2.  **Create a branch** (safe working copy)

3.  **Make commits** (incremental changes)

4.  **Open a Pull Request (PR)** (request review/approval)

5.  **Review + automated checks** (quality gate)

6.  **Merge to main** (accepted into official version)

7.  **CI/CD runs** (build/test/deploy)

This is the core governance loop: *no unreviewed change goes directly
into the main branch*.

# 7) Governance policies (what we will enforce)

## 7.1 Repository visibility

- Default: **Private** repositories (not visible publicly)

- Public repositories only with explicit management approval

## 7.2 Branch protection (quality & stability)

For each critical repo:

- protect main (or master) branch

- require PR reviews before merge

- require status checks (build/test) to pass before merge

- restrict who can push directly to main

## 7.3 CODEOWNERS (ownership clarity)

Define code owners per area/module so reviews automatically route to the
right responsible engineers.

## 7.4 Access control using roles

Use standard GitHub roles to assign least privilege. ([GitHub
Docs](https://docs.github.com/en/organizations/managing-user-access-to-your-organizations-repositories/managing-repository-roles/repository-roles-for-an-organization?utm_source=chatgpt.com))

A practical baseline:

- **Developers:** Write (create branches, PRs)

- **System Engineering / Leads:** Maintain or Admin (depending on
  responsibility)

- **Technical Writers:** Write access only on documentation repos; Read
  elsewhere

## 7.5 Auditability

GitHub Enterprise provides centralized administration and (typically)
audit/visibility features at enterprise scale. ([GitHub
Docs](https://docs.github.com/en/enterprise-cloud%40latest/admin/concepts/enterprise-fundamentals/enterprise-accounts?utm_source=chatgpt.com))\
We will use this to support:

- access reviews

- change traceability

- incident analysis when needed

# 8) CI/CD and Kubernetes deployments (management explanation)

## 8.1 What CI/CD is

- **CI (Continuous Integration):** automatically build/test when code
  changes

- **CD (Continuous Delivery/Deployment):** automatically deploy approved
  versions to environments (Dev/UAT/Prod)

## 8.2 GitHub Actions (automation engine)

GitHub Actions is GitHub's workflow automation system:

- Workflows are defined as code (YAML) in the repo

- Jobs run on "runners" (machines that execute the tasks)

## 8.3 Runners: GitHub-hosted vs self-hosted

- **GitHub-hosted runner:** managed by GitHub

- **Self-hosted runner:** a machine we deploy/manage (VM/server) to run
  workflows inside our network

GitHub defines a self-hosted runner as a system you deploy and manage to
execute GitHub Actions jobs, giving more control but making us
responsible for updates and costs. ([GitHub
Docs](https://docs.github.com/en/actions/concepts/runners/self-hosted-runners?utm_source=chatgpt.com))

*Why self-hosted runners matter for Cuculus:*\
Customer deployments (DGVCL/PVVNL/etc.) often require network
reachability that is only available from within customer/VPN networks.
In such cases, a self-hosted runner placed appropriately is the standard
approach.

## 8.4 Environments (Dev/UAT/Prod controls)

GitHub Actions supports **Environments** where:

- environment-specific secrets are stored

- approvals can be required before a job accesses production secrets /
  performs production deployment ([GitHub
  Docs](https://docs.github.com/en/actions/reference/workflows-and-actions/deployments-and-environments?utm_source=chatgpt.com))

*Important security note:*\
Workflows running on self-hosted runners are **not isolated** the way
containerized environments might be; environment secrets must be handled
with strong controls. ([GitHub
Docs](https://docs.github.com/en/actions/reference/workflows-and-actions/deployments-and-environments?utm_source=chatgpt.com))

# 9) Customer configuration storage rules (DGVCL, PVVNL, etc.)

## 9.1 Separation principle

Customer deployments should be isolated by:

- separate organization (strongest boundary) **or**

- separate private repos with restricted teams

Given your plan, the clean approach is:

- **one org per customer** (or per customer-program), containing only
  that customer's config repos.

## 9.2 What can be stored vs what must not be stored

**Allowed in GitHub**

- Kubernetes manifests

- Helm chart values (non-secret)

- environment overlays (dev/uat/prod) *without credentials*

- deployment instructions/runbooks

**Not allowed in GitHub**

- passwords, API keys, private certificates, tokens

- customer confidential data dumps/logs

**How we handle secrets**

- store secrets in a secret manager (Vault / cloud secret manager /
  sealed secrets / external secrets)

- GitHub stores only references or encrypted artifacts where appropriate

# 10) Documentation versioning for technical writers

## 10.1 Why GitHub for documentation

- every edit is tracked (history)

- review/approval workflow exists (PR review)

- supports collaborative writing (Markdown/Asciidoc)

- supports "release notes per version" tied to code releases

## 10.2 Proposed doc workflow

1.  Writer creates a branch for doc update

2.  Opens PR for review (engineering + product lead)

3.  After approval, merge to main

4.  Tag doc release matching software version (optional)

# 11) Operating procedures (SOP) --- what I will manage

## 11.1 Creating a new Organization (per project/customer)

**Inputs required**

- org name (standard naming convention)

- business owner (lead)

- technical owner(s)

- teams + members

- initial repos to create

**Steps**

1.  Create org under Cuculus enterprise

2.  Create standard teams (see below)

3.  Invite members and assign to teams

4.  Configure org-level policies (2FA, repo creation rights, Actions
    policies)

5.  Create repos from templates (code/config/docs)

6.  Apply branch protection/rulesets to critical repos

## 11.2 Standard teams inside each org

- system-engineering

- developers

- technical-writers *(optional per org)*

- release-devops *(optional but recommended for CD control)*

## 11.3 Onboarding/offboarding users

**Onboarding**

- assign seat

- enforce 2FA policy

- add to correct org/team(s)

**Offboarding**

- remove from org(s)

- review/revoke tokens and runner access

- confirm no sole ownership remains (avoid "orphaned" repos)

# 12) What management should expect (visibility & outcomes)

## 12.1 What improves immediately

- single source of truth for code/config/docs

- controlled changes (reviews instead of direct edits)

- audit-ready traceability

- repeatable deployments with reduced manual error

## 12.2 What management can measure

- deployment frequency (per product/customer)

- lead time for changes (PR cycle time)

- number of production incidents caused by config drift

- compliance indicators (2FA adoption, review adherence)

# 13) Risks and mitigations

## Risk 1 --- Over-segmentation (too many orgs)

**Impact:** overhead in setup and maintaining consistent policies\
**Mitigation:** use templates + standardized team structure; optionally
maintain a "shared" org for common components.

## Risk 2 --- Secrets leakage

**Impact:** high (customer/security exposure)\
**Mitigation:** strict rule "no secrets in GitHub," enable secret
detection controls where available, use environment approvals and tight
runner governance. ([GitHub
Docs](https://docs.github.com/en/actions/reference/workflows-and-actions/deployments-and-environments?utm_source=chatgpt.com))

## Risk 3 --- Single point of failure (one admin)

**Impact:** operational blockage\
**Mitigation:** assign at least one backup Enterprise Owner.

## Risk 4 --- Unsafe self-hosted runner posture

**Impact:** secrets exposure / pipeline compromise\
**Mitigation:** isolate runners per environment/customer where needed;
restrict workflow permissions; require approvals for prod; treat runners
as sensitive infrastructure. ([GitHub
Docs](https://docs.github.com/en/actions/concepts/runners/self-hosted-runners?utm_source=chatgpt.com))

# 14) Glossary (quick reference)

- **Enterprise account:** top-level company administration boundary.
  ([GitHub
  Docs](https://docs.github.com/en/enterprise-cloud%40latest/admin/concepts/enterprise-fundamentals/enterprise-accounts?utm_source=chatgpt.com))

- **Organization:** workspace for teams + repos. ([GitHub
  Docs](https://docs.github.com/en/enterprise-cloud%40latest/organizations?utm_source=chatgpt.com))

- **Repository (repo):** versioned project folder (code/config/docs).

- **Branch:** parallel line of work (safe change area).

- **Pull Request (PR):** request to merge changes after review.

- **Runner:** machine that executes CI/CD workflows. ([GitHub
  Docs](https://docs.github.com/en/actions/concepts/runners/self-hosted-runners?utm_source=chatgpt.com))

- **Environment:** deployment target grouping with secrets/approvals.
  ([GitHub
  Docs](https://docs.github.com/en/actions/reference/workflows-and-actions/deployments-and-environments?utm_source=chatgpt.com))

# Appendix A --- Suggested default permissions (baseline)

## Inside each organization

- system-engineering team: **Maintain** (or Admin if they must manage
  settings)

- developers team: **Write**

- technical-writers team: **Write** only on \*-docs repos; **Read**
  elsewhere

- release-devops team: **Maintain/Admin** on deployment repos and
  Actions settings (if separation is needed)

Repository roles are standard and intended to match function without
giving unnecessary access. ([GitHub
Docs](https://docs.github.com/en/organizations/managing-user-access-to-your-organizations-repositories/managing-repository-roles/repository-roles-for-an-organization?utm_source=chatgpt.com))

# Appendix B --- Proposed naming conventions (optional but recommended)

## Organizations:

- cuculus-project-\<project_name\> (e.g., cuculus-reporting)

- cci-customer-\<name\> (e.g., cuculus-customer-dgvcl)

## Repositories:

- code: \<service-name\>

- config: \<customer\>-\<program\>-config

- gitops: \<program\>-gitops

- docs: \<product\>-docs

## Branches:

- feature/\<ticket\>-\<short-desc\>

- bugfix/\<ticket\>-\<short-desc\>

- release/\<version\>
