# LB1066: OpenShift 5 for Virtualization, AI, and Sovereignty

## Overview

This hands-on lab is the OpenShift 5 path for placing virtual machines, containers, and applications on the right clusters, running an AI workload on the platform, and securing that platform with RHACM and RHACS. Learners create an EMEA Hosted Control Plane, then create a RHEL virtual machine with OpenShift Lightspeed or from a template, use Lightspeed to troubleshoot the cluster and call tools through an MCP server and MCP gateway, write policy for the application already on the two clusters, attest it with Zero Trust Workload Identity Manager, rotate its TLS certificate, use RHACS to enforce a runtime guardrail, apply regional policy, and sign the image they build. Red Hat Zero Trust Workload Identity Manager (ZTWIM) is the identity control for giving those workloads the right access to systems and data.

## Target Audience

- **Role:** Platform engineers, DevSecOps practitioners, Site Reliability Engineers (SREs)
- **Experience level:** Advanced
- **What they already know:** Solid OpenShift administration experience; familiarity with multi-cluster Kubernetes concepts and GitOps workflows
- **What they don't know:** Multi-cluster fleet governance and data residency policy-as-code with RHACM; runtime threat detection and automated enforcement with RHACS; container image signing with Sigstore/RHTAS; agentic troubleshooting, the MCP server, and the MCP gateway with OpenShift Lightspeed

## Prerequisites

- Solid Red Hat OpenShift administration experience (cluster deployment, RBAC, workload management)
- Familiarity with multi-cluster Kubernetes concepts (hub-and-spoke, managed clusters)
- No — prerequisites cannot be validated automatically; learners self-attest at lab start

## Learning Objectives

1. Deploy a Hosted Control Plane (HCP) cluster in an EMEA region using Red Hat Advanced Cluster Management and verify it is registered in the fleet inventory
2. Use OpenShift Lightspeed for agentic troubleshooting, and call cluster and security APIs through an MCP server and the MCP gateway
3. Apply RHACM and RHACS policy to the patient-portal application already running on local-cluster and aws-us, attest that application with Zero Trust Workload Identity Manager, and rotate its service TLS certificate
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
- Red Hat OpenShift Lightspeed
- Red Hat Trusted Artifact Signer (RHTAS)
- Red Hat Zero Trust Workload Identity Manager (ZTWIM)
- Red Hat Universal Base Image (UBI)
- Compliance Operator
- Red Hat build of Keycloak

Open source and community:
- Hosted Control Plane (HyperShift)
- cosign (Sigstore — Fulcio, Rekor, TUF)
- podman
- Skupper
- Argo CD

## Module Map

The same exercises are grouped into three modules. Virtualization stands up the EMEA Hosted Control Plane, creates a RHEL virtual machine with OpenShift Lightspeed or from a template, and checks that virtual machine with the Compliance Operator and RHACS. AI Adoption follows the OpenShift 5 First Look exercises: agentic troubleshooting, the MCP server, and the MCP gateway. Security and Sovereignty starts from patient-portal already running on the two clusters, then covers RHACM and RHACS policy, ZTWIM attestation, and TLS certificate rotation. An optional extra module covers Lightwell and the trusted software supply chain. Regional governance stays at the end of Module 3.

| Module | Title | Duration |
|--------|-------|----------|
| Intro | Introduction and Setup | 10 min |
| 1 | Virtualization | 25 min |
| 2 | AI Adoption | 40 min |
| 3 | Security and Sovereignty | 70 min |
| — | Conclusion | 5 min |
| — | **Total lab** | **~2.5 h** |
| 3 (optional sections) | Supplementary governance at the end of Module 3 | +25 min |
| Extra | Lightwell and trusted software supply chain | +30 min |

## Difficulty Level

Advanced

## Environment

**Learner view:** The lab starts with two pre-provisioned OpenShift clusters: a hub cluster (local-cluster) running RHACM, RHACS, Red Hat Quay, OpenShift Lightspeed, Red Hat Trusted Artifact Signer, and Zero Trust Workload Identity Manager, and a managed cluster (aws-us) registered in the RHACM fleet. The bastion terminal already has podman and cosign. A background setup script (setup.sh) runs at lab start to prepare application manifests and credentials. Participants receive console access credentials for OpenShift, RHACS, Quay, and a terminal. During Module 1 (Virtualization), participants create a third cluster (HCP/EMEA) via the RHACM console. One pre-deployed RHEL VM (rhel-webserver) exists on the hub cluster. Participants create an additional RHEL VM (rhel9-lab-vm) with OpenShift Lightspeed, or from the RHEL 9 template if Lightspeed does not leave it running, then check host compliance and guest vulnerabilities in RHACS. Module 2 (AI Adoption) follows the OpenShift 5 First Look exercises for agentic troubleshooting, the MCP server, and the MCP gateway. It does not wait on the EMEA cluster. Module 3 (Security and Sovereignty) starts from patient-portal already running on local-cluster and aws-us. Participants write RHACM and RHACS policy for that application, attest the frontend with Zero Trust Workload Identity Manager, and rotate its service TLS certificate. Regional governance and the hardened-image signing path are optional.

**Automation needed:** Yes — the `opp-config` Argo CD application installs Red Hat Trusted Artifact Signer (Fulcio, Rekor, and TUF) and Zero Trust Workload Identity Manager. Provisioning installs podman and cosign on the bastion. A setup script still prepares the demo application manifests. Background automation also pre-deploys RHACM, RHACS, Quay, and OpenShift Virtualization on the hub cluster before the lab session begins.

## Infrastructure Requirements

- **Cloud provider:** CNV
- **Cluster type:** Multinode (OpenShift Virtualization, RHACM, RHACS, Quay, RHTAS, and ZTWIM all run on the hub simultaneously)
- **OCP version:** 4.21
- **Topology:** Per-student
- **Sizing:** TBD — confirmed by infra reviewer (hub must support full OPP operator stack; worker sizing and count TBD)
- **Automation approach:** Both — Ansible (setup/provisioning) and GitOps/Helm (ArgoCD workload deployment)
- **AI/MaaS:** OpenShift Lightspeed in the console. No model serving and no GPU.
- **External services:** github.com (setup scripts and demo app manifests), registry.access.redhat.com (UBI base image pull in the extra module)
- **AAP version:** N/A (AAP not in products)
- **Non-GA products:** None (all products are GA)
