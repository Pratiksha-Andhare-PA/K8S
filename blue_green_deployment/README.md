# 🚀 Blue-Green Deployment using Kubernetes

This project demonstrates a **Blue-Green Deployment strategy** using **Kubernetes Deployments and Services** to achieve **zero-downtime application releases** for a Maven-based Java web application.

---

## 📑 Table of Contents

- [What is Blue-Green Deployment?](#-what-is-blue-green-deployment)
- [Technologies Used](#-technologies-used)
- [Project Structure](#-project-structure)
- [Step 1: Apply Kubernetes Manifests](#-step-1-apply-kubernetes-manifests)
- [Step 2: Verify Pods](#-step-2-verify-pods)
- [Step 3: Verify Services](#-step-3-verify-services)
- [Step 4: Before Switch (Blue Live, Green Pre-Prod)](#-step-4-before-switch-blue-live-green-pre-prod)
- [Step 5: Switch Live Traffic (Blue → Green)](#-step-5-switch-live-traffic-blue--green)
- [Step 6: After Switch Verification](#-step-6-after-switch-verification)
- [Rollback (Green → Blue)](#-rollback-green--blue)
- [Conclusion](#-conclusion)

---

## 📌 What is Blue-Green Deployment?

Blue-Green Deployment is a release strategy where two identical environments exist:

- **Blue** → Current production (live)
- **Green** → New version (pre-production)

Traffic is switched between environments by updating the **Kubernetes Service selector**, ensuring **zero downtime** and **instant rollback**.

---

## 🛠️ Technologies Used

- Kubernetes
- Docker
- Java Maven Web Application
- kubectl
- Docker Hub

---

## 📂 Project Structure

```text
.
├── 01_blue_deploy.yml
├── 02_live_service.yml
├── 03_green_deploy.yml
├── 04_preprod_service.yml
└── README.md
```

---

## 🚀 Step 1: Apply Kubernetes Manifests

### Deploy Blue Environment

```
kubectl apply -f 01_blue_deploy.yml
```

### Expose Blue as Live using LoadBalancer

```
kubectl apply -f 02_live_service.yml
```

---

## 🔍 Step 2: Verify Pods

```
kubectl get pods
```

📸 Evidence: screenshots/blue_pods_running.png

<img width="940" height="167" alt="image" src="https://github.com/user-attachments/assets/bfe7014a-5dc7-4c53-9bff-fb1f535b7e8c" />


---

## 🌐 Step 3: Verify Services

```
kubectl get svc
```

📸 Evidence: screenshots/services_list.png

---

## 🔵 Step 4: Before Switch (Blue Live, Green Pre-Prod)

### Access Blue (Live)

```text
http://<LOAD_BALANCER_IP>/bluegreen-app
```

📸 Evidence: screenshots/blue_live_lb.png

---

### Deploy Green Environment

```
kubectl apply -f 03_green_deploy.yml
```

### Expose Green via NodePort (Pre-Prod)

```<cmd>
kubectl apply -f 04_preprod_service.yml
```

### Access Green (Pre-Prod)

```text
http://<NODE_IP>:31785/bluegreen-app
```

📸 Evidence: screenshots/green_preprod_nodeport.png

---

## 🔄 Step 5: Switch Live Traffic (Blue → Green)

Update selector in live service:

```yaml
selector:
  app: bluegreen-app
  color: green
```

Apply changes:

```<cmd>
kubectl apply -f 02_live_service.yml
```

---

## ✅ Step 6: After Switch Verification

```text
http://<LOAD_BALANCER_IP>/bluegreen-app
```

📸 Evidence: screenshots/green_live_lb.png

---

## 🔙 Rollback (Green → Blue)

```yaml
selector:
  app: bluegreen-app
  color: blue
```

```<cmd>
kubectl apply -f 02_live_service.yml
```

---

## 🎯 Conclusion

This project demonstrates a **production-grade Blue-Green Deployment** using Kubernetes with zero downtime and instant rollback.
