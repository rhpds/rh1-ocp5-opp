# Module Outline: module-03 — Technology and Supply Chain

## Brief Overview

This module addresses software supply chain integrity — a core pillar of sovereign cloud architecture. Students pull a Red Hat Universal Base Image (UBI) Python image from the Red Hat registry into their private Quay registry as a trusted golden base image, then rebuild the patient-portal frontend container from that UBI base. Finally, they sign the resulting image using cosign backed by the in-cluster Red Hat Trusted Artifact Signer (RHTAS) stack — Fulcio CA for certificate issuance, Rekor for transparency log immutability, and TUF for trust root distribution. OIDC identity via Red Hat build of Keycloak ties a verified user identity to each signature stored in Quay, creating an auditable chain of custody for every image.

## Audience and Time

- **Target personas:** DevSecOps practitioners, platform engineers, software supply chain security engineers
- **Prerequisites for this module:** Quay console access; terminal access with podman and cosign installed; RHTAS stack pre-deployed on hub cluster (Fulcio, Rekor, TUF, SecureSign operator); Keycloak OIDC client pre-configured
- **Estimated duration:** 30 minutes

## Learning Objectives

- Pull a Red Hat UBI Python base image from the Red Hat registry and push it to a private Red Hat Quay registry as a golden base image
- Rebuild the patient-portal frontend container image using the UBI golden base image via podman
- Initialize cosign trust roots from the in-cluster TUF mirror and authenticate using OIDC via Red Hat build of Keycloak
- Sign a container image using cosign backed by the in-cluster RHTAS stack (Fulcio CA + Rekor transparency log) and verify the signature is stored in Quay

## Lab Structure

| Section | Title | Duration |
|---------|-------|----------|
| 1 | Installation and Setup | 5 min |
| 2 | Getting Your Golden Image | 10 min |
| 3 | Rebuilding the Patient Portal Frontend | 15 min |
| 4 | Signing Your Software | 10 min |

## Detailed Steps

1. Verify cosign is installed in the terminal:
   ```
   cosign version
   ```
2. Source environment variables (registry endpoints, OIDC URLs, cluster routes):
   ```
   source ~/.bashrc
   ```
3. Run the RHTAS setup script to configure the in-cluster signing stack:
   ```
   cd ~/svc-lab/tssc-setup && ./setup.sh
   ```
4. Retrieve RHTAS service routes from the cluster to populate environment variables:
   ```
   oc get tuf -o jsonpath='{.items[0].status.url}'
   oc get fulcio -o jsonpath='{.items[0].status.url}'
   oc get rekor -o jsonpath='{.items[0].status.url}'
   oc get securesign -o jsonpath='{.items[0].status.url}'
   ```
   Export these values as environment variables: `TUF_URL`, `FULCIO_URL`, `REKOR_URL`.
5. Log into the private Quay registry using podman:
   ```
   podman login <quay-registry-route> -u <username> -p <password>
   ```
6. Pull the Red Hat UBI Python 3.12 base image from the Red Hat registry:
   ```
   podman pull registry.access.redhat.com/ubi8/python-312
   ```
7. Tag the image for the private Quay registry:
   ```
   podman tag registry.access.redhat.com/ubi8/python-312 <quay-route>/patient-portal/python-312:golden
   ```
8. Push the golden base image to Quay:
   ```
   podman push <quay-route>/patient-portal/python-312:golden
   ```
9. Navigate to the patient-portal Dockerfile. Update the FROM line to reference the golden base image in the private Quay registry:
   ```
   sed -i 's|FROM.*|FROM <quay-route>/patient-portal/python-312:golden|' Dockerfile
   ```
10. Build the updated patient-portal frontend image:
    ```
    podman build -t <quay-route>/patient-portal/frontend:signed .
    ```
11. Push the rebuilt frontend image to Quay:
    ```
    podman push <quay-route>/patient-portal/frontend:signed
    ```
12. Initialize cosign trust roots from the in-cluster TUF mirror:
    ```
    cosign initialize --mirror=$TUF_URL --root=$TUF_URL/root.json
    ```
13. Log in to cosign using OIDC via Keycloak:
    ```
    cosign login <quay-route>
    ```
14. Sign the frontend image. cosign will open a browser for Keycloak OIDC authentication, then obtain a short-lived Fulcio certificate tied to the authenticated identity:
    ```
    cosign sign --fulcio-url=$FULCIO_URL --rekor-url=$REKOR_URL <quay-route>/patient-portal/frontend:signed
    ```
15. Verify the signature was recorded in the Rekor transparency log:
    ```
    cosign tree <quay-route>/patient-portal/frontend:signed
    ```
16. Open the Quay console and navigate to the patient-portal repository. Confirm that the signed image has a cosign signature layer attached (displayed as a tag or manifest attachment in the repository view).

## Key Takeaways

- Pulling from Red Hat's official registry and retagging into a private Quay registry establishes a golden image policy — all application images derive from an audited, trusted base
- Rebuilding application images from the golden base rather than community images ensures UBI's reduced attack surface and Red Hat's security backport guarantees apply to the entire supply chain
- RHTAS (Fulcio + Rekor + TUF) provides a complete Sigstore-compatible signing infrastructure fully under the sovereign operator's control — no dependency on the public Sigstore infrastructure at sigstore.dev
- OIDC-backed signing ties a verified enterprise identity (Keycloak user) to every image signature, creating an auditable chain of custody that satisfies supply chain security mandates (SLSA, EO 14028)

## Infrastructure Notes

- cosign, podman, and tmux must be pre-installed in the student terminal container
- RHTAS stack (SecureSign operator, Fulcio, Rekor, TUF) must be pre-deployed and in Ready state before this module — provisioning RHTAS is not part of this module
- Keycloak OIDC client for cosign must be pre-configured with the student's lab credentials as an authorized user
- The `svc-lab/tssc-setup` repository must be pre-cloned in the student home directory by the setup script
- Environment variables (`QUAY_ROUTE`, `TUF_URL`, `FULCIO_URL`, `REKOR_URL`) should be pre-exported in `~/.bashrc` by the setup script to reduce friction; the `source ~/.bashrc` step at the start of the module activates them
