
# 🚀 ArgoCD Kustomize Applications

This repository contains ArgoCD applications structured with **base** and **overlays** for multiple environments.

## 📁 Directory Structure

```

argo-apps/
├── springboot-hello-world/
│   ├── base/
│   └── overlays/
│       ├── dev/
│       └── test/
├── dox-demo-maven-springboot/
│   ├── base/
│   └── overlays/
│       ├── dev/
│       └── test/
└── job-orchestrator/
├── base/
└── overlays/
├── dev/
└── test/

````


<details>

````

└── argo-apps
    ├── dox-demo-maven-springboot
    │   ├── base
    │   │   ├── deployment.yaml
    │   │   ├── kustomization.yaml
    │   │   └── service.yaml
    │   └── overlays
    │       ├── dev
    │       │   ├── kustomization.yaml
    │       │   └── patch-image.yaml
    │       └── test
    │           ├── kustomization.yaml
    │           └── patch-image.yaml
    ├── job-orchestrator
    │   ├── base
    │   │   ├── deployment.yaml
    │   │   ├── kustomization.yaml
    │   │   └── service.yaml
    │   └── overlays
    │       ├── dev
    │       │   ├── kustomization.yaml
    │       │   └── patch-image.yaml
    │       └── test
    │           ├── kustomization.yaml
    │           └── patch-image.yaml
    └── springboot-hello-world
        ├── base
        │   ├── deployment.yaml
        │   ├── kustomization.yaml
        │   └── service.yaml
        └── overlays
            ├── dev
            │   ├── kustomization.yaml
            │   └── patch-image.yaml
            └── test
                ├── kustomization.yaml
                └── patch-image.yaml
                
````                
</details>
Each application has:
- **base**: shared Kustomize configuration (deployments, services, ingress, etc.)
- **overlays**: environment-specific configuration for `dev` and `test`

---

## 🧩 ArgoCD Application Definitions

Below are ArgoCD application manifests for each app in both `dev` and `test` environments.

---

### 🌱 1. Springboot Hello World

#### Dev
```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: dev-springboot-hello-world
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/devops-poc-kustomize-helm/argo-kustomize-apps.git
    targetRevision: HEAD
    path: argo-apps/springboot-hello-world/overlays/dev
  destination:
    server: https://kubernetes.default.svc
    namespace: dev
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
    syncOptions:
      - CreateNamespace=true
````

#### Test

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: test-springboot-hello-world
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/devops-poc-kustomize-helm/argo-kustomize-apps.git
    targetRevision: HEAD
    path: argo-apps/springboot-hello-world/overlays/test
  destination:
    server: https://kubernetes.default.svc
    namespace: test
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
    syncOptions:
      - CreateNamespace=true
```

---

### ⚙️ 2. Dox Demo Maven Spring Boot

#### Dev

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: dev-dox-demo-maven-springboot
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/devops-poc-kustomize-helm/argo-kustomize-apps.git
    targetRevision: HEAD
    path: argo-apps/dox-demo-maven-springboot/overlays/dev
  destination:
    server: https://kubernetes.default.svc
    namespace: dev
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
    syncOptions:
      - CreateNamespace=true
```

#### Test

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: test-dox-demo-maven-springboot
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/devops-poc-kustomize-helm/argo-kustomize-apps.git
    targetRevision: HEAD
    path: argo-apps/dox-demo-maven-springboot/overlays/test
  destination:
    server: https://kubernetes.default.svc
    namespace: test
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
    syncOptions:
      - CreateNamespace=true
```

---

### 🧠 3. Job Orchestrator

#### Dev

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: dev-job-orchestrator
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/devops-poc-kustomize-helm/argo-kustomize-apps.git
    targetRevision: HEAD
    path: argo-apps/job-orchestrator/overlays/dev
  destination:
    server: https://kubernetes.default.svc
    namespace: dev
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
    syncOptions:
      - CreateNamespace=true
```

#### Test

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: test-job-orchestrator
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/devops-poc-kustomize-helm/argo-kustomize-apps.git
    targetRevision: HEAD
    path: argo-apps/job-orchestrator/overlays/test
  destination:
    server: https://kubernetes.default.svc
    namespace: test
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
    syncOptions:
      - CreateNamespace=true
```

---

## 📦 Optional: App-of-Apps Pattern

You can manage all these applications from a single **parent ArgoCD Application**:

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: argo-apps-root
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/devops-poc-kustomize-helm/argo-kustomize-apps.git
    targetRevision: HEAD
    path: argo-apps
  destination:
    server: https://kubernetes.default.svc
    namespace: argocd
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```

---

## 🧾 Usage

1. Clone this repository:

   ```bash
   git clone https://github.com/devops-poc-kustomize-helm/argo-kustomize-apps.git
   cd argo-apps
   ```
2. Apply your chosen environment manifests:

   ```bash
   kubectl apply -f readme.md  # or copy YAML snippets
   ```
3. Check applications in ArgoCD UI.

---

📘 **Maintainer**: DevOps Team
📅 **Last Updated**: October 2025
🌐 **Repo**: [https://github.com/devops-poc-kustomize-helm/argo-apps](https://github.com/devops-poc-kustomize-helm/argo-kustomize-apps)
