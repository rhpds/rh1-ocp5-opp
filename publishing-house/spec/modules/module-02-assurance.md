# Module Outline: module-02 — Assurance

## Brief Overview

This module focuses on continuous security assurance using Red Hat Advanced Cluster Security for Kubernetes (RHACS) and the Compliance Operator. Students configure cluster-level settings to resolve a failing compliance check, then schedule and review multi-profile compliance scans across all clusters. The centerpiece is a live runtime enforcement demonstration: students use tmux to observe RHACS automatically terminate a pod that invokes a prohibited package manager, then review the resulting violations log as an audit artifact. This module demonstrates how policy-as-code operates at both configuration and runtime layers within a sovereign cloud architecture.

## Audience and Time

- **Target personas:** DevSecOps practitioners, security engineers, platform SREs
- **Prerequisites for this module:** Module 1 complete; HCP/EMEA cluster is registered in RHACM; RHACS console access credentials available
- **Estimated duration:** 45 minutes

## Learning Objectives

- Configure an allowed container registry policy on the local-cluster to resolve a failing Essential 8 (E8) compliance check in RHACS
- Schedule multi-cluster compliance scans across CIS, Essential 8, NERC-CIP, PCI-DSS, and STIG profiles using RHACS and the Compliance Operator
- Implement a runtime security enforcement demonstration using tmux to observe RHACS kill a pod invoking the Alpine package manager
- Analyze compliance scan results and violations reports in the RHACS console as audit evidence for sovereign cloud governance

## Lab Structure

| Section | Title | Duration |
|---------|-------|----------|
| 1 | Security and Environment Insights (RHACS Basics) | 5 min |
| 2 | Configure Cluster Settings to Pass Compliance Check | 10 min |
| 3 | Schedule Multi-Cluster Compliance Scans | 10 min |
| 4 | Runtime Enforcement Demonstration | 12 min |
| 5 | Violations and Reporting for Audit | 8 min |

## Detailed Steps

1. Open the RHACS console and navigate to Compliance. Review the current compliance posture across the registered clusters. Identify the failing E8 check related to allowed container registries.
2. Switch to the OpenShift console for local-cluster. Navigate to Administration > Cluster Settings. Open the terminal and run:
   ```
   oc config use-context local-cluster
   ```
3. Apply the allowed registry configuration that restricts image pulls to approved registries (internal Quay, registry.access.redhat.com, registry.redhat.io). Use the provided YAML manifest:
   ```
   oc apply -f allowed-registries-config.yaml
   ```
4. Return to RHACS Compliance and re-run the E8 scan for local-cluster. Confirm the previously failing check now shows as Passing.
5. Navigate to RHACS > Compliance > Scan Configurations. Create a new scan schedule targeting all clusters. Add the following compliance profiles: CIS OpenShift, Essential 8 (E8), NERC-CIP, PCI-DSS, STIG.
6. Trigger an immediate scan run and wait for results to populate across all registered clusters (local-cluster, aws-us, emea-hcp).
7. Review the per-cluster compliance summary in RHACS, noting which profiles each cluster passes or fails and comparing results across geographies.
8. Open a terminal session and start tmux with a split pane:
   ```
   tmux new-session \; split-window -h
   ```
9. In the left pane, watch for Kubernetes events on local-cluster:
   ```
   oc get events -w -n <target-namespace>
   ```
10. In the right pane, exec into a test pod running an Alpine-based image:
    ```
    oc exec -it <pod-name> -- /bin/sh
    apk update
    ```
11. Observe in the left pane that RHACS detects the `apk` invocation (prohibited package manager execution) and terminates the pod. Confirm the pod restarts and the violation is logged.
12. Navigate to RHACS > Violations. Filter by Policy: "Alpine Linux Package Manager Execution." Review the violation details: timestamp, cluster, namespace, pod name, enforced action (Kill Pod).
13. Navigate to RHACS > Compliance > Reports. Generate a compliance report for one profile (e.g., PCI-DSS) covering all clusters. Download or preview the report as an audit artifact.

## Key Takeaways

- Cluster-level allowed registry configuration is a prerequisite for passing E8 and CIS compliance checks — demonstrating that cluster hardening and compliance are interdependent
- Multi-profile compliance scans (CIS, E8, NERC-CIP, PCI-DSS, STIG) give sovereign cloud operators a single pane of glass for regulatory posture across geographically dispersed clusters
- RHACS runtime enforcement operates at the kernel level and can terminate non-compliant process executions automatically — not just alert on them
- Violations reports function as audit evidence in regulated environments and can be exported for compliance auditors without requiring direct cluster access

## Infrastructure Notes

- tmux must be available in the lab terminal environment — verify this is pre-installed in the student terminal container
- The runtime enforcement demonstration requires a pre-deployed test pod running an Alpine-based image in a monitored namespace; the setup script should provision this pod
- RHACS must be pre-configured with runtime enforcement enabled (Kill Pod) for the Alpine package manager policy; default RHACS installs may have this policy in Inform-only mode — it must be switched to Enforce before the module
- The Compliance Operator must be pre-installed on all managed clusters before Module 2 begins
