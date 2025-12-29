# End-to-End DevSecOps Project

This repository documents a complete, production-inspired **DevSecOps implementation** built on AWS and Kubernetes.

The project demonstrates how modern teams securely build, scan, deploy, and operate containerized microservices using **Infrastructure as Code, CI/CD, GitOps, policy enforcement, autoscaling, and chaos engineering**.

The system is intentionally designed with:
- Clear separation of concerns
- Security-first pipelines
- Declarative GitOps workflows
- Zero manual Kubernetes changes
- Cost-aware infrastructure decisions

---

## High-Level Architecture Overview

This DevSecOps system is split across **three focused repositories**, each owning a single responsibility 

The overall flow is:

1. Infrastructure is provisioned using Terraform
2. Application code is scanned, built, and pushed via CI/CD
3. Kubernetes desired state is managed using GitOps
4. ArgoCD continuously syncs and enforces cluster state
5. Security, scaling, remediation, and resilience are handled declaratively

---

## Repository Responsibilities

### 1. DevSecOps-Infra (Infrastructure)

**Purpose:**  
Provision AWS infrastructure using Terraform.

**Responsibilities**
- Custom VPC and networking
- IAM roles and policies
- Amazon EKS cluster
- Managed EKS node groups
- Terraform remote state (S3 + DynamoDB)

**Key Principle**  
Infrastructure is provisioned once and then consumed.  
No manual changes are made after creation.

---

### 2. DevSecOps-microservices (Applications + CI/CD)

**Purpose**  
Own application source code and all CI/CD pipelines.

**Applications**
- Go service
- Node.js service
- Python service

**CI/CD Capabilities**
- Secrets scanning (Gitleaks)
- SAST (Bandit)
- Dependency scanning (Snyk)
- Container and IaC scanning (Trivy)
- SBOM generation (Syft)
- Enforced security gates
- Docker image build
- Push to Amazon ECR using GitHub OIDC

**Key Principle**  
Only **secure, scanned, policy-compliant images** are allowed to move forward.

---

### 3. devsecops-gitops (GitOps – Kubernetes Desired State)

**Purpose**  
Define the complete desired state of Kubernetes using GitOps.

**Managed via this repository**
- ArgoCD Applications (app-of-apps)
- Kustomize base manifests
- Environment overlays (dev / prod)
- Ingress configuration
- Argo Rollouts (canary deployments)
- Horizontal Pod Autoscaler (HPA)
- Kyverno admission control policies
- Auto-remediation using CronJobs
- Chaos engineering experiments
- Namespace management
- Policy and load testing manifests

**Key Principle**  
Git is the single source of truth.  
ArgoCD continuously reconciles and self-heals drift.

---

## End-to-End Workflow

1. Developer pushes code to the microservices repository
2. Security pipeline runs (failure stops the pipeline)
3. Images are built and pushed to Amazon ECR
4. Image tags are referenced in the GitOps repository
5. ArgoCD detects changes and syncs the cluster
6. Kyverno enforces policies at admission time
7. Argo Rollouts and HPA manage delivery and scaling
8. Auto-remediation handles failures
9. Chaos experiments validate resilience

---

## Security and Governance Model

- Shift-left security enforced in CI
- Immutable container images
- Admission-time policy enforcement
- No privileged containers
- Resource limits required
- Only trusted image registries allowed
- No manual kubectl usage in production paths

---

## Cost and Design Considerations

This project is intentionally cost-optimized:

- Small EC2 instance types
- No NAT Gateway
- Single EKS cluster
- Minimal managed services
- Resources created, tested, and destroyed as needed

The goal is realism without unnecessary cloud spend.

---

## Why This Project Matters

This is not a demo or tutorial setup.

It demonstrates:
- Real-world DevSecOps architecture
- Strong separation of responsibilities
- Policy-driven Kubernetes security
- Progressive delivery strategies
- GitOps-based operations
- Resilience and failure testing
- Zero-drift infrastructure philosophy

The design mirrors how mature DevSecOps and Platform teams operate in production environments.

---

## Video Tutorial for Project Walkthrough

LINK
