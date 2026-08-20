# rh1-ocp5-opp

This hands-on OpenShift Platform Plus (OPP) lab environment offers a complete enterprise architecture for building, securing, and operating multi-cluster Kubernetes fleets at scale. Built on a hub-and-spoke model using Red Hat OpenShift Container Platform, the lab integrates the full OPP suite—utilizing Red Hat Advanced Cluster Management (RHACM) for global fleet governance and GitOps automation, Red Hat Advanced Cluster Security (RHACS) for continuous DevSecOps compliance and runtime threat detection, Red Hat Quay for enterprise-grade container registry management, and Red Hat OpenShift Data Foundation (ODF) for unified software-defined storage. Through practical, real-world scenarios, platform engineers, DevSecOps specialists, and SREs gain direct experience enforcing zero-trust security policies, automating multi-cluster observability, and managing resilient stateful workloads across hybrid cloud environments.

**Owner:** jalvarez-rh
**Migrated from:** https://github.com/rhpds/sovereign-cloud-showroom

---

## What was set up

1. Repository created (migrated from existing Showroom repo)
2. `catalog-info.yaml` added to repository
3. Registered in Developer Hub catalog
4. Orchestrator workflow started — your AI-guided content pipeline is running!

## What happens next

Claude will walk you through the entire content lifecycle — from intake and spec creation, through Jira tracking and reviews, all the way to a published lab on RHDP. Just follow the prompts!

## Getting started

### DevSpaces (recommended)

1. Open in DevSpaces: `https://devspaces.apps.ocpv-infra02.wdc07.infra.demo.redhat.com#https://github.com/rhpds/rh1-ocp5-opp`
2. Use Claude via the **extension** or the **CLI**:
   - **Extension:** Click the **Claude** icon in the sidebar, click **New Session**. If the Claude icon is not visible, open **Extensions** (`Ctrl/Cmd+Shift+X`), find **Claude Code for VS Code** under the DevSpaces section, click it, then click **Enable (Workspace)**.
   - **CLI:** Open a terminal and run `claude`
3. Run `/rhdp-publishing-house` — and you're off!

### Local machine

1. Install the skills:
   ```
   git clone -b prod https://github.com/rhpds/rhdp-publishing-house-skills.git ~/.claude/skills/publishing-house
   ```
2. Clone the repo:
   ```
   git clone https://github.com/rhpds/rh1-ocp5-opp
   ```
3. `cd rh1-ocp5-opp`
4. Start Claude CLI: `claude`
5. Run `/rhdp-publishing-house` — and you're off!
