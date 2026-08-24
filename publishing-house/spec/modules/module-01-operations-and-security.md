# Module Outline: module-01 — Operations and Security

## Brief Overview

This module establishes the multi-cluster sovereign cloud foundation. Students use Red Hat Advanced Cluster Management (RHACM) to create a new Hosted Control Plane (HCP) cluster scoped to an EMEA region, deploy RHEL virtual machines across clusters via the Fleet Virtualization console, and orchestrate the skupper-patient-demo application across three clusters using Red Hat OpenShift GitOps (Argo CD). The module concludes by introducing policy-as-code as the primary sovereignty mechanism: students apply GDPR-aligned RHACM policies to enforce namespace labeling and data residency on the EMEA cluster.

## Audience and Time

- **Target personas:** Platform engineers, SREs managing multi-cluster fleets
- **Prerequisites for this module:** Console access credentials delivered at lab start; background setup script (setup.sh) must have completed; familiarity with RHACM hub-and-spoke concepts
- **Estimated duration:** 50 minutes

## Learning Objectives

- Deploy a Hosted Control Plane (HCP) cluster in an EMEA region from the RHACM console and confirm it appears as a managed cluster in the fleet inventory
- Deploy a RHEL 9 virtual machine on the HCP/EMEA cluster using the RHACM Fleet Virtualization console
- Deploy the skupper-patient-demo application across three clusters (local-cluster, aws-us, HCP/EMEA) using an Argo CD ApplicationSet
- Configure GDPR-aligned RHACM Policies, Placements, and PlacementBindings to enforce namespace labeling and data residency on the EMEA cluster

## Lab Structure

| Section | Title | Duration |
|---------|-------|----------|
| 1 | RHACM Management Overview | 5 min |
| 2 | Create HCP/EMEA Cluster | 15 min |
| 3 | Deploy RHEL VMs via RHACM Fleet Virtualization | 10 min |
| 4 | Deploy Workloads via OpenShift GitOps | 10 min |
| 5 | Apply GDPR Policies to EMEA Cluster | 10 min |

## Detailed Steps

1. Open the RHACM console on the local-cluster hub and navigate to the Infrastructure > Clusters view. Confirm the two pre-provisioned clusters (local-cluster and aws-us) are visible and in Ready state.
2. Navigate to Infrastructure > Clusters > Create cluster and select Hosted (HyperShift). Fill in the cluster name (e.g., `emea-hcp`), region label (`emea`), and node pool configuration. Submit the form to initiate cluster creation.
3. Monitor cluster provisioning status in the RHACM console until the HCP/EMEA cluster transitions to Ready. Note the EMEA region label applied at creation — this label is referenced by GDPR placement rules later in the module.
4. Navigate to the Fleet Virtualization view in RHACM. Select the HCP/EMEA cluster as the target. Import the provided YAML manifest to create a RHEL 9 VM (`rhel9-lab-vm`). Verify the VM reaches Running state.
5. Navigate to Applications > GitOps in RHACM (or open the Argo CD console). Import the provided ApplicationSet YAML manifest that targets all three clusters (local-cluster, aws-us, emea-hcp) using a label selector.
6. Confirm that Argo CD creates three Application resources (one per cluster) and that all three sync successfully, deploying the skupper-patient-demo workload.
7. Verify inter-cluster connectivity by checking the Skupper network topology from the RHACM Observability dashboard or the terminal.
8. Navigate to Governance > Policies in RHACM. Import the GDPR namespace compliance policy YAML (enforces a `data-residency=emea` label on target namespaces). Set the remediation action to `enforce`.
9. Import the Placement YAML targeting clusters with the `emea` region label.
10. Import the PlacementBinding YAML linking the GDPR policy to the EMEA Placement.
11. Verify policy status in the RHACM Governance dashboard — the EMEA cluster should show Compliant and the other clusters should show Not Selected.
12. Observe the RHACM console as the policy automatically remediates any non-compliant namespace labels on the EMEA cluster.

## Key Takeaways

- RHACM Hosted Control Planes (HyperShift) allow rapid cluster provisioning without dedicated physical control plane nodes, enabling cost-effective regional expansion
- Region labels applied at cluster creation time become the anchor for geographic data residency policy targeting via Placements
- OpenShift GitOps (Argo CD) ApplicationSets enable consistent workload deployment across heterogeneous multi-cluster fleets from a single manifest
- RHACM policy-as-code (Policy + Placement + PlacementBinding) is the primary mechanism for enforcing GDPR data residency constraints at the cluster level

## Infrastructure Notes

- Students create the HCP/EMEA cluster during the module — provisioning time is approximately 5-10 minutes and should be initiated early; no pre-provisioned EMEA cluster is required
- Pre-deployed VMs: `rhel-webserver` exists on the hub cluster at lab start; `rhel9-lab-vm` is student-created during this module
- This module is entirely GUI-driven — no terminal CLI commands are required
- Skupper networking must be pre-bootstrapped by the setup script so that inter-cluster routes are available when the ApplicationSet deploys
