# LB1161 - Sovereign Cloud Architecture with OpenShift, Multi-Cluster Data Residency and Compliance Management

## Overview

This lab delivers hands-on experience designing and operating a sovereign cloud architecture using the Red Hat OpenShift Platform Plus suite across multiple clusters. It addresses the core pillars of digital sovereignty: data residency enforcement, continuous compliance assurance, supply chain integrity, and on-premises AI. Participants will create a Hosted Control Plane (HCP) cluster scoped to an EMEA region, enforce GDPR and NIST policy-as-code across geographically separated clusters using RHACM, run multi-profile compliance scans and observe real-time runtime security enforcement with RHACS, build and cryptographically sign container images using Red Hat Trusted Artifact Signer, and deploy a CPU-based AI workbench with Red Hat OpenShift AI — all without relying on external cloud provider services.

## Target Audience

- **Role:** Platform engineers, DevSecOps practitioners, Site Reliability Engineers (SREs)
- **Experience level:** Advanced
- **What they already know:** Solid OpenShift administration experience; familiarity with multi-cluster Kubernetes concepts and GitOps workflows
- **What they don't know:** Multi-cluster fleet governance and data residency policy-as-code with RHACM; runtime threat detection and automated enforcement with RHACS; container image signing with Sigstore/RHTAS; sovereign on-premises AI deployment with RHOAI

## Prerequisites

- Solid Red Hat OpenShift administration experience (cluster deployment, RBAC, workload management)
- Familiarity with multi-cluster Kubernetes concepts (hub-and-spoke, managed clusters)
- No — prerequisites cannot be validated automatically; learners self-attest at lab start

## Learning Objectives

1. Deploy a Hosted Control Plane (HCP) cluster in an EMEA region using Red Hat Advanced Cluster Management and verify it is registered in the fleet inventory
2. Configure multi-cluster data residency and GDPR policy-as-code using RHACM Policies, Placements, and PlacementBindings targeting the EMEA cluster
3. Deploy containerized workloads across three clusters using Red Hat OpenShift GitOps (Argo CD) ApplicationSets
4. Configure multi-profile compliance scans (CIS, Essential 8, NERC-CIP, PCI-DSS, STIG) across clusters using RHACS and the Compliance Operator
5. Observe and verify real-time runtime security enforcement by triggering a RHACS policy violation and confirming pod termination
6. Build a container image from a Red Hat UBI golden base image and push it to a private Red Hat Quay registry
7. Sign container images using cosign backed by Red Hat Trusted Artifact Signer (Fulcio CA, Rekor transparency log, TUF) with OIDC identity verification
8. Deploy a Jupyter workbench on Red Hat OpenShift AI and execute a data science notebook on CPU-only infrastructure as a sovereign alternative to cloud AI services

## Content Type

Lab (hands-on)

## Products & Technologies

Red Hat products:
- Red Hat OpenShift Container Platform 4.21
- Red Hat Advanced Cluster Management for Kubernetes (RHACM)
- Red Hat Advanced Cluster Security for Kubernetes (RHACS)
- Red Hat OpenShift Virtualization
- Red Hat OpenShift GitOps (Argo CD)
- Red Hat Quay
- Red Hat OpenShift AI (RHOAI)
- Red Hat Trusted Artifact Signer (RHTAS)
- Red Hat Universal Base Image (UBI)
- Compliance Operator
- Red Hat build of Keycloak

Open source and community:
- Hosted Control Plane (HyperShift)
- cosign (Sigstore — Fulcio, Rekor, TUF)
- podman
- Skupper
- Argo CD
- JupyterLab

## Module Map

| Module | Title | Duration |
|--------|-------|----------|
| Intro | Introduction and Setup | 10 min |
| 1 | Operations and Security | 35 min |
| 2 | Assurance | 30 min |
| 3 | Technology and Supply Chain | 30 min |
| 4 | Data and AI | 10 min |
| — | Conclusion | 5 min |
| — | **Total lab** | **~2 h** |
| 5 | Supplementary Governance (Optional) | +25 min |

## Difficulty Level

Advanced

## Environment

**Learner view:** The lab starts with two pre-provisioned OpenShift clusters: a hub cluster (local-cluster) running RHACM, RHACS, Red Hat Quay, Red Hat OpenShift AI, and Red Hat Trusted Artifact Signer, and a managed cluster (aws-us) registered in the RHACM fleet. A background setup script (setup.sh) runs at lab start to prepare application manifests and credentials. Participants receive console access credentials for OpenShift, RHACS, Quay, RHOAI, and a terminal. During Module 1, participants create a third cluster (HCP/EMEA) via the RHACM console. One pre-deployed RHEL VM (rhel-webserver) exists on the hub cluster; participants create an additional RHEL VM (rhel9-lab-vm) during the lab.

**Automation needed:** Yes — a setup script provisions initial lab state (application repos, credential injection, RHTAS stack initialization). Background automation pre-deploys operators and platform services (RHACM, RHACS, Quay, RHOAI, RHTAS, OpenShift Virtualization) on the hub cluster before the lab session begins.

## Infrastructure Requirements

- **Cloud provider:** CNV
- **Cluster type:** Multinode (OpenShift Virtualization, RHACM, RHACS, Quay, RHOAI, and RHTAS all run on the hub simultaneously)
- **OCP version:** 4.21
- **Topology:** Per-student
- **Sizing:** TBD — confirmed by infra reviewer (hub must support full OPP operator stack; worker sizing and count TBD)
- **Automation approach:** Both — Ansible (setup/provisioning) and GitOps/Helm (ArgoCD workload deployment)
- **AI/MaaS:** None (RHOAI present but CPU-only; no model serving, no GPU, no MaaS)
- **External services:** github.com (setup scripts, demo app manifests, AI notebook), registry.access.redhat.com (UBI base image pull in Module 3)
- **AAP version:** N/A (AAP not in products)
- **Non-GA products:** None (all products are GA)
