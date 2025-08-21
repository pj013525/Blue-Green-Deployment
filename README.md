# 🚀 DevOps Project – CI/CD Pipeline with AWS

## 📌 Project Overview
This project demonstrates a **complete DevOps pipeline** starting from code commit to production deployment.  
The goal is to achieve automation, scalability, and monitoring for modern application development.

---

## 🛠️ Tools & Technologies Used
- Git & GitHub (Version Control)  
- Jenkins (CI/CD)  
- SonarQube (Code Quality & Security)  
- Maven (Build Automation)  
- Docker (Containerization)  
- JFrog Artifactory (Artifact Storage)  
- Kubernetes (Container Orchestration)  
- ArgoCD (GitOps Deployment)  
- Nginx Ingress (Load Balancing)  
- KEDA (Auto Scaling)  
- Prometheus & Loki (Monitoring & Logging)  
- Terraform (Infrastructure as Code)  
- AWS Cloud (EC2, EKS, S3, IAM)

---

## 📂 Project Workflow
1. Developers push code to GitHub.  
2. Jenkins triggers build & test pipeline.  
3. SonarQube checks code quality and vulnerabilities.  
4. Docker image is created and pushed to JFrog Artifactory.  
5. ArgoCD deploys the application to Kubernetes.  
6. Nginx Ingress manages traffic, and KEDA handles auto-scaling.  
7. Prometheus & Loki monitor system health and logs.  
8. Terraform provisions AWS infrastructure automatically.

---

## 📸 Architecture Diagram
![Architecture](docs/architecture.png)

---

## ✅ Project Outcome
- Automated CI/CD pipeline from code to deployment.  
- Scalable and fault-tolerant Kubernetes cluster.  
- Centralized monitoring and logging.  
- Infrastructure provisioning through Terraform.  
