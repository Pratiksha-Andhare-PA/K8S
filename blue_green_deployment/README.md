🚀 Blue-Green Deployment using Kubernetes

This project demonstrates a Blue-Green Deployment strategy using Kubernetes Deployments and Services to achieve zero-downtime application releases.

📑 Table of Contents

What is Blue-Green Deployment?

Technologies Used

Project Structure

Architecture Diagram

Step 1: Apply Kubernetes Manifests

Step 2: Verify Pods

Step 3: Verify Services

Step 4: Before Switch (Blue Live, Green Pre-Prod)

Step 5: Switch Live Traffic (Blue → Green)

Step 6: After Switch Verification

Rollback

Key Benefits

Conclusion

📌 What is Blue-Green Deployment?

Blue-Green Deployment is a release strategy where:

Blue → current production version

Green → new version deployed alongside Blue

Traffic is switched using a Kubernetes Service selector

Rollback is instant and safe

🛠️ Technologies Used

Kubernetes

Docker

kubectl

NodePort & LoadBalancer

VS Code

📂 Project Structure
.
├── 01_blue_deploy.yml
├── 02_live_service.yml
├── 03_green_deploy.yml
├── 04_preprod_service.yml
├── README.md

📌 All screenshots are stored in the root directory and referenced directly in README.

🏗️ Architecture Diagram
graph TD
    User -->|HTTP| LB[Live Service - LoadBalancer]
    LB -->|Before Switch| BluePods[Blue Pods v2]
    LB -->|After Switch| GreenPods[Green Pods v1]
    Tester -->|NodePort| PreProdSvc[Pre-Prod Service]
    PreProdSvc --> GreenPods

🚀 Step 1: Apply Kubernetes Manifests

Apply all deployments and services.

kubectl apply -f blue_deploy.yml
kubectl apply -f green_deploy.yml
kubectl apply -f live_service.yml
kubectl apply -f preprod_service.yml

📦 Step 2: Verify Pods
kubectl get pods


📸 Screenshot – Pods Running (Blue & Green)


✔️ Confirms both Blue and Green pods are running simultaneously.

🌐 Step 3: Verify Services
kubectl get svc


📸 Screenshot – Services Created


✔️ Confirms:

Live service → LoadBalancer

Pre-prod service → NodePort

🔍 Step 4: Before Switch (Blue Live, Green Pre-Prod)
🔵 Live Service (Blue – Production)

Access using LoadBalancer URL.

📸 Screenshot – Live Service Running on Blue


🟢 Pre-Prod Service (Green – NodePort)

Access using:

http://<node-ip>:31785


📸 Screenshot – Pre-Prod Running on Green


🔧 Step 5: Switch Live Traffic (Blue → Green)

Update the live service selector.

selector:
  app: bluegreen-app
  color: green


📸 Screenshot – Selector Change in live_service.yml


Or using command:

kubectl patch svc bluegreen-service \
-p '{"spec":{"selector":{"app":"bluegreen-app","color":"green"}}}'

✅ Step 6: After Switch Verification

Now both live and pre-prod services serve Green version.

📸 Screenshot – Live & Pre-Prod on Green


🔍 Additional Verification (Optional)
kubectl describe svc bluegreen-preprod-service


📸 Screenshot – Service Endpoints


✔️ Confirms service is correctly routing traffic to Green pods.

⏪ Rollback (Green → Blue)
kubectl patch svc bluegreen-service \
-p '{"spec":{"selector":{"app":"bluegreen-app","color":"blue"}}}'


Rollback is instant with zero downtime.

🎯 Key Benefits

Zero-downtime deployments

Safe pre-production testing

Instant rollback

No pod restarts during traffic switch

Production-grade Kubernetes strategy

📌 Conclusion

This project demonstrates a real-world Blue-Green Deployment implementation using Kubernetes, where traffic is safely controlled using labels and service selectors, ensuring reliability and high availability.

👩‍💻 Author

Pratiksha Andhare
DevOps | Kubernetes | Cloud