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

📸 Evidence:

<img width="940" height="167" alt="image" src="https://github.com/user-attachments/assets/db81058f-80e2-44a1-8cf2-6cc0b9396033" />


### Verify Services

```<cmd>
kubectl get svc
```

---

## 🌐 Step 3: Verify Live Service Routing (Blue Active)

```text
http://<LIVE_SERVICE_ENDPOINT>/bluegreen-app
```

📸 Evidence:

<img width="940" height="277" alt="image" src="https://github.com/user-attachments/assets/bdb7a1eb-101e-42e1-b677-c1c4142d9371" />


---

## 🟢 Step 4: Apply Green Deployment and Pre-Prod Service

### Deploy Green Environment

```<cmd>
kubectl apply -f 03_green_deploy.yml
```

### Verify Pods

```<cmd>
kubectl get pods
```

📸 Evidence:

<img width="940" height="213" alt="image" src="https://github.com/user-attachments/assets/a7e261e7-b9ce-4216-ac29-597325995403" />


### Create Pre-Prod Service

```<cmd>
kubectl apply -f 04_preprod_service.yml
```

### Verify Services

```<cmd>
kubectl get svc
```

Evidence:

<img width="940" height="224" alt="image" src="https://github.com/user-attachments/assets/8630ea0c-1064-4b0a-9c9d-59df516912c6" />


---

## 🔵 Step 5: Verify Pre-Prod Service Routing

```text
http://<PREPROD_SERVICE_ENDPOINT>/bluegreen-app
```

📸 Evidence:

<img width="940" height="291" alt="image" src="https://github.com/user-attachments/assets/5e6d3e69-044b-461b-8fb8-126610c69e6b" />


---

## 🔄 Step 6: Switch Live Service Selector (Blue → Green)

### Update selector in live service

```yaml
selector:
  app: bluegreen-app
  color: green
```

📸 Evidence

<img width="593" height="449" alt="image" src="https://github.com/user-attachments/assets/73f52c89-8245-47ce-a49d-fe71f259f25c" />


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

<img width="940" height="246" alt="image" src="https://github.com/user-attachments/assets/873054a3-be2a-465b-9a8c-c96f052fb96d" />


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
