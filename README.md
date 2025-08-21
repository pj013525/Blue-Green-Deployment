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

2. Terraform provisions AWS infrastructure automatically.
<img width="1166" height="607" alt="image" src="https://github.com/user-attachments/assets/3b3e8ee2-3674-4fe4-bea9-cdd9f25856c1" />

3. Set the Infrastructure for deployment

4. Integrate every tool and Credentials in Jenkins
<img width="1327" height="675" alt="image" src="https://github.com/user-attachments/assets/44eaffa2-decf-4898-a96d-7d9ad389b2c0" />


5. Create an EKS cluster with roles
<img width="1365" height="630" alt="image" src="https://github.com/user-attachments/assets/5dfab4e3-57d6-4ad0-926e-8446e9d75701" />


6. Create a declarative pipeline

7. Jenkins triggers build & test pipeline.

8. Trivy scan the files  

9. SonarQube checks code quality and vulnerabilities.  
<img width="1350" height="676" alt="image" src="https://github.com/user-attachments/assets/d9144aa5-9bc1-472b-a7b0-0ec9b3a1a829" />


10. Docker image is created and pushed to Docker Hub.

11.  Trivy scan the images

12. Deploys the application to Kubernetes.  
<img width="1348" height="678" alt="image" src="https://github.com/user-attachments/assets/2ee62d52-f42b-4f77-9b2f-fad430720c95" />
<img width="1366" height="677" alt="image" src="https://github.com/user-attachments/assets/8c85022b-b535-424d-b664-34ce6d4117a5" />




---

## ✅ Project Outcome
- Automated CI/CD pipeline from code to deployment.  
- Scalable and fault-tolerant Kubernetes cluster.  
- Centralized monitoring and logging.  
- Infrastructure provisioning through Terraform.  
