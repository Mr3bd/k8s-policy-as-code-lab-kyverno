# 🛡️ Kubernetes Policy-as-Code Security Lab (Kyverno)

A hands-on **Kubernetes Security Lab** demonstrating how to enforce **Policy-as-Code** using Kyverno to prevent insecure workloads before they reach the cluster.

This project simulates real-world Kubernetes attack scenarios and shows how **admission control policies** can enforce security guardrails at deployment time.

Built for:

- 🔐 Cloud Security Engineers  
- ⚙️ DevSecOps Engineers  
- ☸️ Kubernetes Platform Engineers  
- 🎯 Anyone learning Kubernetes security & admission control

---

# 🚀 What This Project Demonstrates

This lab implements a **security-first Kubernetes cluster** using Kyverno policies.

It focuses on preventing common Kubernetes misconfigurations such as:

- Privileged container execution
- Root user workloads
- Missing resource limits
- Unsafe Linux capabilities
- Host-level access (hostNetwork / hostPath)
- Untrusted container registries

👉 Every insecure workload is blocked **before deployment**

---

# 🧱 Security Model

This project implements a **preventive security model**:

```
Developer YAML → Kyverno Admission Controller → Kubernetes API Server

                        ↓

         ❌ Reject (Insecure Workload)
         ✅ Allow (Secure Workload)
```

Security is enforced at the **admission layer**, not after deployment.

---

# 🛡️ Security Policies Implemented

| Policy | What It Prevents |
|--------|------------------|
| disallow-privileged | Prevents privileged containers and host-level breakout |
| require-run-as-nonroot | Blocks root user containers |
| require-resource-limits | Prevents resource exhaustion (DoS risk) |
| disallow-hostnetwork | Prevents host network access and traffic sniffing |
| disallow-hostpath | Prevents mounting host filesystem into containers |
| restrict-capabilities | Removes dangerous Linux capabilities |
| require-readiness-probes | Ensures workload health checks exist |
| restrict-image-registries | Blocks untrusted container image sources |

---

# 🧱 Project Structure

```
.
├── README.md
├── policies/
│   ├── disallow-hostnetwork.yaml
│   ├── disallow-hostpath.yaml
│   ├── disallow-privileged.yaml
│   ├── require-readiness-probes.yaml
│   ├── require-resource-limits.yaml
│   ├── require-run-as-nonroot.yaml
│   ├── restrict-capabilities.yaml
│   └── restrict-image-registries.yaml
└── tests/
    ├── bad-pod.yaml
    └── good-pod.yaml
```

---

# ⚙️ Prerequisites

Make sure you have:

- Docker Desktop
- Minikube
- kubectl
- Helm

---

# 🚀 Setup Instructions

## 1️⃣ Start Kubernetes Cluster (Minikube)

```bash
minikube start --driver=docker
```

---

## 2️⃣ Add Kyverno Helm Repository

```bash
helm repo add kyverno https://kyverno.github.io/kyverno/
helm repo update
```

---

## 3️⃣ Install Kyverno

```bash
helm install kyverno kyverno/kyverno -n kyverno --create-namespace
```

Verify installation:

```bash
kubectl get pods -n kyverno
```

---

## 4️⃣ Deploy Security Policies

```bash
kubectl apply -f policies/
```

Expected output:

```
clusterpolicy.kyverno.io/...
```

---

# 🧪 Testing the Policies

## ❌ 1. Insecure Workload (Should FAIL)

```bash
kubectl apply -f tests/bad-pod.yaml
```

Expected result:

- ❌ Error from server: error when creating
- Multiple policy violations triggered

---

## ✅ 2. Secure Workload (Should PASS)

```bash
kubectl apply -f tests/good-pod.yaml
```

Expected result:

- ✅ pod/good-pod created
- No policy violations

---

# 🔐 Key Security Principles Demonstrated

### 1. Zero Trust at Admission Layer
Nothing is trusted by default.

### 2. Least Privilege Containers
Containers run with minimal permissions.

### 3. Supply Chain Security
Only approved registries are allowed.

### 4. Runtime Hardening
Linux capabilities and host access are restricted.

### 5. Preventive Security Model
Misconfigurations are blocked before production.

---

## 🌟 Star this repo if you find it useful!

Made with 💻 by a Cloud Security Engineer, for Cloud Security Engineers.

---

## 👨‍💻 Author

[Abdullrahman Alhawamdeh](https://www.linkedin.com/in/mr3bd)

## 🌍 Website

https://mr3bd.com

Made with ❤️ using Kubernetes, k8s, Kyverno, minikube