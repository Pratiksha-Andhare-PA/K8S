# 🚀 Blue-Green Deployment using Kubernetes

This project demonstrates a **Blue-Green Deployment strategy** using **Kubernetes Deployments and Services** to achieve **zero-downtime application releases** for a Maven-based Java web application.

---

## 📑 Table of Contents

- [What is Blue-Green Deployment?](#-what-is-blue-green-deployment)
- [Technologies Used](#-technologies-used)
- [Project Structure](#-project-structure)
- [Step 1: Apply Blue Deployment and Live Service](#-step-1-apply-blue-deployment-and-live-service)
- [Step 2: Verify Blue Pods and Services](#-step-2-verify-blue-pods-and-services)
- [Step 3: Verify Live Service Routing (Blue Active)](#-step-3-verify-live-service-routing-blue-active)
- [Step 4: Apply Green Deployment and Pre-Prod Service](#-step-4-apply-green-deployment-and-pre-prod-service)
- [Step 5: Verify Pre-Prod Service Routing](#-step-5-verify-pre-prod-service-routing)
- [Step 6: Switch Live Service Selector (Blue → Green)](#-step-6-switch-live-service-selector-blue--green)
- [Step 7: Verify Live Service Routing (Green Active)](#-step-7-verify-live-service-routing-green-active)
- [Rollback (Green → Blue)](#-rollback-green--blue)
- [Conclusion](#-conclusion)

---

## 📌 What is Blue-Green Deployment?

Blue-Green Deployment is a release strategy where two identical environments exist:

- **Blue** → Current production version
- **Green** → New release version

Traffic is switched between environments by updating the **Live Kubernetes Service selector**, ensuring **zero downtime** and **instant rollback**.

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

## 🚀 Step 1: Apply Blue Deployment and Live Service

### Deploy Blue Environment

```<cmd>
kubectl apply -f 01_blue_deploy.yml
```

### Create Live Service

```<cmd>
kubectl apply -f 02_live_service.yml
```

---

## 🔍 Step 2: Verify Blue Pods and Services

### Verify Pods

```<cmd>
kubectl get pods
```

📸 Evidence: screenshots/blue_pods_running.png

### Verify Services

```<cmd>
kubectl get svc
```

📸 Evidence: screenshots/services_list.png

---

## 🌐 Step 3: Verify Live Service Routing (Blue Active)

```text
http://<LIVE_SERVICE_ENDPOINT>/bluegreen-app
```

📸 Evidence: screenshots/live_service_blue_active.png

---

## 🟢 Step 4: Apply Green Deployment and Pre-Prod Service

### Deploy Green Environment

```<cmd>
kubectl apply -f 03_green_deploy.yml
```

### Create Pre-Prod Service

```<cmd>
kubectl apply -f 04_preprod_service.yml
```

---

## 🔵 Step 5: Verify Pre-Prod Service Routing

```text
http://<PREPROD_SERVICE_ENDPOINT>/bluegreen-app
```

📸 Evidence: screenshots/preprod_service_blue_active.png

---

## 🔄 Step 6: Switch Live Service Selector (Blue → Green)

### Update selector in live service

```yaml
selector:
  app: bluegreen-app
  color: green
```

📸 Evidence: screenshots/live_service_selector_change.png

### Apply updated configuration

```<cmd>
kubectl apply -f 02_live_service.yml
```

---

## ✅ Step 7: Verify Live Service Routing (Green Active)

```text
http://<LIVE_SERVICE_ENDPOINT>/bluegreen-app
```

📸 Evidence: screenshots/live_service_green_active.png

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

This project demonstrates a **production-grade Blue-Green Deployment** using Kubernetes with controlled traffic switching, zero downtime, and instant rollback.

---
