# 🚀 DevOps Project – Blue-Green Deployment CI/CD Pipeline with AWS

## 📌 Project Overview
This project implements a **Blue-Green Deployment strategy** as part of a complete DevOps pipeline.  
It ensures **zero downtime deployment** by switching traffic seamlessly from the *Blue* environment (current version) to the *Green* environment (new version).  

The objective of this project is to achieve:  
- Fully automated builds, tests, and deployments.  
- Highly available and scalable application on AWS EKS.  
- Continuous monitoring and logging with real-time feedback.  
- Secure and reliable infrastructure through automation.  

---

## 🛠️ Tools & Technologies Used
- **Version Control:** Git & GitHub  
- **CI/CD:** Jenkins  
- **Code Quality & Security:** SonarQube, Trivy  
- **Build Tool:** Maven  
- **Containerization:** Docker  
- **Artifact Management:** Nexus Artifactory  
- **Orchestration:** Kubernetes (EKS)   
- **Load Balancing & Routing:** Nginx Ingress  
- **Auto Scaling:** EKS  
- **Infrastructure as Code (IaC):** Terraform  
- **Cloud Platform:** AWS (EC2, EKS, IAM)  

---

## 📂 Project Workflow

### 🔹 1. Code Commit & Source Control
- Developers push source code into the GitHub repository.  
- Webhooks trigger the Jenkins pipeline.  

---

### 🔹 2. Infrastructure Provisioning
- AWS infrastructure provisioned using **Terraform**.  
- Resources include VPC, subnets, EKS cluster, IAM roles, and storage.  

<img width="1166" height="607" alt="infra" src="https://github.com/user-attachments/assets/3b3e8ee2-3674-4fe4-bea9-cdd9f25856c1" />

---

### 🔹 3. Jenkins Setup & Tool Integration
- Jenkins integrates with GitHub, SonarQube, Docker Hub, and Kubernetes.  
- Credentials securely managed with Jenkins Credentials Manager.  

<img width="1327" height="675" alt="jenkins-tools" src="https://github.com/user-attachments/assets/44eaffa2-decf-4898-a96d-7d9ad389b2c0" />

---

### 🔹 4. Kubernetes Cluster Creation
- EKS cluster created with proper IAM roles & security groups.  
- Worker nodes provisioned for application deployments.  

<img width="1365" height="630" alt="eks" src="https://github.com/user-attachments/assets/5dfab4e3-57d6-4ad0-926e-8446e9d75701" />

---

### 🔹 5. CI/CD Pipeline Execution
- **Build Stage:** Maven compiles and packages the application.  
- **Security Stage:** Trivy scans source code & Docker images.  
- **Quality Stage:** SonarQube performs code analysis.  

<img width="1350" height="676" alt="sonarqube" src="https://github.com/user-attachments/assets/d9144aa5-9bc1-472b-a7b0-0ec9b3a1a829" />

---

### 🔹 6. Containerization & Artifact Management
- Docker image is built and tagged with build ID.  
- Image pushed to Docker Hub (can be extended to JFrog Artifactory).  

---

### 🔹 7. Deployment to Kubernetes (Blue-Green Strategy)
- **Blue Deployment**: Current version serving live traffic.  
- **Green Deployment**: New version deployed alongside Blue.  
- Once validated, traffic is switched to Green using **Nginx Ingress**.  

<img width="1348" height="678" alt="bluegreen" src="https://github.com/user-attachments/assets/2ee62d52-f42b-4f77-9b2f-fad430720c95" />  
<img width="1366" height="677" alt="k8s" src="https://github.com/user-attachments/assets/8c85022b-b535-424d-b664-34ce6d4117a5" />

---


## 📸 Architecture Diagram
![Architecture](docs/blue-green-architecture.png)

---

## ✅ Project Outcome
- **Zero-downtime deployments** with Blue-Green strategy.  
- Automated pipeline from **code commit → deployment**.  
- Highly scalable and fault-tolerant Kubernetes cluster.  
- Integrated **security scanning, code analysis**.  
- Infrastructure provisioning automated with Terraform.  
- Improved developer productivity and faster release cycles.  

---

## 📊 Visual Summary of Pipeline
```mermaid
flowchart TD
    A[Developer Pushes Code] --> B[Jenkins Pipeline Triggered]
    B --> C[Build with Maven]
    C --> D[Trivy Security Scan]
    D --> E[SonarQube Code Quality]
    E --> F[Docker Build & Push to Registry]
    F --> H[Blue-Green Deployment on EKS]
    G --> I[Traffic Routed via Nginx Ingress]
    

