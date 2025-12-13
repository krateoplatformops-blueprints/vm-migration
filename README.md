# VM Migration from VMware to KubeVirt with Forklift

## Overview

This Krateo blueprint orchestrates the migration of virtual machines from a **VMware ESXi** environment to a **KubeVirt** environment in a Kubernetes cluster.
It leverages the **Forklift operator** for the migration process, ensuring a smooth transition of VMs.

The blueprint will deploy the necessary resources needed to achieve the migration, including:
- Source and target providers (e.g., VMware and KubeVirt)
- Network and storage mappings
- Migration plans

## Requirements

For this blueprint to function correctly, your Kubernetes cluster **must** have the following operators installed and running on the target Kubernetes cluster. Please follow their official documentation for installation instructions.

1.  **KubeVirt Operator:**
    -   **Purpose:** Manages KubeVirt virtual machine resources within the Kubernetes cluster.

2.  **Forklift Operator:**
    -   **Purpose:** Orchestrates the migration of virtual machines from a source environment (like VMware) to a KubeVirt environment.

## Usage

### Install the Helm Chart

Download Helm Chart values:

```sh
helm repo add marketplace https://marketplace.krateo.io
helm repo update marketplace
helm inspect values marketplace/vm-migration --version 0.1.1 > ~/vm-migration-values.yaml
```

Modify the *vm-migration-values.yaml* file as the following example:

```yaml

```

Install the Blueprint:

```sh
helm install <release-name> vm-migration \
  --repo https://marketplace.krateo.io \
  --namespace <release-namespace> \
  --create-namespace \
  -f ~/vm-migration-values.yaml \
  --version 0.1.1 \
  --wait
```

### Install using Krateo Composable Operation

Install the CompositionDefinition for the *Blueprint*:

```sh
cat <<EOF | kubectl apply -f -
apiVersion: core.krateo.io/v1alpha1
kind: CompositionDefinition
metadata:
  name: vm-migration
  namespace: openshift-mtv
spec:
  chart:
    repo: vm-migration
    url: https://marketplace.krateo.io
    version: 0.1.1
EOF
```

Create a *VmMigration* Composition:

```sh
cat <<EOF | kubectl apply -f -
apiVersion: composition.krateo.io/v0-1-1
kind: VmMigration
metadata:
  name: ovh-rosa-composition
  namespace: openshift-mtv
spec:
  esxi:
    user: # username
    password: # password
EOF
```

### Install using Krateo Composable Portal
