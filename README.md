## Cloud-Native Web Application on GKE – GitOps CI/CD & Disaster Recovery
## Project Overview
This project demonstrates a production-grade CI/CD and Disaster Recovery (DR) pipeline for deploying a cloud-native web application on Google Kubernetes Engine (GKE) using GitOps principles.
The solution automates the full application lifecycle, including infrastructure provisioning, container image build and delivery, Kubernetes deployment, and full cluster backup and recovery.
It simulates real-world DevOps scenarios such as infrastructure failures, deployment errors, and data loss, while ensuring the system can be rapidly and reliably restored with minimal downtime.

---
## Problem Statement
Modern application development and deployment often face challenges such as complexity, inconsistent environments, and a lack of standardized best practices. Teams struggle to achieve fast iteration cycles while maintaining stability between development and production environments. In addition, building scalable and resilient infrastructure often requires significant manual effort and fragmented tooling.

- Traditional deployment approaches introduce several risks, including:
- Human error during application or infrastructure changes
- Lack of version control for Kubernetes manifests and infrastructure
- Slow and unreliable recovery from system or namespace failures
These challenges create a gap between development speed and operational reliability, leading to delays, misconfigurations, and increased operational costs.
---

 ## Problem Solution 
 This project provides a cloud-native DevOps framework that addresses these challenges through automation and declarative infrastructure principles.
By combining modern practices such as:
- TypeScript-based full-stack application development
- Containerization using Docker
- Infrastructure as Code (Terraform)
- CI/CD automation
- GitOps-based deployments
- Disaster recovery with Kubernetes backups
The solution delivers a streamlined and reliable workflow from code to deployment to recovery, allowing teams to focus on building features while ensuring operational excellence, scalability, and resilience.
---

## Key Features
- Full-Stack Cloud-Native Architecture: A modern web application built with a React (Vite) frontend and scalable backend services, designed for performance, modularity, and responsiveness.
- Containerized Development & Deployment: Uses Docker to ensure consistent environments across development, testing, and production, eliminating configuration drift and simplifying deployments.
- Infrastructure as Code (IaC): Cloud infrastructure is fully defined and managed using Terraform, enabling automated, repeatable, and version-controlled provisioning of resources.
- Automated CI/CD & GitOps Delivery: GitHub Actions automates the build and deployment pipeline, while Argo CD provides continuous GitOps-based synchronization of Kubernetes manifests for reliable and declarative deployments.
- Disaster Recovery Ready: Integrated with Velero to enable backup and restore of Kubernetes resources, ensuring resilience and fast recovery in case of system failure or data loss.
- Type-Safe Development: Built using TypeScript to improve code quality, maintainability, and scalability across the application.
- Cloud-Native Architecture: Designed for Kubernetes-based cloud environments with a focus on scalability, fault tolerance, and microservices principles.m
---

##  Architecture Overview
![Architecture Diagram](https://github.com/smogalloyubio/Googlecloud-project1/blob/main/picture/Database%20ER%20diagram%20(crow's%20foot).png)

**Infrastructure Layer**

* GKE cluster provisioned with Terraform
* Google Artifact Registry for container images
* Google Cloud Storage (GCS) bucket for Velero backups

**CI/CD & GitOps Layer**

* GitHub Actions pipeline builds and pushes Docker images
* Kubernetes manifests stored in GitHub
* Argo CD continuously syncs manifests to GKE

**Disaster Recovery Layer**

* Velero configured with GCS
* Namespace-scoped backups
* Restore tested after deletion

---
## 🧱 Technical Architecture

This project is built on a modern cloud-native technology stack designed for performance, scalability, and maintainability.

### ⚙️ Core Technologies

| Technology | Purpose | Key Benefit |
|------------|--------|-------------|
| TypeScript | Primary Development Language | Strong type safety, improved code quality, and maintainability |
| Node.js | Backend Runtime Environment | High-performance and scalable server-side execution |
| React / Vite | Frontend Framework / Build Tool | Fast UI rendering and rapid development experience |
| Docker | Containerization | Consistent environments across development and production |
| Terraform | Infrastructure as Code (IaC) | Automated, repeatable, and version-controlled infrastructure provisioning |
| Argo CD | GitOps Continuous Delivery | Declarative and automated Kubernetes deployments |
| Velero | Kubernetes Backup & Restore | Disaster recovery and cluster data protection |
| GitHub Actions | CI/CD Automation | Automated build, test, and deployment pipelines |
| Kubernetes | Container Orchestration | Scalability, high availability, and self-healing workloads |
---

## Key Features

* **Infrastructure as Code**: GKE provisioned using Terraform
* **CI Pipeline**: Automated Docker image build and push via GitHub Actions
* **GitOps Deployment**: Argo CD synchronizes cluster state from GitHub
* **Namespace Isolation**: Application deployed in a dedicated namespace
* **Disaster Recovery**: Velero backups stored in GCS
* **Recovery Validation**: Namespace or cluster deletion followed by restore
* **Resource Optimization**: Manual CPU patching for small-node environments
* **Security**: Dedicated service accounts and least-privilege IAM roles

---

##  Workflow

1. Terraform provisions GKE infrastructure
2. Code push triggers GitHub Actions workflow
3. Docker image is built and pushed to Artifact Registry
4. Kubernetes manifests are updated in GitHub
5. Argo CD syncs manifests to GKE
6. Application runs in a dedicated namespace
7. Velero backs up namespace resources to GCS
8. Namespace or cluster is deleted (failure simulation)
9. Velero restores workloads and data successfully

---

##  Step-by-Step Implementation Guide

### Step 1: Prerequisites

Ensure you have the following installed and configured:

* Google Cloud SDK (`gcloud`)
* Terraform
* Docker
* kubectl
* Git
* GitHub account

Authenticate with GCP:

```bash
gcloud auth login
gcloud config set project <PROJECT_ID>
gcloud  auth  application-deafult login 
```

---

### Step 2: Provision GKE with Terraform

1. Navigate to the Terraform directory:

```bash

sudo apt-get update && sudo apt-get install -y gnupg software-properties-common


wget -O- https://apt.releases.hashicorp.com/gpg | \
gpg --dearmor | \
sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg > /dev/null


echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
sudo tee /etc/apt/sources.list.d/hashicorp.list


sudo apt-get update && sudo apt-get install terraform

cd terraform

terraform init
terraform apply

```

Terraform provisions:

* GKE cluster
* Networking resources
* Required IAM roles and service accounts

**![Terraform  apply command](https://github.com/smogalloyubio/02-Devops-project-NetflixClone-app/blob/main/picture/Screenshot%202026-01-24%20at%2012.18.12.png):**



---

### Step 3: Build & Push Docker Image (CI Pipeline)

* GitHub Actions workflow builds the Docker image
* Image is tagged and pushed to Google Artifact Registry

Example Artifact Registry image format:

```text
REGION-docker.pkg.dev/PROJECT_ID/REPOSITORY/IMAGE:TAG
```

**![Gitaction pipeline](https://github.com/smogalloyubio/02-Devops-project-NetflixClone-app/blob/main/picture/Screenshot%202026-01-24%20at%2011.12.05.png)**


google cloud artifact registry 
![artifact registry](https://github.com/smogalloyubio/02-Devops-project-NetflixClone-app/blob/main/picture/Screenshot%202026-01-24%20at%2011.11.50.png)



---

### Step 4: Configure Kubernetes Manifests

* Define Kubernetes YAML manifests:

  * Namespace
  * Deployment
  * Service
 

Apply namespace locally (optional validation):

```bash
gcloud container clusters create netflix-cluster \
    --project=Porject_id \
    --zone=us-central1-a \
    --num-nodes=2 \
    --machine-type=e2-standard-2 \
    --enable-ip-alias \
    --release-channel=regular \
    --workload-pool=project_id.svc.id.goog \
    --scopes="https://www.googleapis.com/auth/cloud-platform"
kubectl crate namespace  dev 
 kubectl get nodes
 kubectl  get namespace  --all-namespace
kubectl  config  get-context

```
![Google kubernstes cluster](https://github.com/smogalloyubio/02-Devops-project-NetflixClone-app/blob/main/picture/Screenshot%202026-01-24%20at%2012.33.10.png)



---

### Step 5: Install and Configure Argo CD

Install Argo CD on GKE:

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl get pods - n argocd
#checl the  service of teh argocd
kubectl  get svc -n argocd
# chnage the  service orgocd to loadbalancer
kubectl  patch  svc/argocd-server -n argocd -p
# connect the git repo to argocd 
```

* Create Argo CD Application pointing to GitHub repo
* Enable auto-sync

![argocd cd](https://github.com/smogalloyubio/02-Devops-project-NetflixClone-app/blob/main/picture/Screenshot%202026-01-23%20at%2023.38.26.png)

---

### Step 6: Deploy Application via GitOps

* Push Kubernetes manifest changes to GitHub
* Argo CD automatically deploys to GKE

Verify deployment:

```bash
kubectl get pods -n namespace
kuebctl get all --all-namespace
kubectl get pods -n dev
#check  deployment of the  namespace
kubectl get pods -n  dev 

```
 **![ argocd  deployment](https://github.com/smogalloyubio/02-Devops-project-NetflixClone-app/blob/main/picture/Screenshot%202026-01-23%20at%2023.45.03.png):**



---

### Step 7: Install and Configure Velero

Install Velero with GCS backend:

```bash

# Download the latest Linux release
wget https://github.com/vmware-tanzu/velero/releases/download/v1.15.0/velero-v1.15.0-linux-amd64.tar.gz

# Extract the file
tar -xvf velero-v1.15.0-linux-amd64.tar.gz

# Move the binary to your path
sudo mv velero-v1.15.0-linux-amd64/velero /usr/local/bin/

# Verify installation
velero version --client-only
velero install \
  --provider gcp \
  --plugins velero/velero-plugin-for-gcp:v1.8.0 \
  --bucket <GCS_BUCKET_NAME> \
  --secret-file ./credentials-velero

```
**![velero backup](https://github.com/smogalloyubio/02-Devops-project-NetflixClone-app/blob/main/picture/Screenshot%202026-01-24%20at%2013.22.31.png):**


---

### Step 8: Backup Application Namespace

Create a namespace-scoped backup:

```bash
velero backup create netflix-backup --include-namespaces all-namespace
velero  backup  dscribe netflix-backup
velero  backup get

```

Check backup status:

```bash
velero backup get
```

![velero backup](https://github.com/smogalloyubio/02-Devops-project-NetflixClone-app/blob/main/picture/Screenshot%202026-01-24%20at%2013.21.54.png)



---

### Step 9: Disaster Simulation

* Delete the application namespace or entire cluster

```bash
kubectl delete namespace -all-namespace  dev  canary argocd
kubectl get namespace 
```

**![check  namespace](https://github.com/smogalloyubio/02-Devops-project-NetflixClone-app/blob/main/picture/Screenshot%202026-01-24%20at%2013.26.08.png):**

> Add screenshot showing namespace deletion

---

### Step 10: Restore from Backup

Restore the backup:

```bash
velero restore create --from-backup netflix-backup
```

Verify restore:

```bash
kubectl get all -n <APP_NAMESPACE>
```

**![retore namespace](https://github.com/smogalloyubio/02-Devops-project-NetflixClone-app/blob/main/picture/Screenshot%202026-01-24%20at%2013.36.05.png):**



---

## 📂 Repository Structure

```
.
├── terraform/          
├── argocd/              
├── .github/workflows/  
├── manifest/      
├── argocd/         
├── velero/             
└── README.md
```

---

## Disaster Recovery Test

* Created a Velero backup for the application namespace
* Deleted the namespace / cluster
* Restored from backup stored in GCS
* Verified:

  * Pods recreated successfully
  * Services accessible
  * Application functional after restore

---

## 🔐 Security Considerations

* IAM roles follow least-privilege principle
* Service accounts scoped per component
* No secrets stored in Git
* GitOps provides full audit trail

---

## 📈 Future Improvements

* Automated Velero backup schedules
* Terraform remote state in GCS
* Workload Identity for GKE
* Sealed Secrets for secret management
* Canary or blue/green deployments
* Monitoring with Prometheus and Grafana

---

## skills Demonstrated

* Kubernetes operations on GKE
* Infrastructure as Code with Terraform
* CI/CD pipeline design
* GitOps with Argo CD
* Disaster recovery planning and testing
* Cloud IAM and security best practices

---


**ubioworo rukevwe**
DevOps / Cloud Engineer

