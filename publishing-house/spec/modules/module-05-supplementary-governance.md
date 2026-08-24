# Module Outline: module-05 — Supplementary Governance (Optional)

## Brief Overview

This optional module extends the RHACM policy-as-code governance established in Module 1 into deeper region-specific coverage and organizational structure. Students deploy a NetworkPolicy-backed RHACM policy that blocks external egress from GDPR namespaces on the EMEA cluster, apply NIST SP 800-53 namespace compliance and API audit logging policies to the US (aws-us) cluster, and then organize all geographic policies into PolicySets — RHACM's grouping construct for governance dashboards. The module is self-paced and supplements the core lab without being a dependency for other modules.

## Audience and Time

- **Target personas:** Platform engineers and compliance engineers interested in deeper RHACM governance patterns; students who complete the core modules ahead of schedule
- **Prerequisites for this module:** Module 1 complete; RHACM Governance dashboard accessible; HCP/EMEA cluster and aws-us cluster are registered managed clusters with the policies from Module 1 already applied
- **Estimated duration:** 25 minutes (optional)

## Learning Objectives

- Deploy a RHACM NetworkPolicy governance policy that blocks external egress from GDPR-labeled namespaces on the EMEA cluster
- Configure NIST SP 800-53 namespace compliance and API audit logging policies targeting the US (aws-us) cluster
- Organize regional policies into RHACM PolicySets to create a structured governance dashboard view by geography

## Lab Structure

| Section | Title | Duration |
|---------|-------|----------|
| 1 | Deploy GDPR NetworkPolicy Egress Block (EMEA) | 8 min |
| 2 | Deploy NIST SP 800-53 Policies (US cluster) | 10 min |
| 3 | Organize Policies into PolicySets by Geography | 7 min |

## Detailed Steps

1. Navigate to RHACM Governance > Policies. Review the existing GDPR namespace labeling policies applied in Module 1 on the EMEA cluster.
2. Import the GDPR NetworkPolicy governance policy YAML. This policy creates a Kubernetes NetworkPolicy in the target namespace that blocks all external egress (denies traffic to CIDRs outside the cluster network), ensuring GDPR data cannot be exfiltrated via direct pod-to-external connections. Set remediation to `enforce`.
3. Import the Placement YAML targeting clusters with the `region=emea` label (same Placement used in Module 1 is reusable; confirm it already exists or create it).
4. Import the PlacementBinding YAML linking the NetworkPolicy governance policy to the EMEA Placement.
5. Verify in the RHACM Governance dashboard that the NetworkPolicy policy shows as Compliant on the EMEA cluster. Navigate to the EMEA cluster's OpenShift console and confirm the NetworkPolicy resource was created in the target namespace.
6. Return to RHACM Governance > Policies. Import the NIST SP 800-53 namespace compliance policy YAML — this enforces a `compliance=nist-800-53` label on target namespaces on the aws-us cluster.
7. Import the API audit logging policy YAML — this configures OpenShift API server audit log levels on the aws-us cluster to the NIST-required profile.
8. Import the Placement YAML targeting clusters with the `region=us` label.
9. Import the PlacementBinding YAMLs linking both NIST policies to the US Placement.
10. Verify both NIST policies show as Compliant in the Governance dashboard for the aws-us cluster.
11. Navigate to RHACM Governance > Policy Sets. Click Create Policy Set.
12. Create an EMEA PolicySet: name it `emea-governance`, add the GDPR namespace labeling policy and the GDPR NetworkPolicy egress policy as members.
13. Create a US PolicySet: name it `us-governance`, add the NIST SP 800-53 namespace compliance policy and the API audit logging policy as members.
14. Return to the Governance dashboard and observe the PolicySet groupings. Confirm that the dashboard now presents a clean geographic view: `emea-governance` with EMEA compliance status, `us-governance` with US compliance status.
15. Review the aggregate compliance percentage for each PolicySet — this is the governance dashboard view that a sovereignty officer or compliance auditor would use for day-to-day monitoring.

## Key Takeaways

- NetworkPolicy-backed RHACM governance policies extend data residency enforcement beyond namespace labels into actual network-layer egress controls — creating defense-in-depth for GDPR compliance
- Region-specific policy stacks (GDPR for EMEA, NIST SP 800-53 for US) reflect the real-world requirement that sovereign cloud operators must manage different regulatory frameworks simultaneously across geographic clusters
- RHACM PolicySets provide a governance layer above individual policies, allowing compliance teams to assess regional regulatory posture with a single aggregate status rather than reviewing each policy individually
- All governance in this module is declarative and version-controllable — PolicySets, Policies, Placements, and PlacementBindings are Kubernetes CRs that can be stored in Git and deployed via GitOps

## Infrastructure Notes

- This module is entirely GUI-driven via the RHACM console — no terminal CLI commands are required
- All YAML manifests should be provided as lab assets (copy-paste blocks in the Showroom content); students do not author them from scratch
- The NetworkPolicy egress block policy requires that the Kubernetes NetworkPolicy enforcement is active on the EMEA cluster — confirm the CNI plugin (e.g., OVN-Kubernetes) supports NetworkPolicy enforcement before authoring the lab content
- PolicySets are a relatively recent RHACM feature — confirm the RHACM version deployed in the lab environment supports PolicySets before finalizing this module's content
