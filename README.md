
# Argo CD – App of Apps (Minimal Example)

This repository demonstrates a **minimal Argo CD App of Apps setup** using only **two Applications**:

1. **Root Application** – installed manually in Argo CD
2. **Child Application** – automatically created and managed by the root app

The child application deploys a simple Kubernetes **ConfigMap**.

---

## Repository Structure

```text
.
├── apps
│   ├── child-app.yaml
│   └── config
│       └── configmap.yaml
└── root-app.yaml
````

---

## Applications Overview

### 1️⃣ Root Application (`root-app.yaml`)

* Installed **manually** in Argo CD
* Responsible only for managing **child Application resources**
* Points to the repository root (`.`)
* Automatically creates and prunes child applications

📌 This is the **only Application you apply manually**.

---

### 2️⃣ Child Application (`apps/child-app.yaml`)

* Created automatically by the root application
* Deploys Kubernetes resources from `apps/config`
* Manages a simple ConfigMap
* Fully automated with auto-sync, prune, and self-heal enabled

---

## Deployed Resource

### ConfigMap (`apps/config/configmap.yaml`)

A simple ConfigMap used to validate deployment:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: demo-config
  namespace: demo
data:
  MESSAGE: "Hello from App of Apps"
```

---

## How to Install

### Step 1: Install the Root App

Apply the root application manually:

```bash
kubectl apply -f root-app.yaml
```

Or create it via the Argo CD UI.

---

### Step 2: What Happens Automatically

* Argo CD syncs `root-app`
* `child-app` is created automatically
* The ConfigMap is deployed into the target namespace
* Both applications reach **Synced / Healthy** state

---

## Sync Behavior

* Updating the **ConfigMap** triggers only the **child app**
* Updating `child-app.yaml` triggers the **root app**
* Adding/removing child apps is handled automatically
* No infinite sync loops occur due to strict responsibility separation

---

## Key Design Principles

* Root app manages **Applications only**
* Child app manages **Kubernetes resources only**
* Clean separation prevents redeploy loops
* Git is the single source of truth

---

## Use Cases

* Platform bootstrapping
* Config-only deployments
* Team-based application onboarding
* Foundation for Helm, Kustomize, or ApplicationSet

---

## Notes

* This example intentionally keeps everything minimal
* Easily extendable to:

  * Multiple child apps
  * Environment folders (dev/test/prod)
  * Helm or Kustomize
  * ApplicationSets

---

## Author

**Joby Pooppillikudiyil**

Minimal. Declarative. GitOps-first.
