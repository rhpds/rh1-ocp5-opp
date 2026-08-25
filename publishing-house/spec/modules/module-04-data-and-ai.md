# Module Outline: module-04 — Data and AI

## Brief Overview

This module positions on-premises AI as a sovereign alternative to cloud-hosted AI services. Students explore the Red Hat OpenShift AI (RHOAI) dashboard, review the DataScienceCluster configuration, and launch a CPU-based Jupyter workbench using the Python 3.12 / 2025.2 notebook image. They then clone and execute a student performance prediction notebook, observing that a complete AI workflow — data loading, model training, inference — is achievable within a self-managed cluster without GPU accelerators or external AI API calls. The module is intentionally concise, serving as a proof-of-concept demonstration of sovereign AI capability rather than a deep data science exercise.

## Audience and Time

- **Target personas:** Platform engineers and SREs evaluating on-premises AI; secondary audience of data scientists interested in sovereign deployment
- **Prerequisites for this module:** RHOAI pre-deployed on the hub cluster with a DataScienceCluster CR in Ready state; student has RHOAI console access credentials; internet access to github.com for notebook clone (or pre-cloned by setup script)
- **Estimated duration:** 10 minutes

## Learning Objectives

- Explore the Red Hat OpenShift AI dashboard and identify pre-configured AI/ML infrastructure components (DataScienceCluster, workbench images, accelerator profiles)
- Deploy a CPU-based Jupyter workbench using the Python 3.12 / 2025.2 notebook image in RHOAI
- Execute a student performance prediction notebook to demonstrate end-to-end AI inference on CPU-only sovereign infrastructure

## Lab Structure

| Section | Title | Duration |
|---------|-------|----------|
| 1 | Exploring the RHOAI Dashboard | 5 min |
| 2 | Reviewing AI Hub and Deployments | 3 min |
| 3 | Deploying a Jupyter Workbench | 7 min |
| 4 | Running the Prediction Notebook | 5 min |

## Detailed Steps

1. Open the Red Hat OpenShift AI console using the provided credentials. Navigate to the RHOAI dashboard home page and review the available sections: Data Science Projects, Model Serving, Pipelines, and Accelerator Profiles.
2. Navigate to Settings > Cluster Settings. Observe the DataScienceCluster status and note that no GPU accelerator profiles are configured — the lab operates on CPU only. This is intentional and demonstrates that meaningful AI workloads are achievable without dedicated GPU hardware.
3. Navigate to Settings > Accelerator Profiles. Confirm that no GPU accelerators are listed, reinforcing the CPU-only sovereign deployment model.
4. Navigate to Data Science Projects and create a new project (e.g., `sovereign-ai-demo`).
5. Within the project, click Create a Workbench. Configure the following:
   - **Name:** `student-prediction`
   - **Notebook image:** Python 3.12 / 2025.2
   - **Container size:** Small (CPU-only — no accelerator)
   - **Storage:** Create a new 5Gi PVC for the workbench
6. Click Create and wait for the workbench to reach Running state (approximately 2-3 minutes).
7. Click Open to launch the JupyterLab interface in a browser tab.
8. In the JupyterLab terminal, clone the student performance prediction notebook repository:
   ```
   git clone https://github.com/<repo-path>/sovereign-ai-notebooks.git
   ```
9. Open the notebook file (`student_performance_prediction.ipynb`) from the file browser in JupyterLab.
10. Run all cells sequentially (Kernel > Run All Cells or Shift+Enter through each cell). Observe the following stages: data loading from a CSV file, feature engineering, model training (scikit-learn or similar lightweight framework), and inference output showing predicted student performance scores.
11. Review the final cell output. Note that the entire pipeline ran without any external API calls or GPU resources, demonstrating a fully sovereign AI inference workflow.

## Key Takeaways

- Red Hat OpenShift AI provides a self-managed AI/ML platform that runs entirely within an organization's own infrastructure — no data leaves the sovereign boundary during model training or inference
- CPU-only AI workloads are viable for many classification and prediction tasks; GPU acceleration is only required for large language model serving or large-scale training jobs
- The DataScienceCluster operator abstracts the underlying Kubernetes complexity, allowing data scientists to launch workbenches without cluster administration knowledge
- On-premises AI deployment eliminates dependency on hyperscaler AI APIs (OpenAI, Anthropic, Google Vertex), satisfying data residency requirements for sensitive training data in regulated industries

## Infrastructure Notes

- No GPU nodes are required for this module — the lab explicitly demonstrates CPU-only AI as a sovereign capability
- RHOAI DataScienceCluster must be pre-deployed and in Ready state before the module; student only interacts with the RHOAI console, not the operator configuration
- The student performance prediction notebook should be pre-cloned in the workbench home directory by the setup script as a fallback in case github.com is unreachable from the lab environment
- Workbench startup time (~2-3 min) is the primary wait point — instructors should flag this so students know to expect a brief pause
- Storage class must support ReadWriteOnce PVCs for workbench persistent volumes; ODF is the expected backing storage on the hub cluster
