# LB1066: OpenShift 5 for Virtualization, AI, and Sovereignty

## Overview

This hands-on lab is the OpenShift 5 path for placing virtual machines, containers, and applications on the right clusters, running an AI workload on the platform, and securing that platform with RHACM and RHACS. Learners create an EMEA Hosted Control Plane and a RHEL virtual machine themselves (OpenShift Lightspeed is the hands-off alternative), run a notebook on OpenShift AI, place an application with OpenShift GitOps, use RHACS to scan and enforce runtime guardrails, apply regional policy, and sign the image they build. Red Hat Zero Trust Workload Identity Manager (ZTWIM) is the identity control for giving those workloads the right access to systems and data.

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
2. Deploy a Jupyter workbench on Red Hat OpenShift AI and execute a data science notebook on CPU-only infrastructure as a sovereign alternative to cloud AI services
3. Deploy containerized workloads across three clusters using Red Hat OpenShift GitOps (Argo CD) ApplicationSets
4. Configure multi-cluster data residency and GDPR policy-as-code using RHACM Policies, Placements, and PlacementBindings targeting the EMEA cluster
5. Configure multi-profile compliance scans (CIS, Essential 8, NERC-CIP, PCI-DSS, STIG) across clusters using RHACS and the Compliance Operator
6. Observe and verify real-time runtime security enforcement by triggering a RHACS policy violation and confirming pod termination
7. Build a container image from a Red Hat UBI golden base image and push it to a private Red Hat Quay registry
8. Sign container images using cosign backed by Red Hat Trusted Artifact Signer (Fulcio CA, Rekor transparency log, TUF) with OIDC identity verification

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

The same exercises are grouped into three modules. Virtualization stands up the EMEA Hosted Control Plane and a RHEL virtual machine. AI Adoption is the OpenShift AI workbench. Security and Sovereignty holds GitOps placement, RHACM policy, RHACS assurance, image signing, and the optional regional governance exercises.

| Module | Title | Duration |
|--------|-------|----------|
| Intro | Introduction and Setup | 10 min |
| 1 | Virtualization | 25 min |
| 2 | AI Adoption | 10 min |
| 3 | Security and Sovereignty | 70 min |
| — | Conclusion | 5 min |
| — | **Total lab** | **~2 h** |
| 3 (optional sections) | Supplementary governance at the end of Module 3 | +25 min |

## Difficulty Level

Advanced

## Environment

**Learner view:** The lab starts with two pre-provisioned OpenShift clusters: a hub cluster (local-cluster) running RHACM, RHACS, Red Hat Quay, Red Hat OpenShift AI, and Red Hat Trusted Artifact Signer, and a managed cluster (aws-us) registered in the RHACM fleet. A background setup script (setup.sh) runs at lab start to prepare application manifests and credentials. Participants receive console access credentials for OpenShift, RHACS, Quay, RHOAI, and a terminal. During Module 1 (Virtualization), participants create a third cluster (HCP/EMEA) via the RHACM console. One pre-deployed RHEL VM (rhel-webserver) exists on the hub cluster; participants create an additional RHEL VM (rhel9-lab-vm) during the lab. Module 2 (AI Adoption) uses the pre-deployed OpenShift AI dashboard and does not wait on the EMEA cluster. Module 3 (Security and Sovereignty) deploys the patient-portal application, applies RHACM policy and RHACS controls, and signs an image. Optional governance sections are at the end of Module 3.

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
