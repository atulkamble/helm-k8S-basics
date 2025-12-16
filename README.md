## 1️⃣ Helm Introduction 

### What is Helm?

* Helm is the **package manager for Kubernetes**
* Helps you **define, install, upgrade, rollback, and manage Kubernetes applications**
* Uses **Helm Charts** (pre-configured Kubernetes resources)

### Why Helm is Needed?

* Kubernetes YAML files become **complex and repetitive**
* Managing versions, updates, and rollbacks manually is difficult
* Helm solves:

  * Reusability
  * Versioning
  * Configuration management
  * Easy upgrades & rollbacks

### Key Benefits

* 🚀 Faster application deployments
* 📦 Application packaging with charts
* 🔁 Rollback to previous versions easily
* ⚙️ Environment-specific configurations (dev, test, prod)
* 🔄 Simplifies CI/CD pipelines

### Helm Architecture

* **Helm Client** – CLI tool
* **Helm Charts** – Application packages
* **Release** – Deployed instance of a chart
* **Repository** – Storage for Helm charts

### Helm vs Plain Kubernetes YAML

| Kubernetes YAML    | Helm                  |
| ------------------ | --------------------- |
| Manual deployments | Automated deployments |
| No version control | Versioned releases    |
| Hard to rollback   | Easy rollback         |
| Repetition         | Templating & values   |

---

## 2️⃣ Helm Core Concepts

* **Chart** – Package of Kubernetes resources
* **Release** – Installed chart instance
* **Values** – Configuration input for charts
* **Templates** – Go-templated Kubernetes YAML
* **Repository** – Chart storage location

---

## 3️⃣ Helm Installation (Quick)

```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
helm version
```

---

## 4️⃣ Helm Commands List (Must-Know)

### Helm Help & Version

```bash
helm help
helm version
```

### Helm Repository Commands

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo list
helm repo update
helm repo remove bitnami
```

### Helm Search

```bash
helm search repo nginx
helm search hub mysql
```

### Helm Chart Management

```bash
helm create mychart
helm lint mychart
helm package mychart
helm dependency update
```

### Helm Install / Upgrade / Delete

```bash
helm install myapp mychart
helm install myapp mychart -f values.yaml
helm upgrade myapp mychart
helm uninstall myapp
```

### Helm Release Management

```bash
helm list
helm list -A
helm status myapp
helm history myapp
helm rollback myapp 1
```

### Helm Debug & Dry Run

```bash
helm install myapp mychart --dry-run
helm template mychart
```

---

## 5️⃣ Helm Chart Directory Structure (Basic)

```text
mychart/
├── Chart.yaml
├── values.yaml
├── charts/
├── templates/
│   ├── deployment.yaml
│   ├── service.yaml
│   └── _helpers.tpl
└── README.md
```

---

## 6️⃣ Basic Helm Kubernetes Project (Hands-on)

### 🎯 Project: Deploy NGINX using Helm

---

### Step 1: Create Helm Chart

```bash
helm create nginx-app
cd nginx-app
```

---

### Step 2: Update `values.yaml`

```yaml
replicaCount: 2

image:
  repository: nginx
  tag: latest

service:
  type: NodePort
  port: 80
```

---

### Step 3: Deployment Template (`templates/deployment.yaml`)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Release.Name }}-nginx
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
          ports:
            - containerPort: 80
```

---

### Step 4: Service Template (`templates/service.yaml`)

```yaml
apiVersion: v1
kind: Service
metadata:
  name: {{ .Release.Name }}-service
spec:
  type: {{ .Values.service.type }}
  selector:
    app: nginx
  ports:
    - port: {{ .Values.service.port }}
      targetPort: 80
```

---

### Step 5: Install Helm Chart

```bash
helm install nginx-release .
```

---

### Step 6: Verify

```bash
helm list
kubectl get pods
kubectl get svc
```

---

## 7️⃣ Upgrade & Rollback Demo

### Upgrade

```bash
helm upgrade nginx-release . --set replicaCount=3
```

### Rollback

```bash
helm history nginx-release
helm rollback nginx-release 1
```

---

## 8️⃣ Helm with CI/CD (Talking Points)

* Used in **GitHub Actions**, **Azure DevOps**, **Jenkins**
* Helm values injected per environment
* Common pattern:

  * Build image → Push to registry → Helm upgrade

---

## 9️⃣ Common Helm Interview / Training Points

* Difference between **chart** and **release**
* Role of `values.yaml`
* How rollback works in Helm
* Helm vs Kustomize
* Helm templating basics (`{{ .Values }}`, `{{ .Release.Name }}`)

---

## 🔚 Summary

* Helm simplifies Kubernetes deployments
* Ideal for **production-grade workloads**
* Widely used in DevOps & Cloud projects
* Must-know skill for **Kubernetes, AKS, EKS, GKE**
