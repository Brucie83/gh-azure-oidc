# GitHub Actions OIDC with Azure (Security Demo)

This repository demonstrates how to authenticate **GitHub Actions to Azure using OIDC (OpenID Connect)**,
eliminating the need for long-lived secrets or service principal credentials.

The focus of this project is **cloud security and identity**, not application deployment.

---

## Problem Statement

Traditional CI/CD pipelines often authenticate to Azure using:

- Service Principals with static client secrets
- Broad permissions at subscription level
- Secrets stored in CI systems

This approach introduces:
- Secret leakage risk
- Rotation complexity
- Large blast radius
- Poor auditability

---

## Solution: OIDC with Federated Identity

This project implements **GitHub Actions OIDC authentication** using:

- Azure Entra ID App Registration
- Federated Identity Credential
- GitHub-issued OIDC tokens
- Minimal RBAC permissions

No secrets are stored in GitHub.

---

## How It Works

```text
GitHub Actions
   │
   │  (OIDC token)
   ▼
Azure Entra ID
   │
   │  (federated trust)
   ▼
Azure Subscription (RBAC)
```
---

## Authentication flow:

GitHub issues a short-lived OIDC token

Azure validates the token against the federated credential

Azure issues an access token scoped by RBAC

The workflow accesses Azure without secrets

---
### GitHub Actions Workflow

The repository contains a minimal workflow:

```bash
.github/workflows/oidc.yml
```
---
## Key characteristics:

Uses azure/login@v2

Requires id-token: write

Authenticates without secrets

Verifies access with az account show
---

## Key characteristics:

Uses azure/login@v2

Requires id-token: write

Authenticates without secrets

Verifies access with az account show

---

## Security Benefits

No client secrets

No secret rotation required

Short-lived tokens only

Repository- and branch-scoped trust

Minimal RBAC (Reader role)

This pattern aligns with enterprise cloud security best practices.

---

## Scope & Limitations

This repository intentionally:

Does not deploy resources

Does not include infrastructure code

Focuses exclusively on identity and authentication

It is designed as a security building block for other CI/CD pipelines.
---

## When to Use This Pattern

Use OIDC authentication when:

Building enterprise CI/CD pipelines

Reducing credential management overhead

Enforcing least privilege access

Improving auditability and security posture