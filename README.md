# Brain Tasks App – Production-Style Deployment on Amazon EKS

This project demonstrates how a React application can be containerized, pushed to AWS ECR, and deployed on Amazon EKS using Kubernetes.  
The focus of this project is not only deployment, but also real-world DevOps troubleshooting and decision-making.

---

## 📌 Project Overview

- A React application was cloned from GitHub.
- The application was dockerized using Nginx.
- The Docker image was built and pushed to AWS Elastic Container Registry (ECR).
- An Amazon EKS cluster with managed worker nodes was created.
- The application was deployed to Kubernetes using Deployment and Service manifests.
- The application was successfully accessed and validated.

This project reflects **real production challenges**, not just a happy-path setup.

---

## 🛠️ Tech Stack

- **Frontend:** React
- **Containerization:** Docker
- **Container Registry:** AWS ECR
- **Orchestration:** Kubernetes
- **Cloud Provider:** AWS (EKS, EC2, IAM)
- **Web Server:** Nginx

---

## 🧱 Architecture Summary

- EKS worker nodes run in **private subnets** (best practice).
- The application runs inside a Kubernetes Pod.
- Traffic is routed through a Kubernetes Service.
- Since the cluster uses private networking and EKS Auto Mode, the application is accessed securely using port forwarding.

---

## 🚀 Deployment Flow

### 1. Clone the Application Repository
```bash
git clone https://github.com/Vennilavan12/Brain-Tasks-App.git
cd Brain-Tasks-App
2. Dockerize the Application
A Dockerfile was created to serve the React build using Nginx.

bash
Copy code
docker build --platform linux/amd64 -t brain-tasks-app .
Note: The image was explicitly built for linux/amd64 to match the EKS worker node architecture.

3. Push Image to AWS ECR
bash
Copy code
docker tag brain-tasks-app:latest <account-id>.dkr.ecr.ap-south-1.amazonaws.com/brain-tasks-app:latest
docker push <account-id>.dkr.ecr.ap-south-1.amazonaws.com/brain-tasks-app:latest
4. Create EKS Cluster and Node Group
An Amazon EKS cluster was created.

Managed EC2 worker nodes were added.

kubectl was configured to connect to the cluster.

bash
Copy code
aws eks update-kubeconfig --region ap-south-1 --name brain-tasks-cluster
5. Deploy Application to Kubernetes
bash
Copy code
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
Verify:

bash
Copy code
kubectl get pods
kubectl get svc
🌐 Accessing the Application
Because:

The EKS worker nodes run in private subnets

AWS LoadBalancer provisioning was restricted due to EKS Auto Mode IAM limitations

The application was accessed using Kubernetes port-forwarding, which is a standard and secure approach for private clusters.

bash
Copy code
kubectl port-forward svc/brain-tasks-service 8080:80
Open in browser:

arduino
Copy code
http://localhost:8080
🧪 Validation
Pod status: Running

Deployment status: Available

Nginx logs confirm HTTP 200 responses

Application UI successfully loaded in browser

⚠️ Challenges Faced & Solutions
ARM vs AMD64 Architecture Issue
Docker image was initially built on Apple Silicon (ARM).

EKS nodes run on AMD64.

Fixed by rebuilding the image using:

bash
Copy code
docker build --platform linux/amd64
LoadBalancer Stuck in Pending
EKS cluster used private subnets and Auto Mode.

AWS IAM limitations prevented LoadBalancer creation.

Instead of weakening security, the app was exposed using port-forwarding.

This reflects real-world DevOps decision-making.

📂 Repository Structure
mathematica
Copy code
Brain-Tasks-App/
├── Dockerfile
├── nginx.conf
├── k8s/
│   ├── deployment.yaml
│   └── service.yaml
├── dist/
└── README.md
✅ Conclusion
This project demonstrates:

End-to-end containerized application deployment

Practical Kubernetes usage

Real-world AWS troubleshooting

Secure and production-aware design decisions

The application was successfully deployed, validated, and accessed on Amazon EKS.

👤 Author
Sabari
DevOps Engineer (Hands-on Project)







